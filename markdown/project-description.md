# SmartLock Design Reference

## Introduction

The Machine Learning solution uses a combination of 2D and 3D information processing to identify a specific human who wishes to access a restricted area.

## Architectural Schematic

The base architecture of the SmartLock is briefly described in following image:

<div style="width: 100%;">
    <img src="../images/arch_struct.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="SmartLock Design Reference Architecture">
    <p style="text-align: center; font-style: italic">Fig. 1 - SmartLock Design Reference Architecture</p>
</div>

The system is based on a Machine Learning engine which uses AI models to extract features from both the 2D and 3D acquisition data. 
Both branches (2D and 3D) have their own dedicated AI model for feature extraction. The extracted information is then fused together in order to compute the authorization process. Below is a brief description of the two branches.

## 2D Branch

This branch computes two different AI models in a cascade mode, first Face Detection and then Face Recognition. 
Face Detection is an AI model which analyzes an image from a video source. From this image are extracted, if present, the bounding boxes of the faces. The bounding boxes are a cropped version of the original image, containing only the faces, if present.
Face Recognition is an AI model which receives the bounding box-cropped image of a face as input and returns a description of the face as output. The Face Recognition AI model is used to determine if the person in front of the SmartLock can be identified as someone who has been saved in the database as an authorized user.

## 2D APIs

### Introduction

Those APIs are used to recognize the person who is standing in front of the lock. Those APIs are based on server-client modality which consists in asking a particular machine learning task. Those APIs are based on MuseBox. MuseBox is a Machine Learning framework developed by MakarenaLabs to infer AI models on FPGAs. 

The 2D branch of the SmartLock process performs two operations: 

1. Face Detection
2. Face Recognition

Face Detection consists in understanding whether a human face is present in a camera frame. If so, the frame is then cropped to the face. Face Recognition consists in "describing" the cropped face obtained before. The result of the description is used to label whether the person looking at the camera is present in the database of authorized faces.

### General Musebox Structure for AI/ML Tasks related to Face Analysis

The MuseBox Server listens in localhost (127.0.0.1) at port 9696.
In ZMQ socket binding, the Client will bind at this address to send data to MuseBox Server: `tcp://*:9696`

All the APIs are in JSON format, and have the same structure. The structure is the following:

```
{
    "topic": <string>,
    "only_face": <bool>,
    "image"/"input": <base64 image>/double array,
    "publisherQueue": <string>
}
```
Where:

- `topic`: is the name of the Machine Learning task
- `image`: is the OpenCV image in base64 format (string encoded in UTF8)
- `publisherQueue`: is the address where the Client expects to receive the response
- `only_face`: (optional) if you want to execute only the selected task and not the dependent tasks. For example: if you want to execute the face recognition task, you need to execute before that the face detection for extracting the sub-image that contains the face of the person that you want to recognize; with `only_face: true` you are sure that the passed image contains only the cropped face, meanwhile without the field `only_face`, MuseBox Server executes face detection and face recognition.

### Face Detection

It detects the faces in a scene up to 32×32 pixel.

Request from Client:
```
"topic": "FaceDetection"
"image": "base64 image"
```
Response from Server:
```
{
    "data":
        [
            {
                "boundingBox":
                    {
                        "height": <int>,
                        "width": <int>,
                        "x": <int>,
                        "y": <int>
                    }
            }
        ],
}
```
- `boundingBox` is the bounding boxes of the task, according to OpenCV format (from top-left to bottom-right)

### Face Recognition

Given a face bounding box and a database of faces, it recognizes a face in a scene.

Request from Client:
```
"topic": "FaceRecognition"
"image": "base64 image"
"only_face"?: true
```

Response from Server:
```
{
    "data":
        {
            "prediction": <string>
        }
}
```
- `prediction` corresponds to the name of the person

### Face Landmark

Given a face bounding box, it extracts the 98 relevant points of a face.

Request from Client:
```json
"topic": "FaceLandmark"
"image": "base64 image"
"only_face"?: true
```

Response from Server:
```json
{
    "data":
        {
            "prediction": [196 float]
        }
}
```
 - `prediction` corresponds to (x,y) couples for face landmark points


## 3D Branch

This branch is used to avoid efforts to spoof the 2D system. The function of this branch is to determine whether the 3D mapping of the person in front of the SmartLock is an human face. This rejects efforts to use images, photos, or other methods which can otherwise be accepted and recognized by the 2D system.

## 3D APIs

### Introduction
As seen in the introduction, the architecture is divided in two branches. One of the two branches consists of the analysis and feature extraction of the 3D information of the system. The 3D information is extremely useful to understand the geometry and composition of a real 3D object. We exploit these properties to analyze whether a face lies in front of the camera. Why do we need to add this step? If we have only the 2D branch and our photo saved in the database, someone who wants to defeat the lock could simply show the camera a photo of an authorized individual (e.g., taken from a social network) and enter. Having a branch which analyzes that a real human face is in front of the camera prevents such spoofing techniques.

To do this process we use a representation of the 3D information called Point Cloud. This is a data format which creates a cloud of points of the object that we want to represent. An example could be found in the below image:

<div style="width: 100%;">
    <img src="../images/teapot.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="Tea pot pc">
    <p style="text-align: center; font-style: italic">Fig. 1 - Example of Point Cloud</p>
</div>

In our setup we need 3 types of APIs to manipulate the Point Cloud obtained by the depth sensor. Those APIs are based on the BSD-licensed [Point Cloud Library](https://pointclouds.org/).

The first type of API is a point cloud downsampler, which reduces the computational effort, of subsequent steps. The downsampler reduces the number of points in the cloud while maintaining its geometrical properties (so a face remains a face). With less points it is easier to manipulate the Point Cloud and extract information and features out of it. We will refer to it as **Downsampler**.

The second type of API which we need is something which can extract different components out of a Point Cloud. Extracting this subgroup s of points is a process called segmentation and is used to isolate different object in a Point Cloud. This process is carried out by grouping points that are close one to each other and discard the ones that are far away, given a certain distance threshold. We will use the **Segmenter** to isolate the face of the person from the background points obtained by the acquisition of the 3D sensor. We will suppose in our system the face to be in front of the camera. We refer to the object which does this operation as **Segmenter**.

The third type of API is one which extracts features out of the Point Cloud. We refer to this as **Feature Extractor 3D**. This type of API is used to compute a vector of descriptors of our Point Cloud. We are going to compare these vectors between Point Clouds to see how much two Point Clouds are similar. In our case, we will categorize if the Point Cloud is sufficiently similar to a human face before proceeding with the unlocking process.

The basic algorithm for the 3D branch can be then summarized in these three steps:

1. Check if the Point Cloud has too many points, and if so, downsample using the **Downsampler**.
2. Isolate the face from point cloud, using the **Segmenter**.
3. Extract the features out of the Point Cloud and check how close they resemble a face using the **3D Feature Extractor**.
 

### Class name: `Downsampler`

This object is used to voxelize the point cloud. The voxelization process results in a less dense 3D representation, where the points are grouped into voxels. The voxels are computed by group of points that are close in space. In our intent we will represent more points with a single voxel, thus resulting in an actual downsampling operation for the Point Cloud. The voxels are computed by grouping points in "leaves" (can be seen as cubes) with modifiable X-Y-Z axis value to enlarge/stretch them.

```c++
downsampler(float leaf_size_x, float leaf_size_y, float leaf_size_z);
```

Constructor of the class

- **Function params**:
	- `leaf_size_x`: Size on the X axis of the voxel
	- `leaf_size_y`: Size on the Y axis of the voxel
	- `leaf_size_z`: Size on the Z axis of the voxel
	
```c++
pcl::PointCloud<pcl::PointXYZ> compute_downsampling(pcl::PointCloud<pcl::PointXYZ>::Ptr cloud);
```

Computes the voxelization on the point cloud and returns the downsampled cloud. 

- **Function params**:
	- `cloud`: pointer to the point cloud we want to downsample.


```c++
void set_leaf_size_x(float x);
```

Set the value of the X axis of the voxel. Overwrites what has been set during the construction of the object.

- **Function params**:
	- `x`: pointer to the point cloud we want to downsample.

```c++
void set_leaf_size_y(float y);
```

Set the value of the Y axis of the voxel. Overwrites what has been set during the construction of the object.

- **Function params**:
	- `y`: pointer to the point cloud we want to downsample.

```c++
void set_leaf_size_z(float z);
```

Set the value of the Z axis of the voxel. Overwrites what has been set during the construction of the object.

- **Function params**:
	- `z`: pointer to the point cloud we want to downsample.


### Class name: `Segmenter`

This class implements a Euclidean Segmenter. This Segmenter divides the Point Cloud chosen into subgroups. The metric chosen to create the subgroups is the Euclidean distance. Out of this subgroups we will return just the biggest. The logic behind this choice is that the face is placed in front of the camera, and thus most of the points represent the face itself. If other points are present in the background they are not part of the face and can be discarded.

```c++
segmenter();
```

Constructor of the class

```c++
pcl::PointCloud<pcl::PointXYZ> clustering(pcl::PointCloud<pcl::PointXYZ>::Ptr cloud);
```

This methods accepts in input a Point Cloud, segments it and return the biggest of the subgroup created.

- **Function params**:
	- `cloud`: The Point Cloud to segment.


On the Zynq7000, the Python version of this API is available, and not the C++ implementation. Reported below is the signature, as the name of the functions and their application do not diverge with respect to what has been already explained.

To import the API just tap:

```python
import musebox_pcl_bindings_segmenter as ec
```

And then, to use it tap:

```python
ec.euclidean_clustering(point_cloud)
```

- **Functions Params**:
	- `point_cloud`: A Nx3 shaped numpy array which represents our Point Cloud

### Class name: `Feature Extractor 3D`

This object is used to compute the descriptors of the point cloud. The descriptors are dependent on the geometry of the point cloud itself. In our application this results in checking the resemblance of this group of points with a face.

```c++
feature_extractor_3d(metrics feature_comparison_metric);
```

Constructor of the class

- **Function params**:
	- `feature_comparison_metric`: How the loss of the features extracted is computed. Only the L2 loss is supported.

```c++
std::tuple< float, pcl::PointCloud<pcl::VFHSignature308> > get_features(pcl::PointCloud<pcl::PointXYZ>::Ptr cloud1, pcl::PointCloud<pcl::PointXYZ>::Ptr cloud2);
```

Constructor of the class

- **Function params**:
	- `cloud1`: First Point Cloud in input. In this case, it represents the features of the face in front of the 3D camera;
	- `cloud2`: Cloud to be compared with cloud1.


On the Zynq7000, the Python version of this API is available, and not the C++ implementation. Reported below is the signature, as the name of the functions and their application do not diverge with respect to what has been already explained.

To import the API just tap:

```python
import musebox_pcl_bindings_feature_extractor as fe3d
```

And then, to use it tap:

```python
fe3d.feature_extractor_3d(segmented_point_cloud, face_point_cloud)
```

- **Functions Params**:
	- `segmented_point_cloud`: A Nx3 shaped numpy array which represents our Point Cloud (obtained by the segmenter) 
	- `face_point_cloud`: A Nx3 shaped numpy array which represents our face Point Cloud
