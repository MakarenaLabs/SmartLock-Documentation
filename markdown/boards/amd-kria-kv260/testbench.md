# Kria KV260 Testbench

This is a guide on how to validate the FaceRecognition AI model on the KV260 board.
To run the validation testbench you need to have:

- Kria KV260 board;
- Ubuntu 22.04 image (username: `ubuntu`, password: `makarenalabs`);
- MuseBox License [@MakarenaLabs](mailto:staff@makarenalabs.com);

The MuseBox License is really important, without it you won't be able to run the testbench. In order to obtain the license, you need to request the node locker license.
The license is valid for a specific board, so you cannot use the license on a different node.
For the license request, you need to create the license request file. To do that, simply run this command:
```bash
sudo license_request
```
This command generates the file `license_request.req` in the path `/usr/local`. You need to send to us the file via email at [staff@makarenalabs.com](mailto:staff@makarenalabs.com) with the subject *MuseBox License request* and with the message body *I accept your evaluation license agreement*. Our internal system will check if the email is associated to a valid customer, then, according to your signed contract, the system will respond to you with the license file, called `license.lic`. When you receive the license file, you need to place the file in this path: `/usr/local/bin`.

Once you have your license, you should go to a browser and go to `http://<kria_board_ip>:9090`. The password to insert is `xilinx`. Go inside the MuseBox folder and open `validate_face_recognition.ipynb`. Now you can run all the cells and run the testbench.
When you run the first cell:
```python
import cv2
import os
import numpy as np
from FaceRecognitionSL import FaceRecognition
```
you should see as output:
```python
Checking license...
License ok
```

On the other hand, if you get any error and the kernel seems to die, it may be an error with the license.

You can change the maximum length of the images to run the validation on, by changing the variable:
```python
max_len_images = 1000
```
and you can change the Face Recognition similarity threshold by changing the variable:
```python
threshold = 0.12
```