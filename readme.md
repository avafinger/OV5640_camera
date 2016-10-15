Linux Device Tree with OV5640 enabled for BananaPi M64 / Pine64+
=================================================================

This is the Linux Device Tree with OV5640 enabled so your BananaPi M64 and Pine64+ can work with the enhanced OV5640 driver for the CMOS camera that incorporates many image resolutions and/or image quality.
You can take advantage of a higher FPS (Video Mode), Image Quality (Preview or Capture) or Window sizes, choosing the one that best fit your needs.

Working window sizes and expected FPS (video mode)
- QSXGA: 2592x1936 (7.5 FPS)
- QXGA: 2048x1536 (7.5 FPS)
- 1080P: 1920x1080 (7.5 FPS, 15 FPS)
- UXGA: 1600x1200 (7.5 FPS, 15 FPS)
- UXGA: 1280x960 (7.5 FPS, 15 FPS)
- 720P: 1280x720 (7.5 FPS, 15 FPS)
- XGA: 1024x768 (7.5 FPS, 15 FPS)
- SVGA: 800x600 (15 FPS, 30 FPS)
- VGA: 640x480 (15 FPS, 30 FPS)
- QVGA: 320x240 (30 FPS)
- QCIF: 176x144 (30 FPS with some artifacts)


In order to use OV5640 camera on BananaPi M64 and/or Pine64+ you will need the following:

- A Compiled Linux Device Tree (dtb) with OV5640 enabled
- Ubuntu Xenial 16.04 arm64 or Debian Jessie with unbroken package (*) (Desktop or server)

(*) What do you mean by broken distro/package? In general terms a broken Distro/Package have some different library version linked in 
and put the distro in such a state you cannot fix it or if you fix one package you may break another and so one. You better get a good and clean image.

You may end like so:

	sudo apt-get install libopencv-dev python-opencv
	Reading package lists... Done
	Building dependency tree       
	Reading state information... Done
	Some packages could not be installed. This may mean that you have
	requested an impossible situation or if you are using the unstable
	distribution that some required packages have not yet been created
	or been moved out of Incoming.
	The following information may help to resolve the situation:
	
	The following packages have unmet dependencies:
	 libopencv-dev : Depends: libopencv-objdetect-dev (= 2.4.9.1+dfsg-1.5ubuntu1) but it is not going to be installed
	                 Depends: libopencv-highgui-dev (= 2.4.9.1+dfsg-1.5ubuntu1) but it is not going to be installed
	                 Depends: libopencv-legacy-dev (= 2.4.9.1+dfsg-1.5ubuntu1) but it is not going to be installed
	                 Depends: libopencv-contrib-dev (= 2.4.9.1+dfsg-1.5ubuntu1) but it is not going to be installed
	                 Depends: libopencv-videostab-dev (= 2.4.9.1+dfsg-1.5ubuntu1) but it is not going to be installed
	                 Depends: libopencv-superres-dev (= 2.4.9.1+dfsg-1.5ubuntu1) but it is not going to be installed
	                 Depends: libopencv-ocl-dev (= 2.4.9.1+dfsg-1.5ubuntu1) but it is not going to be installed
	                 Depends: libcv-dev (= 2.4.9.1+dfsg-1.5ubuntu1) but it is not going to be installed
	                 Depends: libhighgui-dev (= 2.4.9.1+dfsg-1.5ubuntu1) but it is not going to be installed
	                 Depends: libcvaux-dev (= 2.4.9.1+dfsg-1.5ubuntu1) but it is not going to be installed
	E: Unable to correct problems, you have held broken packages.



- Enhanced OV5640 driver (A64) BananaPi M64 / Pine64+ Build with latest longsleep's kernel (included here)

Steps you should take
=====================
You will need a linux box to update your current distro with the new kernel image and modules with OV5640.
You can also update kernel with the new OV5640 driver manually onboard without a second linux box but this instructions are out of the scope.

* Identify where your SD_CARD 

type:

	 df -lh


You should see something like this:


	/dev/sdc1        50M   24M   26M  48% /media/boot
	/dev/sdc2        15G   12G  2.0G  87% /media/rootfs

or

	/dev/sdb1        50M   24M   26M  48% /media/boot
	/dev/sdb2        15G   12G  2.0G  87% /media/rootfs


so your SD_CARD letter will have the following form: 

	/dev/sdX 

and can be /dev/sdb or /dev/sdc and 1st partition is /media/boot (kernel Image) and 2nd partition is /media/rootfs (kernel modules with OV5640).
You could also find the correct /dev/SD_CARD by typing dmesg:

	dmesg
	

output:

	[18631.665749] sdc: detected capacity change from 15931539456 to 0
	[19243.730508] sd 4:0:0:0: [sdc] 31116288 512-byte logical blocks: (15.9 GB/14.8 GiB)


In some distro you could have /media/ubuntu/rootfs or /media/ubuntu/ROOTFS and is up to you to find the correct path.
bpi-M64 1st partition: /media/BPI-BOOT/bananapi/bpi-m64/linux


Type this instructions in your linux box from command line:

- Pine64+

Xenial Image:

	git clone https://github.com/avafinger/OV5640_camera
	cd OV5640_camera
	sudo su
	mv /media/boot/pine64/Image /media/boot/pine64/Image_OK
	sync
	unzip Image_ov5640.zip
	cp -v Image /media/boot/pine64/Image
	mv /media/boot/pine64/sun50i-a64-pine64-plus.dtb /media/boot/pine64/sun50i-a64-pine64-plus.dtb_OK
	cp -v sun50i-a64-pine64-plus.dtb /media/boot/pine64/sun50i-a64-pine64-plus.dtb
	sync
	tar -xvpzf kernel_ov5640.tar.gz -C /media/rootfs/lib/modules --numeric-ow
	sync
	tar -xvpzf linux-headers-3.10.102.tar.gz -C /media/rootfs/usr/src --numeric-ow
	sync


- You could try using the MATE image from bpi-m64

MATE image:
	
	git clone https://github.com/avafinger/OV5640_camera
	cd OV5640_camera
	sudo su
	mv /media/BPI-BOOT/bananapi/bpi-m64/linux/Image /media/BPI-BOOT/bananapi/bpi-m64/linux/Image_OK
	sync
	unzip Image_ov5640.zip
	cp -v Image /media/BPI-BOOT/bananapi/bpi-m64/linux/Image
	mv /media/BPI-BOOT/bananapi/bpi-m64/linux/pine64-plus.dtb /media/BPI-BOOT/bananapi/bpi-m64/linux/pine64-plus.dtb_OK
	cp -v sun50i-a64-pine64-plus.dtb /media/BPI-BOOT/bananapi/bpi-m64/linux/pine64-plus.dtb
	sync
	tar -xvpzf kernel_ov5640.tar.gz -C /media/BPI-ROOT/lib/modules --numeric-ow
	sync
	tar -xvpzf linux-headers-3.10.102.tar.gz -C /media/BPI-ROOT/usr/src --numeric-ow
	sync

wait untill complete and only after this you unmount the SD_CARD

* unmount the SD_CARD

* insert your SD_CARD into BananaPi M64 / Pine64+ and boot, if everything is fine you will boot into your DISTRO and you are ready to start playing with the camera.


How to load OV5640 manually
===========================
Unload any previous vfe device loaded

	sudo modprobe -r -f vfe_v4l2
	sleep 2
	sudo modprobe -r -f ov5640


Load the new improved OV5640

	sudo modprobe ov5640
	sleep 2
	sudo modprobe vfe_v4l2


where:

frame_rate=0 (default with no parameters = no parameters = best FPS / quality), frame_rate=1 (7.5 FPS), frame_rate=2 (15 FPS), frame_rate=3 (30 FPS) (default=0 - or no parms - default settings)

default frame rate tries to use the best FPS possible. If you want better quality use frame_rate=1

Application to grab frames - v4l2 / OpenCV (works on Desktop or server)
======================================================================

Install linux kernel headers before building cap


	git clone https://github.com/avafinger/cap-v4l2
	cd cap-v4l2
	sudo ./install_deps.sh
	./build_script_A64.sh
	./cap 1920 1080 4 1 -999 -1 -1 (output will be frame_1920x1080.jpg)



you can also run a simple test ./test_ov5640.sh to grab all frames and display some FPS statistics


Important
=========

You need the kernel with the new OV5640 driver compiled as a module.

You need to load the correct driver (this latest ov5640) and make sure /dev/video0 is created.

Check if you have all dependencies.


tested on:
==========
- BananaPi M64
- Pine64+

NanoPi A64 (can possibly works, there is a great chance - guessing)

History Log:
* initial commit, missing info (yet to complete)

Output from testing cap app
===========================

	TEST 0 - ( 2592 x 1936 )
	---- cap parameters -----
	width: 2592
	height: 1936
	v4l2 buffers: 4
	exposure: -999
	hflip: -1
	sensor_vflip: -1
	Mode: V4L2_MODE_VIDEO
	Driver: "sunxi-vfe"
	Card: "sunxi-vfe"
	Bus: "sunxi_vfe vfe.12"
	Version: 1.0
	Capabilities: 05000001
	Input: 0
	V4L2 pixel formats:
	0: [0x50323234] '422P' (planar YUV 422)
	1: [0x32315559] 'YU12' (planar YUV 420)
	2: [0x32315659] 'YV12' (planar YVU 420)
	3: [0x3631564E] 'NV16' (planar YUV 422 UV combined)
	4: [0x3231564E] 'NV12' (planar YUV 420 UV combined)
	5: [0x3136564E] 'NV61' (planar YUV 422 VU combined)
	6: [0x3132564E] 'NV21' (planar YUV 420 VU combined)
	7: [0x32314D48] 'HM12' (MB YUV420)
	8: [0x56595559] 'YUYV' (YUV422 YUYV)
	9: [0x55595659] 'YVYU' (YUV422 YVYU)
	10: [0x59565955] 'UYVY' (YUV422 UYVY)
	11: [0x59555956] 'VYUY' (YUV422 VYUY)
	12: [0x31384142] 'BA81' (RAW Bayer BGGR 8bit)
	13: [0x47524247] 'GBRG' (RAW Bayer GBRG 8bit)
	14: [0x47425247] 'GRBG' (RAW Bayer GRBG 8bit)
	15: [0x47425247] 'GRBG' (RAW Bayer RGGB 8bit)
	16: [0x30314742] 'BG10' (RAW Bayer BGGR 10bit)
	17: [0x30314247] 'GB10' (RAW Bayer GBRG 10bit)
	18: [0x30314142] 'BA10' (RAW Bayer GRBG 10bit)
	19: [0x30314142] 'BA10' (RAW Bayer RGGB 10bit)
	20: [0x32314742] 'BG12' (RAW Bayer BGGR 12bit)
	21: [0x32314247] 'GB12' (RAW Bayer GBRG 12bit)
	22: [0x32314142] 'BA12' (RAW Bayer GRBG 12bit)
	23: [0x32314142] 'BA12' (RAW Bayer RGGB 12bit)
	
	Sensor size: 2592x1936 pixels
	Pixel Format: V4L2_PIX_FMT_YUV420
	Frame #10 will be saved!
	Length: 7528448 	Bytesused: 7528448 	Address: 0x453be0
	FPS[0]: 1.27
	Length: 7528448 	Bytesused: 7528448 	Address: 0x453bf0
	FPS[1]: 7.51
	Length: 7528448 	Bytesused: 7528448 	Address: 0x453c00
	FPS[2]: 7.51
	Length: 7528448 	Bytesused: 7528448 	Address: 0x453c10
	FPS[3]: 7.51
	Length: 7528448 	Bytesused: 7528448 	Address: 0x453be0
	FPS[4]: 7.50
	Length: 7528448 	Bytesused: 7528448 	Address: 0x453bf0
	FPS[5]: 7.51
	Length: 7528448 	Bytesused: 7528448 	Address: 0x453c00
	FPS[6]: 7.50
	Length: 7528448 	Bytesused: 7528448 	Address: 0x453c10
	FPS[7]: 7.50
	Length: 7528448 	Bytesused: 7528448 	Address: 0x453be0
	FPS[8]: 7.50
	Length: 7528448 	Bytesused: 7528448 	Address: 0x453bf0
	FPS[9]: 0.30
	------- Avg FPS: 7.51 --------
	
	TEST 1 - ( 2048 x 1536 )
	---- cap parameters -----
	width: 2048
	height: 1536
	v4l2 buffers: 4
	exposure: -999
	hflip: -1
	sensor_vflip: -1
	Mode: V4L2_MODE_VIDEO
	Driver: "sunxi-vfe"
	Card: "sunxi-vfe"
	Bus: "sunxi_vfe vfe.12"
	Version: 1.0
	Capabilities: 05000001
	Input: 0
	V4L2 pixel formats:
	0: [0x50323234] '422P' (planar YUV 422)
	1: [0x32315559] 'YU12' (planar YUV 420)
	2: [0x32315659] 'YV12' (planar YVU 420)
	3: [0x3631564E] 'NV16' (planar YUV 422 UV combined)
	4: [0x3231564E] 'NV12' (planar YUV 420 UV combined)
	5: [0x3136564E] 'NV61' (planar YUV 422 VU combined)
	6: [0x3132564E] 'NV21' (planar YUV 420 VU combined)
	7: [0x32314D48] 'HM12' (MB YUV420)
	8: [0x56595559] 'YUYV' (YUV422 YUYV)
	9: [0x55595659] 'YVYU' (YUV422 YVYU)
	10: [0x59565955] 'UYVY' (YUV422 UYVY)
	11: [0x59555956] 'VYUY' (YUV422 VYUY)
	12: [0x31384142] 'BA81' (RAW Bayer BGGR 8bit)
	13: [0x47524247] 'GBRG' (RAW Bayer GBRG 8bit)
	14: [0x47425247] 'GRBG' (RAW Bayer GRBG 8bit)
	15: [0x47425247] 'GRBG' (RAW Bayer RGGB 8bit)
	16: [0x30314742] 'BG10' (RAW Bayer BGGR 10bit)
	17: [0x30314247] 'GB10' (RAW Bayer GBRG 10bit)
	18: [0x30314142] 'BA10' (RAW Bayer GRBG 10bit)
	19: [0x30314142] 'BA10' (RAW Bayer RGGB 10bit)
	20: [0x32314742] 'BG12' (RAW Bayer BGGR 12bit)
	21: [0x32314247] 'GB12' (RAW Bayer GBRG 12bit)
	22: [0x32314142] 'BA12' (RAW Bayer GRBG 12bit)
	23: [0x32314142] 'BA12' (RAW Bayer RGGB 12bit)
	
	Sensor size: 2048x1536 pixels
	Pixel Format: V4L2_PIX_FMT_YUV420
	Frame #10 will be saved!
	Length: 4718592 	Bytesused: 4718592 	Address: 0x453be0
	FPS[0]: 1.12
	Length: 4718592 	Bytesused: 4718592 	Address: 0x453bf0
	FPS[1]: 7.51
	Length: 4718592 	Bytesused: 4718592 	Address: 0x453c00
	FPS[2]: 7.51
	Length: 4718592 	Bytesused: 4718592 	Address: 0x453c10
	FPS[3]: 7.50
	Length: 4718592 	Bytesused: 4718592 	Address: 0x453be0
	FPS[4]: 7.50
	Length: 4718592 	Bytesused: 4718592 	Address: 0x453bf0
	FPS[5]: 7.51
	Length: 4718592 	Bytesused: 4718592 	Address: 0x453c00
	FPS[6]: 7.50
	Length: 4718592 	Bytesused: 4718592 	Address: 0x453c10
	FPS[7]: 7.50
	Length: 4718592 	Bytesused: 4718592 	Address: 0x453be0
	FPS[8]: 7.50
	Length: 4718592 	Bytesused: 4718592 	Address: 0x453bf0
	FPS[9]: 0.46
	------- Avg FPS: 7.51 --------
	
	TEST 2 - ( 1920 x 1080 )
	---- cap parameters -----
	width: 1920
	height: 1080
	v4l2 buffers: 4
	exposure: -999
	hflip: -1
	sensor_vflip: -1
	Mode: V4L2_MODE_VIDEO
	Driver: "sunxi-vfe"
	Card: "sunxi-vfe"
	Bus: "sunxi_vfe vfe.12"
	Version: 1.0
	Capabilities: 05000001
	Input: 0
	V4L2 pixel formats:
	0: [0x50323234] '422P' (planar YUV 422)
	1: [0x32315559] 'YU12' (planar YUV 420)
	2: [0x32315659] 'YV12' (planar YVU 420)
	3: [0x3631564E] 'NV16' (planar YUV 422 UV combined)
	4: [0x3231564E] 'NV12' (planar YUV 420 UV combined)
	5: [0x3136564E] 'NV61' (planar YUV 422 VU combined)
	6: [0x3132564E] 'NV21' (planar YUV 420 VU combined)
	7: [0x32314D48] 'HM12' (MB YUV420)
	8: [0x56595559] 'YUYV' (YUV422 YUYV)
	9: [0x55595659] 'YVYU' (YUV422 YVYU)
	10: [0x59565955] 'UYVY' (YUV422 UYVY)
	11: [0x59555956] 'VYUY' (YUV422 VYUY)
	12: [0x31384142] 'BA81' (RAW Bayer BGGR 8bit)
	13: [0x47524247] 'GBRG' (RAW Bayer GBRG 8bit)
	14: [0x47425247] 'GRBG' (RAW Bayer GRBG 8bit)
	15: [0x47425247] 'GRBG' (RAW Bayer RGGB 8bit)
	16: [0x30314742] 'BG10' (RAW Bayer BGGR 10bit)
	17: [0x30314247] 'GB10' (RAW Bayer GBRG 10bit)
	18: [0x30314142] 'BA10' (RAW Bayer GRBG 10bit)
	19: [0x30314142] 'BA10' (RAW Bayer RGGB 10bit)
	20: [0x32314742] 'BG12' (RAW Bayer BGGR 12bit)
	21: [0x32314247] 'GB12' (RAW Bayer GBRG 12bit)
	22: [0x32314142] 'BA12' (RAW Bayer GRBG 12bit)
	23: [0x32314142] 'BA12' (RAW Bayer RGGB 12bit)
	
	Sensor size: 1920x1080 pixels
	Pixel Format: V4L2_PIX_FMT_YUV420
	Frame #10 will be saved!
	Length: 3112960 	Bytesused: 3112960 	Address: 0x453be0
	FPS[0]: 4.76
	Length: 3112960 	Bytesused: 3112960 	Address: 0x453bf0
	FPS[1]: 12.52
	Length: 3112960 	Bytesused: 3112960 	Address: 0x453c00
	FPS[2]: 12.50
	Length: 3112960 	Bytesused: 3112960 	Address: 0x453c10
	FPS[3]: 12.50
	Length: 3112960 	Bytesused: 3112960 	Address: 0x453be0
	FPS[4]: 12.49
	Length: 3112960 	Bytesused: 3112960 	Address: 0x453bf0
	FPS[5]: 12.51
	Length: 3112960 	Bytesused: 3112960 	Address: 0x453c00
	FPS[6]: 12.50
	Length: 3112960 	Bytesused: 3112960 	Address: 0x453c10
	FPS[7]: 12.50
	Length: 3112960 	Bytesused: 3112960 	Address: 0x453be0
	FPS[8]: 12.50
	Length: 3112960 	Bytesused: 3112960 	Address: 0x453bf0
	FPS[9]: 0.71
	------- Avg FPS: 12.50 --------
	
	TEST 3 - ( 1600 x 1200 )
	---- cap parameters -----
	width: 1600
	height: 1200
	v4l2 buffers: 4
	exposure: -999
	hflip: -1
	sensor_vflip: -1
	Mode: V4L2_MODE_VIDEO
	Driver: "sunxi-vfe"
	Card: "sunxi-vfe"
	Bus: "sunxi_vfe vfe.12"
	Version: 1.0
	Capabilities: 05000001
	Input: 0
	V4L2 pixel formats:
	0: [0x50323234] '422P' (planar YUV 422)
	1: [0x32315559] 'YU12' (planar YUV 420)
	2: [0x32315659] 'YV12' (planar YVU 420)
	3: [0x3631564E] 'NV16' (planar YUV 422 UV combined)
	4: [0x3231564E] 'NV12' (planar YUV 420 UV combined)
	5: [0x3136564E] 'NV61' (planar YUV 422 VU combined)
	6: [0x3132564E] 'NV21' (planar YUV 420 VU combined)
	7: [0x32314D48] 'HM12' (MB YUV420)
	8: [0x56595559] 'YUYV' (YUV422 YUYV)
	9: [0x55595659] 'YVYU' (YUV422 YVYU)
	10: [0x59565955] 'UYVY' (YUV422 UYVY)
	11: [0x59555956] 'VYUY' (YUV422 VYUY)
	12: [0x31384142] 'BA81' (RAW Bayer BGGR 8bit)
	13: [0x47524247] 'GBRG' (RAW Bayer GBRG 8bit)
	14: [0x47425247] 'GRBG' (RAW Bayer GRBG 8bit)
	15: [0x47425247] 'GRBG' (RAW Bayer RGGB 8bit)
	16: [0x30314742] 'BG10' (RAW Bayer BGGR 10bit)
	17: [0x30314247] 'GB10' (RAW Bayer GBRG 10bit)
	18: [0x30314142] 'BA10' (RAW Bayer GRBG 10bit)
	19: [0x30314142] 'BA10' (RAW Bayer RGGB 10bit)
	20: [0x32314742] 'BG12' (RAW Bayer BGGR 12bit)
	21: [0x32314247] 'GB12' (RAW Bayer GBRG 12bit)
	22: [0x32314142] 'BA12' (RAW Bayer GRBG 12bit)
	23: [0x32314142] 'BA12' (RAW Bayer RGGB 12bit)
	
	Sensor size: 1600x1200 pixels
	Pixel Format: V4L2_PIX_FMT_YUV420
	Frame #10 will be saved!
	Length: 2883584 	Bytesused: 2883584 	Address: 0x453be0
	FPS[0]: 0.96
	Length: 2883584 	Bytesused: 2883584 	Address: 0x453bf0
	FPS[1]: 7.51
	Length: 2883584 	Bytesused: 2883584 	Address: 0x453c00
	FPS[2]: 7.50
	Length: 2883584 	Bytesused: 2883584 	Address: 0x453c10
	FPS[3]: 7.51
	Length: 2883584 	Bytesused: 2883584 	Address: 0x453be0
	FPS[4]: 7.50
	Length: 2883584 	Bytesused: 2883584 	Address: 0x453bf0
	FPS[5]: 7.50
	Length: 2883584 	Bytesused: 2883584 	Address: 0x453c00
	FPS[6]: 7.50
	Length: 2883584 	Bytesused: 2883584 	Address: 0x453c10
	FPS[7]: 7.51
	Length: 2883584 	Bytesused: 2883584 	Address: 0x453be0
	FPS[8]: 7.51
	Length: 2883584 	Bytesused: 2883584 	Address: 0x453bf0
	FPS[9]: 0.73
	------- Avg FPS: 7.51 --------
	
	TEST 4 - ( 1280 x 960 )
	---- cap parameters -----
	width: 1280
	height: 960
	v4l2 buffers: 4
	exposure: -999
	hflip: -1
	sensor_vflip: -1
	Mode: V4L2_MODE_VIDEO
	Driver: "sunxi-vfe"
	Card: "sunxi-vfe"
	Bus: "sunxi_vfe vfe.12"
	Version: 1.0
	Capabilities: 05000001
	Input: 0
	V4L2 pixel formats:
	0: [0x50323234] '422P' (planar YUV 422)
	1: [0x32315559] 'YU12' (planar YUV 420)
	2: [0x32315659] 'YV12' (planar YVU 420)
	3: [0x3631564E] 'NV16' (planar YUV 422 UV combined)
	4: [0x3231564E] 'NV12' (planar YUV 420 UV combined)
	5: [0x3136564E] 'NV61' (planar YUV 422 VU combined)
	6: [0x3132564E] 'NV21' (planar YUV 420 VU combined)
	7: [0x32314D48] 'HM12' (MB YUV420)
	8: [0x56595559] 'YUYV' (YUV422 YUYV)
	9: [0x55595659] 'YVYU' (YUV422 YVYU)
	10: [0x59565955] 'UYVY' (YUV422 UYVY)
	11: [0x59555956] 'VYUY' (YUV422 VYUY)
	12: [0x31384142] 'BA81' (RAW Bayer BGGR 8bit)
	13: [0x47524247] 'GBRG' (RAW Bayer GBRG 8bit)
	14: [0x47425247] 'GRBG' (RAW Bayer GRBG 8bit)
	15: [0x47425247] 'GRBG' (RAW Bayer RGGB 8bit)
	16: [0x30314742] 'BG10' (RAW Bayer BGGR 10bit)
	17: [0x30314247] 'GB10' (RAW Bayer GBRG 10bit)
	18: [0x30314142] 'BA10' (RAW Bayer GRBG 10bit)
	19: [0x30314142] 'BA10' (RAW Bayer RGGB 10bit)
	20: [0x32314742] 'BG12' (RAW Bayer BGGR 12bit)
	21: [0x32314247] 'GB12' (RAW Bayer GBRG 12bit)
	22: [0x32314142] 'BA12' (RAW Bayer GRBG 12bit)
	23: [0x32314142] 'BA12' (RAW Bayer RGGB 12bit)
	
	Sensor size: 1280x960 pixels
	Pixel Format: V4L2_PIX_FMT_YUV420
	Frame #10 will be saved!
	Length: 1843200 	Bytesused: 1843200 	Address: 0x453be0
	FPS[0]: 6.77
	Length: 1843200 	Bytesused: 1843200 	Address: 0x453bf0
	FPS[1]: 15.03
	Length: 1843200 	Bytesused: 1843200 	Address: 0x453c00
	FPS[2]: 15.01
	Length: 1843200 	Bytesused: 1843200 	Address: 0x453c10
	FPS[3]: 15.02
	Length: 1843200 	Bytesused: 1843200 	Address: 0x453be0
	FPS[4]: 15.01
	Length: 1843200 	Bytesused: 1843200 	Address: 0x453bf0
	FPS[5]: 15.01
	Length: 1843200 	Bytesused: 1843200 	Address: 0x453c00
	FPS[6]: 15.01
	Length: 1843200 	Bytesused: 1843200 	Address: 0x453c10
	FPS[7]: 15.01
	Length: 1843200 	Bytesused: 1843200 	Address: 0x453be0
	FPS[8]: 15.01
	Length: 1843200 	Bytesused: 1843200 	Address: 0x453bf0
	FPS[9]: 1.15
	------- Avg FPS: 15.01 --------
	
	TEST 5 - ( 1280 x 720 )
	---- cap parameters -----
	width: 1280
	height: 720
	v4l2 buffers: 4
	exposure: -999
	hflip: -1
	sensor_vflip: -1
	Mode: V4L2_MODE_VIDEO
	Driver: "sunxi-vfe"
	Card: "sunxi-vfe"
	Bus: "sunxi_vfe vfe.12"
	Version: 1.0
	Capabilities: 05000001
	Input: 0
	V4L2 pixel formats:
	0: [0x50323234] '422P' (planar YUV 422)
	1: [0x32315559] 'YU12' (planar YUV 420)
	2: [0x32315659] 'YV12' (planar YVU 420)
	3: [0x3631564E] 'NV16' (planar YUV 422 UV combined)
	4: [0x3231564E] 'NV12' (planar YUV 420 UV combined)
	5: [0x3136564E] 'NV61' (planar YUV 422 VU combined)
	6: [0x3132564E] 'NV21' (planar YUV 420 VU combined)
	7: [0x32314D48] 'HM12' (MB YUV420)
	8: [0x56595559] 'YUYV' (YUV422 YUYV)
	9: [0x55595659] 'YVYU' (YUV422 YVYU)
	10: [0x59565955] 'UYVY' (YUV422 UYVY)
	11: [0x59555956] 'VYUY' (YUV422 VYUY)
	12: [0x31384142] 'BA81' (RAW Bayer BGGR 8bit)
	13: [0x47524247] 'GBRG' (RAW Bayer GBRG 8bit)
	14: [0x47425247] 'GRBG' (RAW Bayer GRBG 8bit)
	15: [0x47425247] 'GRBG' (RAW Bayer RGGB 8bit)
	16: [0x30314742] 'BG10' (RAW Bayer BGGR 10bit)
	17: [0x30314247] 'GB10' (RAW Bayer GBRG 10bit)
	18: [0x30314142] 'BA10' (RAW Bayer GRBG 10bit)
	19: [0x30314142] 'BA10' (RAW Bayer RGGB 10bit)
	20: [0x32314742] 'BG12' (RAW Bayer BGGR 12bit)
	21: [0x32314247] 'GB12' (RAW Bayer GBRG 12bit)
	22: [0x32314142] 'BA12' (RAW Bayer GRBG 12bit)
	23: [0x32314142] 'BA12' (RAW Bayer RGGB 12bit)
	
	Sensor size: 1280x720 pixels
	Pixel Format: V4L2_PIX_FMT_YUV420
	Frame #10 will be saved!
	Length: 1384448 	Bytesused: 1384448 	Address: 0x453be0
	FPS[0]: 2.74
	Length: 1384448 	Bytesused: 1384448 	Address: 0x453bf0
	FPS[1]: 32.99
	Length: 1384448 	Bytesused: 1384448 	Address: 0x453c00
	FPS[2]: 32.87
	Length: 1384448 	Bytesused: 1384448 	Address: 0x453c10
	FPS[3]: 32.87
	Length: 1384448 	Bytesused: 1384448 	Address: 0x453be0
	FPS[4]: 32.87
	Length: 1384448 	Bytesused: 1384448 	Address: 0x453bf0
	FPS[5]: 32.86
	Length: 1384448 	Bytesused: 1384448 	Address: 0x453c00
	FPS[6]: 32.87
	Length: 1384448 	Bytesused: 1384448 	Address: 0x453c10
	FPS[7]: 32.86
	Length: 1384448 	Bytesused: 1384448 	Address: 0x453be0
	FPS[8]: 32.87
	Length: 1384448 	Bytesused: 1384448 	Address: 0x453bf0
	FPS[9]: 1.58
	------- Avg FPS: 32.88 --------
	
	TEST 6 - ( 1024 x 768 )
	---- cap parameters -----
	width: 1024
	height: 768
	v4l2 buffers: 4
	exposure: -999
	hflip: -1
	sensor_vflip: -1
	Mode: V4L2_MODE_VIDEO
	Driver: "sunxi-vfe"
	Card: "sunxi-vfe"
	Bus: "sunxi_vfe vfe.12"
	Version: 1.0
	Capabilities: 05000001
	Input: 0
	V4L2 pixel formats:
	0: [0x50323234] '422P' (planar YUV 422)
	1: [0x32315559] 'YU12' (planar YUV 420)
	2: [0x32315659] 'YV12' (planar YVU 420)
	3: [0x3631564E] 'NV16' (planar YUV 422 UV combined)
	4: [0x3231564E] 'NV12' (planar YUV 420 UV combined)
	5: [0x3136564E] 'NV61' (planar YUV 422 VU combined)
	6: [0x3132564E] 'NV21' (planar YUV 420 VU combined)
	7: [0x32314D48] 'HM12' (MB YUV420)
	8: [0x56595559] 'YUYV' (YUV422 YUYV)
	9: [0x55595659] 'YVYU' (YUV422 YVYU)
	10: [0x59565955] 'UYVY' (YUV422 UYVY)
	11: [0x59555956] 'VYUY' (YUV422 VYUY)
	12: [0x31384142] 'BA81' (RAW Bayer BGGR 8bit)
	13: [0x47524247] 'GBRG' (RAW Bayer GBRG 8bit)
	14: [0x47425247] 'GRBG' (RAW Bayer GRBG 8bit)
	15: [0x47425247] 'GRBG' (RAW Bayer RGGB 8bit)
	16: [0x30314742] 'BG10' (RAW Bayer BGGR 10bit)
	17: [0x30314247] 'GB10' (RAW Bayer GBRG 10bit)
	18: [0x30314142] 'BA10' (RAW Bayer GRBG 10bit)
	19: [0x30314142] 'BA10' (RAW Bayer RGGB 10bit)
	20: [0x32314742] 'BG12' (RAW Bayer BGGR 12bit)
	21: [0x32314247] 'GB12' (RAW Bayer GBRG 12bit)
	22: [0x32314142] 'BA12' (RAW Bayer GRBG 12bit)
	23: [0x32314142] 'BA12' (RAW Bayer RGGB 12bit)
	
	Sensor size: 1024x768 pixels
	Pixel Format: V4L2_PIX_FMT_YUV420
	Frame #10 will be saved!
	Length: 1179648 	Bytesused: 1179648 	Address: 0x453be0
	FPS[0]: 1.25
	Length: 1179648 	Bytesused: 1179648 	Address: 0x453bf0
	FPS[1]: 9.40
	Length: 1179648 	Bytesused: 1179648 	Address: 0x453c00
	FPS[2]: 9.38
	Length: 1179648 	Bytesused: 1179648 	Address: 0x453c10
	FPS[3]: 9.38
	Length: 1179648 	Bytesused: 1179648 	Address: 0x453be0
	FPS[4]: 9.38
	Length: 1179648 	Bytesused: 1179648 	Address: 0x453bf0
	FPS[5]: 9.38
	Length: 1179648 	Bytesused: 1179648 	Address: 0x453c00
	FPS[6]: 9.38
	Length: 1179648 	Bytesused: 1179648 	Address: 0x453c10
	FPS[7]: 9.38
	Length: 1179648 	Bytesused: 1179648 	Address: 0x453be0
	FPS[8]: 9.39
	Length: 1179648 	Bytesused: 1179648 	Address: 0x453bf0
	FPS[9]: 1.63
	------- Avg FPS: 9.38 --------
	
	TEST 7 - ( 800 x 600 )
	---- cap parameters -----
	width: 800
	height: 600
	v4l2 buffers: 4
	exposure: -999
	hflip: -1
	sensor_vflip: -1
	Mode: V4L2_MODE_VIDEO
	Driver: "sunxi-vfe"
	Card: "sunxi-vfe"
	Bus: "sunxi_vfe vfe.12"
	Version: 1.0
	Capabilities: 05000001
	Input: 0
	V4L2 pixel formats:
	0: [0x50323234] '422P' (planar YUV 422)
	1: [0x32315559] 'YU12' (planar YUV 420)
	2: [0x32315659] 'YV12' (planar YVU 420)
	3: [0x3631564E] 'NV16' (planar YUV 422 UV combined)
	4: [0x3231564E] 'NV12' (planar YUV 420 UV combined)
	5: [0x3136564E] 'NV61' (planar YUV 422 VU combined)
	6: [0x3132564E] 'NV21' (planar YUV 420 VU combined)
	7: [0x32314D48] 'HM12' (MB YUV420)
	8: [0x56595559] 'YUYV' (YUV422 YUYV)
	9: [0x55595659] 'YVYU' (YUV422 YVYU)
	10: [0x59565955] 'UYVY' (YUV422 UYVY)
	11: [0x59555956] 'VYUY' (YUV422 VYUY)
	12: [0x31384142] 'BA81' (RAW Bayer BGGR 8bit)
	13: [0x47524247] 'GBRG' (RAW Bayer GBRG 8bit)
	14: [0x47425247] 'GRBG' (RAW Bayer GRBG 8bit)
	15: [0x47425247] 'GRBG' (RAW Bayer RGGB 8bit)
	16: [0x30314742] 'BG10' (RAW Bayer BGGR 10bit)
	17: [0x30314247] 'GB10' (RAW Bayer GBRG 10bit)
	18: [0x30314142] 'BA10' (RAW Bayer GRBG 10bit)
	19: [0x30314142] 'BA10' (RAW Bayer RGGB 10bit)
	20: [0x32314742] 'BG12' (RAW Bayer BGGR 12bit)
	21: [0x32314247] 'GB12' (RAW Bayer GBRG 12bit)
	22: [0x32314142] 'BA12' (RAW Bayer GRBG 12bit)
	23: [0x32314142] 'BA12' (RAW Bayer RGGB 12bit)
	
	Sensor size: 800x600 pixels
	Pixel Format: V4L2_PIX_FMT_YUV420
	Frame #10 will be saved!
	Length: 720896 	Bytesused: 720896 	Address: 0x453be0
	FPS[0]: 2.16
	Length: 720896 	Bytesused: 720896 	Address: 0x453bf0
	FPS[1]: 22.57
	Length: 720896 	Bytesused: 720896 	Address: 0x453c00
	FPS[2]: 22.52
	Length: 720896 	Bytesused: 720896 	Address: 0x453c10
	FPS[3]: 22.52
	Length: 720896 	Bytesused: 720896 	Address: 0x453be0
	FPS[4]: 22.52
	Length: 720896 	Bytesused: 720896 	Address: 0x453bf0
	FPS[5]: 22.52
	Length: 720896 	Bytesused: 720896 	Address: 0x453c00
	FPS[6]: 22.52
	Length: 720896 	Bytesused: 720896 	Address: 0x453c10
	FPS[7]: 22.50
	Length: 720896 	Bytesused: 720896 	Address: 0x453be0
	FPS[8]: 22.53
	Length: 720896 	Bytesused: 720896 	Address: 0x453bf0
	FPS[9]: 2.79
	------- Avg FPS: 22.53 --------
	
	TEST 8 - ( 640 x 480 )
	---- cap parameters -----
	width: 640
	height: 480
	v4l2 buffers: 4
	exposure: -999
	hflip: -1
	sensor_vflip: -1
	Mode: V4L2_MODE_VIDEO
	Driver: "sunxi-vfe"
	Card: "sunxi-vfe"
	Bus: "sunxi_vfe vfe.12"
	Version: 1.0
	Capabilities: 05000001
	Input: 0
	V4L2 pixel formats:
	0: [0x50323234] '422P' (planar YUV 422)
	1: [0x32315559] 'YU12' (planar YUV 420)
	2: [0x32315659] 'YV12' (planar YVU 420)
	3: [0x3631564E] 'NV16' (planar YUV 422 UV combined)
	4: [0x3231564E] 'NV12' (planar YUV 420 UV combined)
	5: [0x3136564E] 'NV61' (planar YUV 422 VU combined)
	6: [0x3132564E] 'NV21' (planar YUV 420 VU combined)
	7: [0x32314D48] 'HM12' (MB YUV420)
	8: [0x56595559] 'YUYV' (YUV422 YUYV)
	9: [0x55595659] 'YVYU' (YUV422 YVYU)
	10: [0x59565955] 'UYVY' (YUV422 UYVY)
	11: [0x59555956] 'VYUY' (YUV422 VYUY)
	12: [0x31384142] 'BA81' (RAW Bayer BGGR 8bit)
	13: [0x47524247] 'GBRG' (RAW Bayer GBRG 8bit)
	14: [0x47425247] 'GRBG' (RAW Bayer GRBG 8bit)
	15: [0x47425247] 'GRBG' (RAW Bayer RGGB 8bit)
	16: [0x30314742] 'BG10' (RAW Bayer BGGR 10bit)
	17: [0x30314247] 'GB10' (RAW Bayer GBRG 10bit)
	18: [0x30314142] 'BA10' (RAW Bayer GRBG 10bit)
	19: [0x30314142] 'BA10' (RAW Bayer RGGB 10bit)
	20: [0x32314742] 'BG12' (RAW Bayer BGGR 12bit)
	21: [0x32314247] 'GB12' (RAW Bayer GBRG 12bit)
	22: [0x32314142] 'BA12' (RAW Bayer GRBG 12bit)
	23: [0x32314142] 'BA12' (RAW Bayer RGGB 12bit)
	
	Sensor size: 640x480 pixels
	Pixel Format: V4L2_PIX_FMT_YUV420
	Frame #10 will be saved!
	Length: 462848 	Bytesused: 462848 	Address: 0x453be0
	FPS[0]: 3.28
	Length: 462848 	Bytesused: 462848 	Address: 0x453bf0
	FPS[1]: 30.12
	Length: 462848 	Bytesused: 462848 	Address: 0x453c00
	FPS[2]: 30.03
	Length: 462848 	Bytesused: 462848 	Address: 0x453c10
	FPS[3]: 30.03
	Length: 462848 	Bytesused: 462848 	Address: 0x453be0
	FPS[4]: 30.01
	Length: 462848 	Bytesused: 462848 	Address: 0x453bf0
	FPS[5]: 30.02
	Length: 462848 	Bytesused: 462848 	Address: 0x453c00
	FPS[6]: 30.07
	Length: 462848 	Bytesused: 462848 	Address: 0x453c10
	FPS[7]: 30.07
	Length: 462848 	Bytesused: 462848 	Address: 0x453be0
	FPS[8]: 30.03
	Length: 462848 	Bytesused: 462848 	Address: 0x453bf0
	FPS[9]: 4.14
	------- Avg FPS: 30.05 --------
	
	TEST 9 - ( 320 x 240 )
	---- cap parameters -----
	width: 320
	height: 240
	v4l2 buffers: 4
	exposure: -999
	hflip: -1
	sensor_vflip: -1
	Mode: V4L2_MODE_VIDEO
	Driver: "sunxi-vfe"
	Card: "sunxi-vfe"
	Bus: "sunxi_vfe vfe.12"
	Version: 1.0
	Capabilities: 05000001
	Input: 0
	V4L2 pixel formats:
	0: [0x50323234] '422P' (planar YUV 422)
	1: [0x32315559] 'YU12' (planar YUV 420)
	2: [0x32315659] 'YV12' (planar YVU 420)
	3: [0x3631564E] 'NV16' (planar YUV 422 UV combined)
	4: [0x3231564E] 'NV12' (planar YUV 420 UV combined)
	5: [0x3136564E] 'NV61' (planar YUV 422 VU combined)
	6: [0x3132564E] 'NV21' (planar YUV 420 VU combined)
	7: [0x32314D48] 'HM12' (MB YUV420)
	8: [0x56595559] 'YUYV' (YUV422 YUYV)
	9: [0x55595659] 'YVYU' (YUV422 YVYU)
	10: [0x59565955] 'UYVY' (YUV422 UYVY)
	11: [0x59555956] 'VYUY' (YUV422 VYUY)
	12: [0x31384142] 'BA81' (RAW Bayer BGGR 8bit)
	13: [0x47524247] 'GBRG' (RAW Bayer GBRG 8bit)
	14: [0x47425247] 'GRBG' (RAW Bayer GRBG 8bit)
	15: [0x47425247] 'GRBG' (RAW Bayer RGGB 8bit)
	16: [0x30314742] 'BG10' (RAW Bayer BGGR 10bit)
	17: [0x30314247] 'GB10' (RAW Bayer GBRG 10bit)
	18: [0x30314142] 'BA10' (RAW Bayer GRBG 10bit)
	19: [0x30314142] 'BA10' (RAW Bayer RGGB 10bit)
	20: [0x32314742] 'BG12' (RAW Bayer BGGR 12bit)
	21: [0x32314247] 'GB12' (RAW Bayer GBRG 12bit)
	22: [0x32314142] 'BA12' (RAW Bayer GRBG 12bit)
	23: [0x32314142] 'BA12' (RAW Bayer RGGB 12bit)
	
	Sensor size: 320x240 pixels
	Pixel Format: V4L2_PIX_FMT_YUV420
	Frame #10 will be saved!
	Length: 118784 	Bytesused: 118784 	Address: 0x453be0
	FPS[0]: 3.20
	Length: 118784 	Bytesused: 118784 	Address: 0x453bf0
	FPS[1]: 30.15
	Length: 118784 	Bytesused: 118784 	Address: 0x453c00
	FPS[2]: 30.04
	Length: 118784 	Bytesused: 118784 	Address: 0x453c10
	FPS[3]: 30.02
	Length: 118784 	Bytesused: 118784 	Address: 0x453be0
	FPS[4]: 30.03
	Length: 118784 	Bytesused: 118784 	Address: 0x453bf0
	FPS[5]: 30.03
	Length: 118784 	Bytesused: 118784 	Address: 0x453c00
	FPS[6]: 30.03
	Length: 118784 	Bytesused: 118784 	Address: 0x453c10
	FPS[7]: 30.01
	Length: 118784 	Bytesused: 118784 	Address: 0x453be0
	FPS[8]: 30.04
	Length: 118784 	Bytesused: 118784 	Address: 0x453bf0
	FPS[9]: 9.92
	------- Avg FPS: 30.04 --------
	
	TEST 10 - ( 176 x 144 )
	---- cap parameters -----
	width: 176
	height: 144
	v4l2 buffers: 4
	exposure: -999
	hflip: -1
	sensor_vflip: -1
	Mode: V4L2_MODE_VIDEO
	Driver: "sunxi-vfe"
	Card: "sunxi-vfe"
	Bus: "sunxi_vfe vfe.12"
	Version: 1.0
	Capabilities: 05000001
	Input: 0
	V4L2 pixel formats:
	0: [0x50323234] '422P' (planar YUV 422)
	1: [0x32315559] 'YU12' (planar YUV 420)
	2: [0x32315659] 'YV12' (planar YVU 420)
	3: [0x3631564E] 'NV16' (planar YUV 422 UV combined)
	4: [0x3231564E] 'NV12' (planar YUV 420 UV combined)
	5: [0x3136564E] 'NV61' (planar YUV 422 VU combined)
	6: [0x3132564E] 'NV21' (planar YUV 420 VU combined)
	7: [0x32314D48] 'HM12' (MB YUV420)
	8: [0x56595559] 'YUYV' (YUV422 YUYV)
	9: [0x55595659] 'YVYU' (YUV422 YVYU)
	10: [0x59565955] 'UYVY' (YUV422 UYVY)
	11: [0x59555956] 'VYUY' (YUV422 VYUY)
	12: [0x31384142] 'BA81' (RAW Bayer BGGR 8bit)
	13: [0x47524247] 'GBRG' (RAW Bayer GBRG 8bit)
	14: [0x47425247] 'GRBG' (RAW Bayer GRBG 8bit)
	15: [0x47425247] 'GRBG' (RAW Bayer RGGB 8bit)
	16: [0x30314742] 'BG10' (RAW Bayer BGGR 10bit)
	17: [0x30314247] 'GB10' (RAW Bayer GBRG 10bit)
	18: [0x30314142] 'BA10' (RAW Bayer GRBG 10bit)
	19: [0x30314142] 'BA10' (RAW Bayer RGGB 10bit)
	20: [0x32314742] 'BG12' (RAW Bayer BGGR 12bit)
	21: [0x32314247] 'GB12' (RAW Bayer GBRG 12bit)
	22: [0x32314142] 'BA12' (RAW Bayer GRBG 12bit)
	23: [0x32314142] 'BA12' (RAW Bayer RGGB 12bit)
	
	Sensor size: 176x144 pixels
	Pixel Format: V4L2_PIX_FMT_YUV420
	Frame #10 will be saved!
	Length: 40960 	Bytesused: 40960 	Address: 0x453be0
	FPS[0]: 3.23
	Length: 40960 	Bytesused: 40960 	Address: 0x453bf0
	FPS[1]: 30.13
	Length: 40960 	Bytesused: 40960 	Address: 0x453c00
	FPS[2]: 30.04
	Length: 40960 	Bytesused: 40960 	Address: 0x453c10
	FPS[3]: 30.03
	Length: 40960 	Bytesused: 40960 	Address: 0x453be0
	FPS[4]: 30.03
	Length: 40960 	Bytesused: 40960 	Address: 0x453bf0
	FPS[5]: 30.03
	Length: 40960 	Bytesused: 40960 	Address: 0x453c00
	FPS[6]: 30.03
	Length: 40960 	Bytesused: 40960 	Address: 0x453c10
	FPS[7]: 30.03
	Length: 40960 	Bytesused: 40960 	Address: 0x453be0
	FPS[8]: 30.03
	Length: 40960 	Bytesused: 40960 	Address: 0x453bf0
	FPS[9]: 13.27
	------- Avg FPS: 30.04 --------

