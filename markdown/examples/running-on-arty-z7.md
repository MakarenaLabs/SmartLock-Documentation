# Running the example on the Arty Z7-20 board

<div style="width: 100%;">
        <img src="https://www.xilinx.com/content/dam/xilinx/imgs/prime/IMG_6224%20-%20ARTY-Z7%20-%20Obl%20-%20600.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="Arty Z7-20">
        <p style="text-align: center; font-style: italic">Fig. 1 - Arty Z7-20</p>
</div>

## Setting up the board

The demo, in addition to the miscellaneous listed in the [How to run the demo](how-to-run.md) page, is composed by:

1. Zynq7000 board ([Arty Z7-20 Board](https://www.xilinx.com/products/boards-and-kits/1-pdb0q2.html), using the Zynq7020);
2. Linux Workstation;
3. MakarenaLabs evaluation image file for Arty (please write to [staff@makarenalabs.com](mailto:staff@makarenalabs.com) to get the access);
4. A MicroSD card _greater than or equal to_ 16GB;

## Image File Installation

1. Download the MakarenaLabs image and, if it is necessary, merge the downloaded image files into a single image file using 7Zip.
2. Verify the SHA-256 hash of the image file. This will ensure that no errors occurred in the transmission and merge of the files. If your system is Windows 11:
    1. Click _Search_ and run `powershell`. (Note: `cmd` will not work).
    2. Navigate to the directory containing the image file.
    3. Enter `Get-FileHash <filename>`.
    4. Wait 15 to 30 seconds.
    5. The hash value will appear.

3. Flash the MakarenaLabs image file to the microSD card with Balena Etcher (or another similar one). Please note that the image must be written to the microSD card as a boot.image.

Next, connect the Arty to your router with an Ethernet cable. The board is configured to boot with DHCP, thus it should be dynamically assigned an IP by your router. Determine the IP addresses of your Arty board and also your workstation, using your router's management software. Some routers make the software available by typing `192.168.1.1` in your web browser. If you the router's management software is not accessible or available, it has also a static IP address (`192.168.2.99`) you can use to connect to. In this case, you have to be in the same subnetwork of the Arty Z7, so set your network adapter properly. The addresess are needed to run the demo. To power the Arty simply connect a micro-USB cable from the board to your PC or to a USB power adapter.

## Populate Faces Database

1. Connect the 2D USB webcam to the Arty z7, and the 3D camera to a USB 3.0 port of your PC.

2. The first step to run the demo (optional, but strongly suggested for testing) is to add your face to the database.

3. Check the Arty z7 IP address, then open your browser and tap `[ip address of board]:9090` (i.e. `192.168.2.99:9090`). This will open a Jupyter Notebooks environment.

4. Navigate to the folder `smart-lock/driver`. Here you will find a notebook called `takeAPic`, open it.

5. Place in front of the USB camera of the setup, and then tap three times `Shift+Enter` to executes the three cells of the notebook.

6. Stay in front of the camera for 10-20s, as the startup of the Notebook requires some time.

7. After taking the pic the Notebook will ask for the filename to save in the database, then press enter to save it.

8. Write the name but do not insert any extension in the name (i.e. _guglielmo_ and not _guglielmo.jpg_).


At this point check inside the folder named `database` to verify that the photo of your face exists with the name you assigned to it. At this point you can close the Web Tab.

## Start recognition process

1. Open a terminal and connect to the board via SSH. To do so you need to tap into the terminal:

        ssh xilinx@[ip address of Arty]

    It will ask you the password. The password is `xilinx`.

2. To navigate to the folder where the script is placed, execute:

        cd jupyter_notebooks/smart-lock/driver

3. To start the routine, you must run the next step with sudo privileges. So:
   
        sudo /usr/local/share/pynq-venv/bin/python smart_lock_arty.py --ip [your_workstation's_IP] --camera_index -1

    You must set the IP address of your workstation and the camera index value, which is `-1` so the OS is able to choose the camera index independently.
    This will start the routine of smart lock which is now waiting to have input fed from your PC. The python script is ready when the log _Start Smart Lock Routine_ appears.

## Start 3D software on your Workstation

First of all, you must configure the tool inside your workstation. Only the steps for a Linux environment are provided, because the Windows version is not currently available.

### Linux Version

Go to the path where you have downloaded the libroyal SDK provided by MakarenaLabs (please contact us at staff@makarenalabs.com if you do not have the SDK) and go to the build folder:

```bash
cd [/path/to/your/libroyale]/samples/to_arty_7z/build
```

Please be sure to have these prerequisites to continue (we are currently refer to an Ubuntu distro, please check the right commands for your platform):

 - [OpenCV](https://opencv.org/)
 - [Point Cloud Libraries](https://pointclouds.org/)
 - [ZeroMQ](https://zeromq.org/)

You can install them running:

```bash
sudo apt update
sudo apt install -y libopencv-dev libpcl-dev libzmq3-dev
```

If all the requirements are met, then you can proceed to configure the tool with:

```bash
cmake ..
make -j8
```

At this time, you are able to start the 3D software in your workstation. So, you can start the 3D software running

```bash
./smart_lock_sender 192.168.2.99
```

Now the routine of SmartLock should start and print out status messages to Arty console which describes what is happening and whether the Arty and Workstation are exchanging messages.

### Windows Version
Currently not supported
