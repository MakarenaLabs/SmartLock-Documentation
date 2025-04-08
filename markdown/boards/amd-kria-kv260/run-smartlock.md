# Running the example on the AMD Kria&trade; KV260 board

Once you have completed the setup of the hardware parts, you can run the SmartLock Solution! In this chapter is
explained how to run the example on the AMD Kria&trade; KV260 board.

Before to run the SmartLock Core software, you have to populate the database of the users faces' features with the faces
you want to be recognized. Otherwise, the sofware still correctly running, but not recognizing anyone as expected.

## Populate Faces' Features Database

The first step is to add the features of your face to the database. You can do that using the `take-a-pic` tool.

### Execution

Before starting the execution, verify that the camera is correctly connected via USB port on the Kria&trade; board:

1. Open a Terminal searching the application on the Ubuntu drawer or press `Ctrl+Alt+T`;
2. Go to the SmartLock Core build directory with:

        cd smartlock-core-onsemi/build

3. Execute the tool with the command

        ./take-a-pic_v3.1.1

   and place yourself in front of the camera.

4. Follow the monitor tool instructions. You will have to press the `c` key to take a pic, or the `q` button to quit.

5. You should now see a new line on the terminal asking to insert the name of the file.

6. Please insert it _**without**_ the file extension (i.e. _guglielmo_, not _guglielmo.png_).

[//]: # (<div style="width: 100%; text-align: center">  )

[//]: # (   <img src="/images/tap-1person.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="TakeAPic with 1 Person">  )

[//]: # (   <p style="text-align: center; font-style: italic">Fig. 1 - TakeAPic with 1 person</p>  )

[//]: # (</div>  )

## Start demo software

Let's start with the run of the software.

1. Open a Terminal searching the application on the Ubuntu drawer or press `Ctrl+Alt+T` (if you are still in the
   previous terminal, jump to the step 3);
2. Go to the SmartLock Core build directory with:

        cd smartlock-core-onsemi/build

3. Run the `smartlock.exe` executable without any parameter. An example is:

        ./smartlock_v3.1.1

   Place your face in front of the camera in order to be detected and, once the face is recognized, the flow stucks and
   asks you to press the `r` button on the keyboard to restart the recognition flow. In this case, a virtual door is
   opened for you! Otherwise, the recognition flow still continue to run and it is waiting to recognize someone while
   the virtual door keeps closed.
