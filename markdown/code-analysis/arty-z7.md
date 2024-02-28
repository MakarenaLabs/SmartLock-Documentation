# SmartLock example on Zynq7000

## Introduction

We will now paste the code of the example application of the SmartLock running on a Zynq7000 (in our case an Arty-7Z020 board) and explain how it works piece by piece.

```python
#!/usr/bin/env python
# coding: utf-8

import numpy as np
from driver import io_shape_dict
from driver_base import FINNExampleOverlay
import time
import cv2
import zmq
import json
import base64
import imageio
import io
import os

import musebox_pcl_bindings_feature_extractor as fe3d
import musebox_pcl_bindings_segmenter as ec

import argparse
from yunet import YuNet

mean = [0.6071, 0.4609, 0.3944]
std = [0.2457, 0.2175, 0.2129]

database_dict_list = []

# Load the cascade
face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')

bsize = 1
bitfile = "/home/xilinx/jupyter_notebooks/smart-lock/bitfile/smartlock.bit"
weights = "/home/xilinx/jupyter_notebooks/smart-lock/driver/runtime_weights/"
platform = "zynq-iodma"


driver = FINNExampleOverlay(
    bitfile_name=bitfile,
    platform=platform,
    io_shape_dict=io_shape_dict,
    batch_size=bsize,
    runtime_weight_dir=weights,
)

def run(image_input):
    global driver, mean, std
    image_input = image_input.astype(np.float32)
    image_input[:,:,0] /= 255
    image_input[:,:,1] /= 255
    image_input[:,:,2] /= 255
    image_input = cv2.resize(image_input, (140, 140))
    image_input[:,:,0] -= mean[0]
    image_input[:,:,1] -= mean[1]
    image_input[:,:,2] -= mean[2]

    image_input[:,:,0] /= std[0]
    image_input[:,:,1] /= std[1]
    image_input[:,:,2] /= std[2]
    image_input *= 255
    ibuf_normal = image_input.astype(np.uint8).reshape(driver.ibuf_packed_device[0].shape)
    driver.copy_input_data_to_device(ibuf_normal)
    driver.execute_on_buffers()
    obuf_normal = np.empty_like(driver.obuf_packed_device[0])
    driver.copy_output_data_from_device(obuf_normal)
    obuf_folded = driver.unpack_output(obuf_normal)
    final_output = driver.unfold_output(obuf_folded)
    return final_output

# ip 192.168.188.107
def init_sender(ip="tcp://*:5557"):
    global context, socket_pub
    ############## SEND #############
    socket_pub = context.socket(zmq.PUB)
    socket_pub.bind("tcp://*:5557")


context = zmq.Context()
socket = None
socket_pub = None

def init_listener(ip="tcp://192.168.188.60:5556"):
    global context, socket
    # CREATE COMMUNICATION LISTENER
    # context = zmq.Context()
    ############## RECEIVE #############
    socket = context.socket(zmq.SUB)
    socket.connect(ip)
    socket.setsockopt_string(zmq.SUBSCRIBE, str(''))


def get_data():
    global context, socket
    message = None
    i = 0
    while i < 1:
        try:
            zmq_response = socket.recv()
            # print("has received")
            message = json.loads(zmq_response)
            i = i + 1
        except Exception as e:
            print("Exception: ", e)
    return message


def send_trigger(trigger: str):
   global socket_pub
   
   message = {"trigger": trigger}
   socket_pub.send_string(json.dumps(message))


def init_database(folder: str):
    global database_dict_list
    for filename in os.listdir(folder):
        img = cv2.imread(os.path.join(folder, filename))
        features = run(img)
        if img is not None:
            database_dict_list.append({"features": features, "label": filename})

def init(ip_address: str):

    ip_address_ = "tcp://" + ip_address + ":5556"

    init_listener(ip_address_)

    init_sender()

    init_database("database")

def main_sl(ip_address: str, camera_index: int):

    init(ip_address)

    model = YuNet(modelPath="face_detection_yunet_2022mar.onnx",
                  inputSize=[320, 320],
                  confThreshold=0.9,
                  nmsThreshold=0.3,
                  topK=5000,
                  )
    try:
        cap.release()
    except:
        pass
    cap = cv2.VideoCapture(camera_index)
    cap.set(cv2.CAP_PROP_BUFFERSIZE, 1)

    pcl_similarity = 2500
    fr_similarity = 700

    aspect_ratio = (320/320, 320/320)

    print("Start Smart Lock Routine")

    while True:
        
        message_recv = get_data()
        _, img = cap.read()
        
        if img is None:
            print("I can't read from the camera!")
            continue

        result_segmenter = ec.euclidean_clustering(message_recv["point_cloud_camera"])
        res = fe3d.feature_extractor_3d(result_segmenter, message_recv["point_cloud_database"])
        
        if res > pcl_similarity:
            print("NOT A PERSON!")
            print(res)
            send_trigger("continue")
            continue
        
        sim_ = 99999
        person_ = ""
        try:
            image = img.copy()
            image = cv2.resize(image, (320, 320))
            img = cv2.resize(img, (320, 320))
            model.setInputSize([320, 320])
            result = model.infer(img)
            output = None
            for det in result:
                bbox = det[0:4].astype(np.int32)
                output = bbox
                break
            
            image = image[int(bbox[1]*aspect_ratio[1]):int((bbox[1]+bbox[3])*aspect_ratio[1]), int(bbox[0]*aspect_ratio[1]):int((bbox[0]+bbox[2])*aspect_ratio[1])]
            
            features = run(image)
            for elem in database_dict_list:
                sim_face = np.linalg.norm(features - elem["features"])

                if sim_face < fr_similarity:
                    if sim_face < sim_:
                        sim_ = sim_face
                        person_ = elem["label"]
            if sim_ != 99999:
                sim_ = 99999
                print("person is: ", person_)
            else:
                print("UNKOWN")

            send_trigger("continue")
        except:
            send_trigger("continue")
            pass


if __name__ == "__main__":
    # Create the parser
    parser = argparse.ArgumentParser()# Add an argument
    parser.add_argument('--ip', type=str, required=True)# Parse the argument
    parser.add_argument('--camera_index', type=int, required=True)# Parse the argument
    args = parser.parse_args()
    main_sl(args.ip, args.camera_index)
```

We now dig function per function and explain its functionality.

## Run Face Recognition

```python
def run(image_input):
    global driver, mean, std
    image_input = image_input.astype(np.float32)
    image_input[:,:,0] /= 255
    image_input[:,:,1] /= 255
    image_input[:,:,2] /= 255
    image_input = cv2.resize(image_input, (140, 140))
    image_input[:,:,0] -= mean[0]
    image_input[:,:,1] -= mean[1]
    image_input[:,:,2] -= mean[2]

    image_input[:,:,0] /= std[0]
    image_input[:,:,1] /= std[1]
    image_input[:,:,2] /= std[2]
    image_input *= 255
    ibuf_normal = image_input.astype(np.uint8).reshape(driver.ibuf_packed_device[0].shape)
    driver.copy_input_data_to_device(ibuf_normal)
    driver.execute_on_buffers()
    obuf_normal = np.empty_like(driver.obuf_packed_device[0])
    driver.copy_output_data_from_device(obuf_normal)
    obuf_folded = driver.unpack_output(obuf_normal)
    final_output = driver.unfold_output(obuf_folded)
    return final_output
```

This function takes in input a bounding box processed using the python bindings of OpenCV and return the features that describe it. The feature extraction is performed using our custom Face Recognition model. The image normalization is mandatory before inferring the model, but is taken care from the function.

## TCP connection with an external workstation

```python

def init_sender(ip="tcp://*:5557"):
    global context, socket_pub
    ############## SEND #############
    socket_pub = context.socket(zmq.PUB)
    socket_pub.bind("tcp://*:5557")


context = zmq.Context()
socket = None
socket_pub = None

def init_listener(ip="tcp://192.168.188.60:5556"):
    global context, socket
    # CREATE COMMUNICATION LISTENER
    # context = zmq.Context()
    ############## RECEIVE #############
    socket = context.socket(zmq.SUB)
    socket.connect(ip)
    socket.setsockopt_string(zmq.SUBSCRIBE, str(''))


def get_data():
    global context, socket
    message = None
    i = 0
    while i < 1:
        try:
            zmq_response = socket.recv()
            # print("has received")
            message = json.loads(zmq_response)
            i = i + 1
        except Exception as e:
            print("Exception: ", e)
    return message


def send_trigger(trigger: str):
   global socket_pub
   
   message = {"trigger": trigger}
   socket_pub.send_string(json.dumps(message))

```

This group of functions creates a publisher and a subscriber, based on the ZMQ library and the TCP protocol, used to create a double communication channel between the host pc and the Zynq7000. The functions `init_listener` and `init_sender` initialize the subscriber and the publisher respectively. Both take as argument the respective IP address publisher/subscriber (or rather the host PC and the Zynq7000). For the publisher we will adopt the technique of broadcasting the messages to every subscriber listening to a specific port, so no particular IP address would be required to use this function. `send_trigger` is used to sync the read/write mechanism between the host pc and the Zynq7000. The host PC listens to the Arty, and sends a message every time a trigger is sent from the Zynq7000 using this function.

## Load the photos to be recognized

```python
def init_database(folder: str):
    global database_dict_list
    for filename in os.listdir(folder):
        img = cv2.imread(os.path.join(folder, filename))
        features = run(img)
        if img is not None:
            database_dict_list.append({"features": features, "label": filename})
```
    
This function loads the photos to be recognized and extracts a vocabulary of features and a label out of it. The features will be used in the Face Recognition process, and the label is used to assign a name if a person is recognized in the process.

## Main Loop

```python

def main_sl(ip_address: str, camera_index: int):

    init(ip_address)

    model = YuNet(modelPath="face_detection_yunet_2022mar.onnx",
                  inputSize=[320, 320],
                  confThreshold=0.9,
                  nmsThreshold=0.3,
                  topK=5000,
                  )
    try:
        cap.release()
    except:
        pass
    cap = cv2.VideoCapture(camera_index)
    cap.set(cv2.CAP_PROP_BUFFERSIZE, 1)

    pcl_similarity = 2500
    fr_similarity = 700

    aspect_ratio = (320/320, 320/320)

    print("Start Smart Lock Routine")

    while True:

        message_recv = get_data()
        _, img = cap.read()

        if img is None:
            print("I can't read from the camera!")
            continue

        result_segmenter = ec.euclidean_clustering(message_recv["point_cloud_camera"])
        res = fe3d.feature_extractor_3d(result_segmenter, message_recv["point_cloud_database"])

        if res > pcl_similarity:
            print("NOT A PERSON!")
            print(res)
            send_trigger("continue")
            continue

        sim_ = 99999
        person_ = ""
        try:
            image = img.copy()
            image = cv2.resize(image, (320, 320))
            img = cv2.resize(img, (320, 320))
            model.setInputSize([320, 320])
            result = model.infer(img)
            output = None
            for det in result:
                bbox = det[0:4].astype(np.int32)
                output = bbox
                break

            image = image[int(bbox[1]*aspect_ratio[1]):int((bbox[1]+bbox[3])*aspect_ratio[1]), int(bbox[0]*aspect_ratio[1]):int((bbox[0]+bbox[2])*aspect_ratio[1])]

            features = run(image)
            for elem in database_dict_list:
                sim_face = np.linalg.norm(features - elem["features"])

                if sim_face < fr_similarity:
                    if sim_face < sim_:
                        sim_ = sim_face
                        person_ = elem["label"]
            if sim_ != 99999:
                sim_ = 99999
                print("person is: ", person_)
            else:
                print("UNKOWN")

            send_trigger("continue")
        except:
            send_trigger("continue")
            pass

```

In the main loop we put together all the functions to create our desired flow. 

First the publisher, subscriber and vocabulary of people are initialized through the function `init`. 

```python
init(ip_address)
```

Secondly, we load the model of Face Detection in the variable `model`.

```python
model = YuNet(modelPath="face_detection_yunet_2022mar.onnx",
          inputSize=[320, 320],
          confThreshold=0.9,
          nmsThreshold=0.3,
          topK=5000,
          )
```

We initialize the camera using the variable `cap`. This is our object which grabs the 2D frames.

```python
try:
	cap.release()
except:
	pass
cap = cv2.VideoCapture(camera_index)
cap.set(cv2.CAP_PROP_BUFFERSIZE, 1)
```

We set our similarity threshold values for Face Recognition (`fr_similarity`) and 3D Person Detection (`pcl_similarity`). The lower those values are the more robust the system is, but with less probability to give an output.

```python
pcl_similarity = 2500
fr_similarity = 700
```

We receive the messages from the publisher of the 3D acquisition and grab simultaneously a frame from the 2D camera:
    
```python
message_recv = get_data()
_, img = cap.read()

if img is None:
    print("I can't read from the camera!")
    continue
```    

We check that there is actually a face in front of the camera.
    
```python
result_segmenter = ec.euclidean_clustering(message_recv["point_cloud_camera"])
res = fe3d.feature_extractor_3d(result_segmenter, message_recv["point_cloud_database"])

if res > pcl_similarity:
	print("NOT A PERSON!")
	print(res)
	send_trigger("continue")
	continue
```

If there is a person, we start the 2D branch, by firstly detecting and cropping a face.

```python
image = img.copy()
image = cv2.resize(image, (320, 320))
img = cv2.resize(img, (320, 320))
model.setInputSize([320, 320])
result = model.infer(img)
output = None
for det in result:
	bbox = det[0:4].astype(np.int32)
	output = bbox
	break

image = image[int(bbox[1]*aspect_ratio[1]):int((bbox[1]+bbox[3])*aspect_ratio[1]), int(bbox[0]*aspect_ratio[1]):int((bbox[0]+bbox[2])*aspect_ratio[1])]

```

We perform Face Recognition:

```python
features = run(image)
```

And then we check if the person is present into the database, and if so we assign a label to the most likely similar person, otherwise we restart the loop:

```python
    for elem in database_dict_list:
        sim_face = np.linalg.norm(features - elem["features"])

        if sim_face < fr_similarity:
            if sim_face < sim_:
                sim_ = sim_face
                person_ = elem["label"]
    if sim_ != 99999:
        sim_ = 99999
        print("person is: ", person_)
    else:
        print("UNKOWN")
```





