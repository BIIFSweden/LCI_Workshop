[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# :microscope: LCI Microscopy Course: Image Analysis Workshop 2024

Welcome to the image analysis workshop organized by the [Bioimage Informatics facility](https://www.scilifelab.se/units/bioimage-informatics/) & the [Live Cell Imaging facility](https://ki.se/en/bionut/live-cell-imaging-core-facility-lci) as part of the LCI Microscopy course 2024! During this workshop you will:

- [x] See typical examples of what can be done with image analysis and the limitations of each example
- [x] Understand which image artefacts can be corrected by image analysis in some cases but are easier to correct before acquisition, e.g., uneven illumination, noise
- [x] Understand which image artefacts cannot be corrected by image analysis, e.g., saturation, underexposure, bleedthrough

To follow up on the examples presented in this workshop, download the code available on this GitHub page (you can do that by clicking on the green button above). In addition, you also have to download Fiji from the following [link](https://fiji.sc/). The installation of additional required plugins will be detailed in the following sections.

:bookmark: **Tip**: To open an image in Fiji, go to "File->Open". Then, browse the image of interest. If the Bio-Formats plugin interface appears, check the import options and press "OK."

:bookmark: **Tip**: You can open a script in Fiji by dragging and dropping a file in the main interface or through "Plugins->Macros->Edit...". Then, browse the code to be used in each session.

:people_holding_hands: **Group work**: You will be randomly assigned to breakout rooms to work in groups for each activity. Results should be reported in the shared document (link to be sent during the workshop).

## :alarm_clock: Schedule

* **10:20-11:20** Correction of uneven illumination
* **11:25-12:20** Handling noise
* **12:20-13.20** Lunch break
* **13:20-14:20** Image artifacts that cannot be fixed
* **14:25-15:20** Nuclei & Cell Segmentation

## :muscle: Let's start!

## 1) Uneven illumination - how to correct

Uneven illumination can be due to different factors, e.g., microscope settings, sample artefacts (samples that are not flat), shading or vignetting (attenuation of the pixel intensity from the centre of the optical axis to the edges). 

Uneven illumination can cause discontinuities in whole slide images, background bleaching in time-lapse fluorescent images and compromise downstream analysis. In this workshop, we will explore two algorithms that can be used to correct such artefacts.
* [Rolling-ball](https://imagej.net/plugins/rolling-ball-background-subtraction) algorithm
* [BaSiC](https://github.com/marrlab/BaSiC#imagejfiji-plugin) plugin (click on the link to follow installation instructions)

:arrow_right: Images to be used in this session are located in "../images/illumination_correction/"

Join your breakout room and then follow the instructions corresponding to your group:

:people_holding_hands: **Group 1:** Illumination correction via rolling ball algorithm
* Open an image from the **"stitched"** folder
* Create a copy of the original image ("Image->Duplicate...")
* Go to "Process->Subtract Background..."
* Choose the radius of the rolling ball algorithm and press OK

Repeat these steps for the other files in the **"stitched"** folder, each corresponding to a different overlap (1%, 5% and 10%). You can also try to correct individual tiles from the "tiles" folder. Try different values of the radius parameter.

:bulb: What happens as the size of the radius increases?

:bulb: What happens as the overlap increases?

:people_holding_hands: **Groups 2 and 3:** Illumination correction via BaSiC plugin
* Install the BaSiC plugin via Fiji update sites (instructions above)
* Open the script "Illumination_Basic.ijm"
* Update parameter values (lines 12 to 15)
	- For "img2_" set parameters to:
	```
	overlap = 1; // choose 1, 5 or 10
	grid_size_x = 5;
	grid_size_y = 3;
	file_name = "img";
	```
	- For "WSI_Brain" set parameters to:
	```
	overlap = 10;
	grid_size_x = 9;
	grid_size_y = 6;
	file_name = "C3-BrainSection";
	```
* Press the "Run button". A Dialog window will appear, and you should browse the folder named "uncorrected" inside the reference folder (e.g. "../images/illumination_correction/tiles/img2_1pc/uncorrected/").
	- **Group 2:** run the script for "img2" with different overlap values ("img2_1pc", "img2_5pc" and "img2_10pc")
	- **Group 3:** run the script for "img2_10pc" and "WSI_Brain"

**Note:** Corrected stitched images are saved inside the reference folder

:bulb: Compare the uncorrected *vs* corrected images and discuss how the corrected image was improved (*or not*) for each example.

:bulb: What happens as the overlap increases? (Group 2)

:bulb: What happens as the number of tiles increases? (Group 3)

:people_holding_hands: **Group 4:** Uneven focus on z-stacks
* Open the image of interest in Fiji (from folder "../images/illumination_correction/uneven_focus/");
* Inspect the z-stack. What is wrong?
* Create a copy of the reference stack ("Image->Duplicate", remember to select the "Duplicate stack" option)
* Segment via thresholding ("Image->Adjust->Threshold..."). Keep the resulting segmented stack.
* Select the original stack and apply a maximum intensity projection (MIP) via "Image->Stacks->Z project..."
* Create two copies of the projected image. 
	- Apply the rolling ball algorithm ("Process->Subtract Background...") in one of them
* Segment both copies via thresholding

:bulb: Compare the segmentation results of 1) the z-stack, 2) the MIP, and 3) the corrected MIP.

**Note:** To help your analysis, you can compare the intensity profiles of the reference images: Draw a straight line from left to right in the reference image, then go to "Analyze->Plot Profile"

## 2) Handling noise

Microscopic images may be affected by different types of noise: dark noise from sensors, shot noise (due to inherent nature of light), readout noise (due to amplification and conversion of the signal). There are different image processing techniques that can be used to denoise images. In this workshop, we will focus on convolution filters. The image below shows the effect of the averaging filter during acquisition.

![](images/averaging_filter.png?raw=true "Screenshot")

Each group should take a specific reference image and generate the output measures using the script "Noise.ijm" script.

:arrow_right: Images to be used in this session are located in "../images/noise/". When processing the "Tissue_" images, split the channels and select the DAPI channel before running the script ("Image->Color->Split Channels").

:people_holding_hands: Group assignment:
* Group 1: images "Nuclei_no_avg.nd2" and "Tissue_no_avg.nd2"
* Group 2: images "Nuclei_avg_2x.nd2" and "Tissue_2x_avg.nd2"
* Group 3: images "Nuclei_avg_4x.nd2" and "Tissue_4x_avg.nd2"
* Group 4: images "Nuclei_avg_8x.nd2" and "Tissue_8x_avg.nd2"
* Group 5: images "Nuclei_avg_16x.nd2" and "Tissue_16x_avg.nd2"

:arrow_right: Instructions to run the script and generate the measures: 
* Open the image of interest in Fiji;
* Open and run the script "Noise.ijm";
* Copy the measures from the "Summary" table to the shared file 
* Inspect the Results table and the Roi Manager tool
* Take some time to analyze the script!

**Testing the Median filter**: uncomment line #20 of the script (by removing the "//" at beginning of the line, see below). Then re-run the script for the same reference images. Copy the measures to the shared file and compare the new measures with the previous values.

```
run("Median...", "radius=4");
```

:bulb: What happens as the size of the averaging filter increases?

:bulb: What happens if we don't use the median filter?

## 3) Image artifacts

Some image artifacts cannot be fixed by image analysis and may compromise intensity and shape measurements. Examples can be seen in the images below, such as, bleedtrough, saturation, under exposure and soft focus.

### 3.1) Bleedtrough

![](images/bleedtrough.png?raw=true "Screenshot")

### 3.2) Saturation & Under exposure

<img src="images/saturation_vs_underexposure.png" height=89% width=89% ></a>

### 3.3) Soft focus

<img src="images/out_focus.png" height=58% width=58% ></a>

### Hands-on exercise

You will work in groups to investigate how each artifact can affect image quantification.

:arrow_right: Images to be used in this session are located in "../images/image_artifacts/"

:people_holding_hands: Group assignment:
* Group 1: image "../images/image_artifacts/sequential.nd2"
* Group 2: image "../images/image_artifacts/simultaneous.nd2"
* Group 3: image "../images/image_artifacts/seq_saturated.nd2"
* Group 4: image "../images/image_artifacts/seq_undexposed.nd2"
* Group 5: image "../images/image_artifacts/soft_focus.nd2"

:arrow_right: Instructions to run the script: 
* Open the image of interest in Fiji;
* Open and run the script "ImageArtifacts.ijm";
* From the "Log" window copy the values of the "total segmented area" and "average intensity" of each channel (green and red)
* Take some time to analyze the script!

Wait for all groups to complete the task and add results in the shared file. Then discuss:

:bulb: How are the measures affected by each artifact?

:bulb: How to avoid such artifacts during acquisition?

## 4) Nuclei & Cell Segmentation

The segmentation of nuclei and cells is the starting point of several image analysis tasks in microscopy. In this assignment we will use Fiji to create a pipeline for the segmentation of cells and nuclei. Be inspired by the image below! 

You can choose to work with your own images, acquired during the LCI Microscopy course, or, you can work with the images located in the folder "../images/noise/". 

![](images/cell_nuclei_segmentation.png?raw=true "Screenshot")

:arrow_right: :bookmark: Notes & Tips
* Test the different thresholding algorithms in Fiji ("Image->Adjust->Threshold...");
* Remember that the background subtraction can improve the segmentation results;
* Pre-processing: you can find several convolution filters in "Process->Filters";
* Remember to create a copy of the original image in case you want to quantify pixel intensity
* If thresholding does not perform well, you can try [StarDist](https://github.com/stardist/stardist-imagej)
	- Available as a plugin for FIJI. Two trained models: fluorescent nuclei and H&E stained images
* Activate the macro recording function "Plugins->Macros->Record..." in Fiji to save all the functions and plugins that you use. Then try to create your own script!

## Acknowledgements

* :microscope: **Image Acquisition** *by Gabriela Imreh*
* :thought_balloon: **Proposal & Design** *by Gisele Miranda & Sylvie Le Guyader*
* :hammer_and_wrench: **Implementation** *by Gisele Miranda*
* :clapper: **Organization** *by BioImage Informatics facility*
