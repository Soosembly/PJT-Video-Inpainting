# Web UI Tutorial for Video Inpainting

## Prepare the video as input for the WebUI.
**The structure of _"test-video.mp4"_ must be like this : </br>
You need to edit and prepare your video so that the object or area you want to inpaint appears in the first frame.
If there are no objects or areas in the first frame that you want to inpaint, you won't be able to do so at this time.**

<p align="center"><img src="./img/capture_test_video.png"
 width = "300" height = "300" alt="test-video.mp4"/> </p>


# Use WebUI get test_video data
## 1. Upload your video
Upload your video in one of the two ways 'A' or 'B' below.
### A. Drag a file into a block
 <p align="center"><img src="./img/first frame load.gif" alt="first frame load by drag"/> </p>

### B. Click the block to upload a test video
<p align="center"><img src="./img/click upload.gif" alt="click first frame load by click"/> </p>


## 2. Segment the area you want using SAM

Click with your mouse on the area you want to inpaint. </br>
The segment-anything model (called SAM) comes into play, automatically marking areas. </br>
Click the desired area as precisely as possible until all the desired areas are displayed.

In our example, we want to erase the logo, so we mark the logo on the clothing as an area.
<p align="center"><img src="./img/samclick.gif"  width = "400" height = "350" alt="sam click"/> </p>

We'll discuss specific parameter settings and buttons within the **Click tab** in more detail below.
<p align="center"><img src="./img/CLICK TAB.png" width = "400" height = "400" alt="sam click tab"/> </p>

- **aot_model**: Select the DeAOT/AOT to use for tracking and propagation.

- **sam_gap**: Controls the frequency of segmentation to add newly appearing objects at specified frame intervals. Increasing this value reduces the frequency of discovering new targets, but significantly increases inference speed.

- **points_per_side**: Used to control the number of points per face used to generate a mask by sampling a grid over the image. Increasing the size improves the ability to detect small objects, while larger targets can be segmented more finely.

- **max_obj_num**: Limits the maximum number of objects that DeAOT can detect and track. 
Higher number of objects increases memory usage, approximately 16GB of memory can handle up to 255 objects.


## 3. Segment-and-Track-Anything using DeAOT
If you are satisfied with the segmented area in the first frame, click the **[Start Tracking!]** button. 

This will take the region in the first frame and automatically track it using the DeAOT model to display the region in the entire frame.
It then creates a video of the masked frames and previews them in the top right.
You can watch this video to see if the region is masked well.


<p align="center"><img src="./img/sam tracking.gif" width = "400" height = "400" alt="sam tracking"/> </p>

## 4. Inpainting using E2FGVI
Check the masked video and if the area is well masked, press the **[start inpainting!]** button.

<p align="center"><img src="./img/inpainting.gif" alt="sam tracking"/> </p>

## 5. Download
You can download three materials here.
1. a masked (or segmented) video: _**test_video_seg1.mp4**_
2. an inpainting video: _**test_video_inpainting_h264.mp4**_
3. a compressed file of the masked frames of the video: _**test_video_pred_mask.zip**_
