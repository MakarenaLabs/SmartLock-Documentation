# How to run demos on the boards

This is a video of the setup on the Kria KV260 board. For the Arty Z7-20 the plug-in camera flow is the same.
<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%;">
        <iframe src="https://www.youtube.com/embed/0KkTUA2-3sY"
        frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
</div>

---  

## Miscellaneous requirements

Before running the solution on the board of your choice, firstly you have to prepare:

 - 2D USB Camera ([Logitech C310HD](https://www.logitech.com/en-us/products/webcams/c310-hd-webcam.960-001065.html) or [Logitech C505](https://www.logitech.com/en-us/products/webcams/c505-hd-webcam.960-001364.html) camera recommended);
 - 3D Camera [PMD Flexx2_VGA](https://3d.pmdtec.com/en/3d-cameras/flexx2/?utm_term=&utm_campaign=3D+Development+Kit&utm_source=adwords&utm_medium=ppc&hsa_acc=3421193929&hsa_cam=17997773983&hsa_grp=153752102618&hsa_ad=670962829811&hsa_src=g&hsa_tgt=dsa-1749788904683&hsa_kw=&hsa_mt=&hsa_net=adwords&hsa_ver=3&gad=1&gclid=Cj0KCQjwl8anBhCFARIsAKbbpyTCLc-K7re7bpdU53uRYWhz26cwuz4m5MfhWMY36dupc_xLTKSRopQaAkexEALw_wcB);
 - USB 3.2 capable USB Type-C Male to USB Type-A Male Data cable: this cable must be a USB 3.2 Data cable, not simply a charging cable. The correct cable is noticeably thick (an example of [one cable model](https://www.amazon.com/dp/B09QKHPT35) which works). Please note that if your workstation’s USB 3.2 connectors use USB Type-C instead of Type-A, then you will need a USB 3.2 capable USB Type-C to USB Type-C Data cable;
 - MicroSD to SD adapter;
 - Small tripod with clamp to hold cameras;
 - Ethernet cable;
 - Network router;

## How to setup the cameras

The following photo shows the camera setup and how the face should be placed. The important factor is that the two cameras are "parallel" and looking in the same direction (it works better if the two lens are placed one on top of each other). The face should be placed in the right distance (about 20 to 30 centimeters) away from the cameras. The face (and possibly the eyes) must look in front of the cameras.

Make sure you place the 3D camera with the cable **on its left**, as you can see in the photo at the end of this paragraph.

To recap, the key points to run correctly the demo are:

 - The two camera sensors must be aligned;
 - The face should be placed 20 to 30 centimeters away from the cameras;
 - The face must be straight to both cameras;


<div style="width: 100%; text-align: center">  
   <img src="../../images/camera1-c505.jpg" style="display: block; margin-left: auto; margin-right: auto; width: 40%;" alt="Setup camera front">  
   <p style="text-align: center; font-style: italic">Fig. 2 - Cameras Setup</p>  
</div>  

## Running examples

Moving forward, we provide some instructions about running the examples on these two boards:

 - [Kria KV260](running-on-kria-kv260.md)
 - [Arty Z7-20](running-on-arty-z7.md)

### Running on Windows

 If you are planning to run the demo in a Windows environment, please be sure to have all requirements needed and described in the [Hardware & Software requirements](../hw-sw-requirements.md#windows-requirements) chapter.
