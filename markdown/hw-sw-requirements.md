# Hardware & Software requirements

The SmartLock solution uses different requirements, both software and hardware. In this section all these needs will be explained.

## Hardware requirements

### AMD/Xilinx Boards

The SmartLock solution proposed here was built on two different AMD/Xilinx boards:

 - **Arty Z7-20**: The Arty Z7 is a ready-to-use development platform designed around the Zynq 7000 System-on-Chip from AMD. The Zynq 7000 architecture tightly integrates a dual-core, 650 MHz ARM Cortex-A9 processor with AMD 7 series Field Programmable Gate Array (FPGA) logic. For more information, please visit the [official website](https://www.xilinx.com/products/boards-and-kits/1-pdb0q2.html);

    <div style="width: 100%;">
        <img src="https://www.xilinx.com/content/dam/xilinx/imgs/prime/IMG_6224%20-%20ARTY-Z7%20-%20Obl%20-%20600.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="Arty Z7-20">
        <p style="text-align: center; font-style: italic">Fig. 1 - Arty Z7-20</p>
    </div>

 - **Kria KV260**: The KV260 is built for advanced vision application development without requiring complex hardware design knowledge. For more information, please visit the [official website](https://www.xilinx.com/products/som/kria/kv260-vision-starter-kit.html);

    <div style="width: 100%;">
        <img src="https://www.xilinx.com/content/dam/xilinx/imgs/products/som/som-kv260-4.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="Kria KV260">
        <p style="text-align: center; font-style: italic">Fig. 2 - Kria KV260</p>
    </div>

### 2D and 3D Cameras

The architecture of SmartLock solution is based on two type of USB camera, one to extract 3D face features and a 2D one to detect and recognize the person in front of it. To run the solution you need:

 - **2D Camera**: [Logitech C505](https://www.logitech.com/it-it/products/webcams/c505-hd-webcam.960-001364.html) or [Logitech C310HD](https://www.logitech.com/it-it/products/webcams/c310-hd-webcam.960-001065.html) (you can use the USB camera of your choice, but these ones are strongly recommended);

    <table>
        <tr>
            <td>
                <div style="width: 100%;">
                    <img src="https://resource.logitech.com/w_692,c_lpad,ar_4:3,q_auto,f_auto,dpr_2.0/d_transparent.gif/content/dam/logitech/en/products/webcams/c310/gallery/c310-gallery-1.png?v=1%202x,%20https://resource.logitech.com/w_692,c_lpad,ar_4:3,q_auto,f_auto,dpr_1.0/d_transparent.gif/content/dam/logitech/en/products/webcams/c310/gallery/c310-gallery-1.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="Logitech C310HD">                
                </div>
            </td>
            <td>
                <div style="width: 100%;">
                    <img src="https://resource.logitech.com/w_692,c_lpad,ar_4:3,q_auto,f_auto,dpr_2.0/d_transparent.gif/content/dam/logitech/en/products/video-conferencing/c505/gallery/c505-gallery-1.png?v=1 2x, https://resource.logitech.com/w_692,c_lpad,ar_4:3,q_auto,f_auto,dpr_1.0/d_transparent.gif/content/dam/logitech/en/products/video-conferencing/c505/gallery/c505-gallery-1.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="Logitech C505">
                </div>
            </td>
        </tr>
        <tr>
            <td style="text-align: center; font-style: italic">
                <a href="https://www.logitech.com/en-us/products/webcams/c310-hd-webcam.960-000585.html">Logitech C310HD</a>
            </td>
            <td style="text-align: center; font-style: italic">
                <a href="https://www.logitech.com/en-us/products/webcams/c505-hd-webcam.960-001363.html">Logitech C505</a>
            </td>
        </tr>
    </table>

 - **3D Camera**: [PMD Flexx2_VGA](https://3d.pmdtec.com/en/3d-cameras/flexx2/?utm_term=&utm_campaign=3D+Development+Kit&utm_source=adwords&utm_medium=ppc&hsa_acc=3421193929&hsa_cam=17997773983&hsa_grp=153752102618&hsa_ad=670962829811&hsa_src=g&hsa_tgt=dsa-1749788904683&hsa_kw=&hsa_mt=&hsa_net=adwords&hsa_ver=3&gad=1&gclid=Cj0KCQjwl8anBhCFARIsAKbbpyTCLc-K7re7bpdU53uRYWhz26cwuz4m5MfhWMY36dupc_xLTKSRopQaAkexEALw_wcB) from [PMD Tech](https://3d.pmdtec.com/en/)

    <div style="width: 100%;">
        <img src="../images/3dcamera.png" style="display: block; margin-left: auto; margin-right: auto; width: 40%;" alt="PMD Flexx2_VGA">
        <p style="text-align: center; font-style: italic; margin-top: 20px"><a href="https://3d.pmdtec.com/en/3d-cameras/flexx2/?utm_term=&utm_campaign=3D+Development+Kit&utm_source=adwords&utm_medium=ppc&hsa_acc=3421193929&hsa_cam=17997773983&hsa_grp=153752102618&hsa_ad=670962829811&hsa_src=g&hsa_tgt=dsa-1749788904683&hsa_kw=&hsa_mt=&hsa_net=adwords&hsa_ver=3&gad=1&gclid=Cj0KCQjwl8anBhCFARIsAKbbpyTCLc-K7re7bpdU53uRYWhz26cwuz4m5MfhWMY36dupc_xLTKSRopQaAkexEALw_wcB">PMD Flexx2_VGA</a></p>
    </div>

### Miscellaneous

Some other miscellaneous are required:

 - **MicroSD card**: it must be _greater than or equal to_ 16GB (32GB if you are going to run the demo into the Kria KV260 demo);
 - **MicroSD to USB adapter**: to burn the image;
 - **USB Type-C/Type-A**: to plug in the 3D camera into the board;
 - **Ethernet cable**: it is needed for board internet connection;
 - **PC/Laptop**: to start the demo;
 - **Router**: to know the IP address of the board;
 - **Tripod**: with clamping mechanism to hold 2D and 2D cameras ([this](https://www.amazon.com/Aureday-Flexible-Portable-Wireless-Recording/dp/B09V4XYHR5/ref=sr_1_2_sspa?crid=5CQQVXGYZ1TY&keywords=tripod&qid=1693820079&sprefix=tripo%2Caps%2C276&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1) is an example);

Optional (only for Kria KV260 board):

 - **Full-HD HDMI Monitor and HDMI cable**: to show the GUI directly on the monitor plugged into the Kria KV260 board. In this case, you have to plug in the Full-HD HDMI monitor (1920x1080p) into the Kria KV260 HDMI port and work directly on the board's graphic interface;
 - **Keyboard and mouse kit**: to run the commands into the Kria board;

## Software requirements

There are some software that need to be installed into your PC in order to run the solution successfully. First of all, you need to have at least 22GB free on your disk in order to download the image to be burnt (it is more or less 19GB big). To burn the image, you need:

 - [**7Zip**](https://www.7-zip.org/download.html): to unarchive the downloaded image files;
 - [**Balena Etcher**](https://etcher.balena.io/): to burn the image. You can use one of your choice, but this one is strongly recommended;
 - **SSH Terminal**: to connect to the board of your choice. If you are running the demo with a Windows PC, [PuTTY](https://www.putty.org/) or [TeraTerm](https://ttssh2.osdn.jp/index.html.en) are recommended. Otherwise, if you are running on Linux, you can use the integrated terminal;


### Windows requirements

If you choose to run the demo into the Kria KV260 board and you are running on a Windows PC, you need also the [Xming Server](http://www.straightrunning.com/XmingNotes/) in order to see the graphic user interface of the software. In this case, we show below how to set up Xming Server under PuTTY software. Please refer to the Xming Server website to know how to install it.

First of all, open the PuTTY software and, in the list at the left in the main page, open _Connection_, click _SSH_ → _X11_, check the _Enable X11 forwarding_ and in the _X display location_ field write `localhost:0`, as show in the image below.

<div style="width: 100%;">
    <img src="../images/putty1.jpg" style="display: block; margin-left: auto; margin-right: auto; width: 70%;" alt="PMD Flexx2_VGA">
    <p style="text-align: center; font-style: italic; margin-top: 20px">PuTTY X11 port forwarding</p>
</div>