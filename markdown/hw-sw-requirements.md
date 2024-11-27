# Hardware & Software requirements

The SmartLock solution uses different requirements, both software and hardware. In this section all these needs will be
explained.

## Hardware requirements

### AMD Kria&trade; KV260 Board

The SmartLock solution proposed here was built on the AMD Kria&trade; KV260 board:

- **Kria&trade; KV260**: The Kria&trade; KV260 is built for advanced vision application development without requiring
  complex hardware design knowledge. For more information, please visit
  the [official website](https://www.xilinx.com/products/som/kria/kv260-vision-starter-kit.html);

   <div style="width: 100%;">
       <img src="https://www.xilinx.com/content/dam/xilinx/imgs/products/som/som-kv260-4.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="Kria KV260">
       <p style="text-align: center; font-style: italic">Fig. 1 - Kria&trade; KV260</p>
   </div>

### RGB-IR Camera

The architecture of SmartLock solution is based on a RGB-IR camera provided by [onsemi](https://www.onsemi.com/) and a
VCSEL dot projector provided by [ams OSRAM](https://ams-osram.com/).
The dot projector is
a [VCSEL (Vertical Cavity Surface Emitting Laser) projector](https://en.wikipedia.org/wiki/Vertical-cavity_surface-emitting_laser)
and is needed to support the reconstruction of the real human face:

- [onsemi Demo3 Kit](https://www.onsemi.com/design/tools-software/evaluation-board/AR0830CSSH11SMKAH3-GEVB) with its
  own [AR0830CS RGB-IR sensor](https://www.onsemi.com/products/sensors/image-sensors/AR0830);
- [ams OSRAM BELAGO1.1 VCSEL dot projector](https://ams-osram.com/products/lasers/ir-lasers-vcsel/ams-belago1-1-dot-projector)
  with its driver
  board ([ams OSRAM AS1170](https://ams-osram.com/products/drivers/led-drivers/ams-as1170-2-channel-led-and-vcsel-driver-ic));

  <table>
       <tr>
           <td>
               <div style="width: 100%;">
                <img src="/images/AR0830CS.jpg" style="display: block; margin-left: auto; margin-right: auto;" alt="onsemi AR0830CS RGB-IR sensor">
              </div>
           </td>
           <td>
               <div style="width: 100%;">
                <img src="/images/belago-1.1.jpg" style="display: block; margin-left: auto; margin-right: auto; width: 80%;" alt="onsemi AR0830CS RGB-IR sensor">
               </div>
           </td>
       </tr>
       <tr>
           <td style="text-align: center; font-style: italic">
               <p style="text-align: center; font-style: italic">Fig. 2 - onsemi AR0830CS RGB-IR sensor</p>
           </td>
           <td style="text-align: center; font-style: italic">
               <p style="text-align: center; font-style: italic">Fig. 3 - ams OSRAM BELAGO1.1 dot projector</p>
           </td>
       </tr>
   </table>

[//]: # (https://www.digikey.com/en/products/detail/onsemi/AR0830CSSH11SMKAH3-GEVB/22161867)

### Miscellaneous

Some other miscellaneous are required:

- **MicroSD card**: it must be _greater than or equal to_ 32GB;
- **MicroSD to USB adapter**: to burn the image;
- **Full-HD HDMI Monitor and HDMI cable**: to set up and start up run the SmartLock solution;
- **Keyboard and mouse kit**: to run the commands into the AMD Kria&trade; KV260 board;

The whole hardware setup should be very similar to the image below:

   <div style="width: 100%; margin-bottom: 20px;">
     <img src="/images/whole-setup.JPEG" style="display: block; margin-left: auto; margin-right: auto;" alt="onsemi AR0830CS RGB-IR sensor">
   </div>

1. [onsemi Demo3 Kit](https://www.onsemi.com/design/tools-software/evaluation-board/AR0830CSSH11SMKAH3-GEVB)
   with [AR0830CS RGB-IR sensor](https://www.onsemi.com/products/sensors/image-sensors/AR0830) mounted aboard;
2. A stable tripod to install the Demo3 Kit (one of your choice is ok, this model is
   the [Neewer T160A](https://www.amazon.com/Neewer-Stabilizer-Lightweight-Smartphones-Camcorders/dp/B074GMW9WY));
3. USB 3.0 cable provided with the Demo3 Kit;
4. An AMD Kria&trade; KV260 board with its microSD card;
5. HDMI cable to connect the AMD Kria&trade; KV260 board to your FullHD monitor;
6. [ams OSRAM BELAGO1.1 VCSEL dot projector](https://ams-osram.com/products/lasers/ir-lasers-vcsel/ams-belago1-1-dot-projector)
   with its driver
   board ([ams OSRAM AS1170](https://ams-osram.com/products/drivers/led-drivers/ams-as1170-2-channel-led-and-vcsel-driver-ic)).
   Please note that the component in the photo above is in a hardware development state, the sample you have should be
   different;

The photo does not include the screen and the keyboard and mouse kit to control the AMD Kria&trade; KV260 board;

## Software requirements

There are some software that need to be installed into your PC in order to run the solution successfully. First of all,
you need to have at least 27GB free on your disk in order to download the image to be burnt (it is more or less 19GB
big). To burn the image, you need:

- [**7Zip**](https://www.7-zip.org/download.html): to unarchive the downloaded image files;
- [**Balena Etcher**](https://etcher.balena.io/): to burn the image. You can use one of your choice, but this one is
  strongly recommended;

### OS Image Request

At this point, you have to download the latest release of the image to burn into the SD card of the AMD Kria&trade;
KV260 board. To do so, please [send a request email](mailto:staff@makarenalabs.com?subject=[SMARTLOCK]
SmartLock v3.1.1 OS image request) to MakarenaLabs. Be sure to have at least 27GB of free space in your PC in order to
download all the files.
