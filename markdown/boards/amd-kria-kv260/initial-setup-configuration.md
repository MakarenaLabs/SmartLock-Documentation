# Initial setup configuration

In this chapter is explained how to setup the hardware to run the example on the AMD Kria&trade; KV260 board. There are
some preliminary steps
in order to have the setup completely up and running, that they are described in the section below.

<div style="width: 100%;">
    <img src="https://www.xilinx.com/content/dam/xilinx/imgs/products/som/som-kv260-4.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="Kria KV260">
    <br>
    <p style="text-align: center; font-style: italic">Fig. 1 - AMD Kria&trade; KV260</p>
</div>

## SmartLock Kit
If you received the SmartLock kit, please follow this section. Otherwise, please go ahead with the [Preliminary actions](#preliminary-actions) chapter.

Your kit is composed by:

- Black 3D printed SmartLock box;
- AMD Kria&trade; KV260 Development Kit with 32GB microSD card. It is already flashed with the new version of the SmartLock Ubuntu Image (v3.2.0);
- AMD Development Tools Basic Accessory for the Kria&trade; KV260;

### 3D SmartLock Box Unboxing
The video below is showing you the content of the box you received. As in the packaging list paper, you will find:

 - A mini tripod stand in the black bag;
 - An USB 3.0 cable to connect the onsemi Demo 3 kit to the AMD Kria&trade; KV260 board;
 - An Type-A Type-A USB cable to power on the dot projector (VCSEL);
 - The black box of the SmartLock (3D printed and designed by MakarenaLabs), which contains:
    - [onsemi Demo 3 Board (AGB1N0CS-GEVVK)](https://www.onsemi.com/design/evaluation-board/AGB1N0CS-GEVK);
    - [onsemi Optical Sensor Dvelopment Tools 8MP (AR0830CSSH11SMKAH3-GEVB)](https://www.onsemi.com/design/evaluation-board/AR0830CSSH11SMKAH3-GEVB);
    - [ams osram BELAGO1.1 Dot Projector](https://ams-osram.com/products/lasers/ir-lasers-vcsel/ams-belago1-1-dot-projector);
    - [ams osram AS1170 Dev Kit (AS1170_WL_EVM_EB)](https://ams-osram.com/products/drivers/led-drivers/ams-as1170-2-channel-led-and-vcsel-driver-ic);

<div style="position: relative; width: 100%; height: 0; padding-bottom: 50%; margin-bottom: 50px;">
        <iframe src="https://www.youtube.com/embed/rJ-al2hD7MY"
        frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
</div>

<div style="background-color:rgba(180, 180, 0, 30%); vertical-align: middle; padding:20px 0;border-radius: 10px">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Please be sure to remove the protective transparent cap from the 2D sensor.
</div>

<br>

As said before, the AMD Kria&trade; KV260 is already equipped with the flashed microSD card with SmartLock Ubuntu Image v3.2.0, so you can jump to [Board connections](#board-connection) chapter.

## Preliminary actions

At this step, you should have completed all the requirements provided in the
section [Hardware & Software requirements](/hw-sw-requirements).
If so, you should have this setup:

- An AMD Kria&trade; KV260 board with the flashed SD card using the image provided
  by [MakarenaLabs](https://www.makarenalabs.com/);
- The Demo 3 Development Kit provided by onsemi with the AR0830CS RGB-IR sensor installed on board and its own USB 3.0
  cable;
- The dot projector in the right position as near as possible to the 2D sensor;
- Your FullHD monitor and the keyboard/mouse kit connected to the AMD Kria&trade; KV260;

### Image File Installation

Install the OS following these steps:

#### Windows version

1. Download the MakarenaLabs image. They are some files to be merged together with 7Zip;
2. Merge the downloaded image files into a single image file;
3. Verify the SHA-256 hash of the image file. This will ensure that no errors occurred in the transmission and merge of
   the files:
    1. Click _Search_ and run `powershell`. (Note: `cmd` will not work);
    2. Navigate to the directory containing the image file;
    3. Enter `Get-FileHash <filename>`;
    4. Wait 15 to 30 seconds;
    5. The hash value will appear;
4. Flash the MakarenaLabs image file to the microSD card with Balena Etcher. It must be flashed as a boot image;

#### Linux version

1. Download the MakarenaLabs image. They are some files to be merged together with 7Zip;
2. Merge the downloaded image files into a single image file;
3. Verify the SHA-256 hash of the image file. This will ensure that no errors occurred in the transmission and merge of
   the files:
    1. Open the terminal searching on the Linux drawer `Terminal` or typing `Ctrl+Alt+T`;
    2. Navigate to the directory containing the image file;
    3. Enter `sha256sum <filename>`;
    4. Wait 15 to 30 seconds;
    5. The hash value will appear;
4. Flash the MakarenaLabs image file to the microSD card with Balena Etcher. It must be flashed as a boot image;

You can check here the integrity of the image file writing in the field the hash value you got in the step `3.5` of the
previous list:

  <style>
      .container {
          display: flex;
          align-items: center;
          gap: 10px;
          max-width: 100%;
      }
      input[type="text"] {
          flex: 1;
          padding: 8px;
          font-size: 14px;
          border: 1px solid #ccc;
          border-radius: 4px;
      }
      button {
          padding: 8px 12px;
          font-size: 14px;
          border: none;
          border-radius: 4px;
          background-color: #007BFF;
          color: white;
          cursor: pointer;
      }
      button:disabled {
          background-color: #ccc;
          cursor: not-allowed;
      }
      button:hover:not(:disabled) {
          background-color: #0056b3;
      }
      .result {
          margin-top: 20px;
          font-weight: bold;
      }
  </style>

  <div class="container">
      <input type="text" id="checksumInput" placeholder="Enter checksum">
      <button id="validateButton" onclick="validateChecksum()" disabled>Validate</button>
  </div>
  <div class="result" id="result"></div>

  <script>
      const checksumInput = document.getElementById('checksumInput');
      const validateButton = document.getElementById('validateButton');
      const resultDiv = document.getElementById('result');
  
      // Enable/disable button based on input field content
      checksumInput.addEventListener('input', () => {
          validateButton.disabled = checksumInput.value.trim() === '';
          resultDiv.textContent = '';
      });
  
      function validateChecksum() {
          const inputValue = checksumInput.value.trim();
  
          // Example: Replace with your checksum validation logic
          const expectedChecksum = "12345abcd"; // Replace with the actual expected checksum
  
          if (inputValue === expectedChecksum) {
              resultDiv.textContent = "Checksum is valid!";
              resultDiv.style.color = "green";
          } else {
              resultDiv.textContent = "Checksum is invalid!";
              resultDiv.style.color = "red";
          }
      }
  </script>

If the checksum validation is passed, the image is successfully flashed, and you can continue with the next steps.

## Board connection

Now, you are ready to start up the board. To do so, insert the newly flashed SD card into the AMD Kria&trade; KV260 and
connect the HDMI cable into the dedicated port. Now, you can connect the power cable to the connector to boot up the
board.

**NOTE**: insert the HDMI cable **<u>before</u>** the power connector, otherwise you cannot be able to see anything on
the screen. So, the steps are:

1. Insert the flashed SD card;
2. Connect the AMD Kria&trade; KV260 to your monitor via HDMI cable;
3. Connect the power supply to the board in order to power it.

When the Kria boots, please check the three LEDs near the fan. The center light should blink and the other two lights
should be solid when the board has successfully booted. You should be now in the login screen of Ubuntu 22.04.

<div style="width: 100%;">
    <img src="/images/kria-leds.jpg" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="Kria successfully booted leds">
    <br>
    <p style="text-align: center; font-style: italic">Fig. 2 - Kria KV260 booting LEDs</p>
</div>

Log into the `ubuntu` account using the password `makarenalabs`. Now, you should see the desktop.

At this step, you are ready to connect the SmartLock Box using the USB cables provided in the kit. 

### USB Connections

Connect the blue and the black cables into the USB hub on the right of the AMD Kria&trade; KV260

<div style="width: 100%;">
    <img src="/images/usb-connections.jpg" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="USB Connections">
    <br>
    <p style="text-align: center; font-style: italic">Fig. 3 - USB Connections AMD Kria&trade; KV260</p>
</div>

and connect also them to the SmartLock Box

<div style="width: 100%;">
    <img src="/images/usb-connections-box.jpg" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="USB Connections">
    <br>
    <p style="text-align: center; font-style: italic">Fig. 4 - USB Connections SmartLock Box</p>
</div>

Now, you must wait the onsemi camera to be ready (it takes more or less 10 seconds) before continue.


### PMOD Connections
Then, connect the white and purple cables to the PMOD. Insert the PMOD connectors following the white piece of tape on
the PMOD feet on the AMD Kria&trade; KV260, as shown in the following figure.

<div style="width: 100%;">
    <img src="/images/pmod-connections.jpg" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="PMOD Connections">
    <br>
    <p style="text-align: center; font-style: italic">Fig. 5 - PMOD Connections</p>
</div>

At this step, you are ready to run the SmartLock Core solution! Go next to know how to do so.
