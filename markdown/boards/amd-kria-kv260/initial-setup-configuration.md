# Initial setup configuration

In this chapter is explained how to setup the hardware to run the example on the AMD Kria&trade; KV260 board. There are
some preliminary steps
in order to have the setup completely up and running, that they are described in the section below.

<div style="width: 100%;">
    <img src="https://www.xilinx.com/content/dam/xilinx/imgs/products/som/som-kv260-4.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="Kria KV260">
    <p style="text-align: center; font-style: italic">Fig. 1 - AMD Kria&trade; KV260</p>
</div>

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

1. Download the MakarenaLabs image. They are some files to be merged together with 7Zip;
2. Merge the downloaded image files into a single image file;
3. Verify the SHA-256 hash of the image file. This will ensure that no errors occurred in the transmission and merge of
   the files. If your system is Windows 11:
    1. Click _Search_ and run `powershell`. (Note: `cmd` will not work);
    2. Navigate to the directory containing the image file;
    3. Enter `Get-FileHash <filename>`;
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
    <p style="text-align: center; font-style: italic">Fig. 2 - Kria KV260 booting LEDs</p>
</div>

Log into the `ubuntu` account using the password `makarenalabs`. Now, you should see the desktop.

At this step, you are ready to connect the onsemi Demo3 Development Kit to one of the USB port of the AMD Kria&trade;
KV260 and wait it
to be ready (it takes more or less 10 seconds). The board is ready when the LEDs placed behind are lit in fixed
position, as shown in the image below.

<div style="width: 100%;">
    <img src="/images/onsemi-leds.jpg" style="display: block; margin-left: auto; margin-right: auto; width: 50%;" alt="onsemi sensor successfully booted leds">
    <p style="text-align: center; font-style: italic">Fig. 3 - onsemi Demo3 Evaluation Kit booting LEDs</p>
</div>

Now you can connect also the ams OSRAM dot projector.

[//]: # (TODO: inserire qui immagine con il dot projector installato nella versione beta)

At this step, you are ready to run the SmartLock Core solution! Go next to know how to do so.
