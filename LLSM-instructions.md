## LLSM processing pipeline instructions
### Get ROI from Fiji
1. Open the MIP of the corresponding file on Fiji
2. Rotate image 90 degrees to the left
<img src="./resources/Pasted%20image%2020261027164433.png" width="450">
3. Open ROI manager
<img src="./resorces/Pasted%20image%2020261027164748.png" width="450">
This window should pop up
<img src="./resources/Pasted%20image%2020261027165323.png" width="450">
4. Use the rectangle tool to draw out the ROI
<img src="./resources/Pasted%20image%2020261027165406.png" width="450">
5. While the ROI bounding box is selected (make sure that you see this yellow box), click Add (the first button in the ROI manager window)
<img src=".resources//Pasted%20image%2020261027165508.png" width="450">
6. There should be a chain of numbers on the left side of the window. That's the ROI. You can add more ROI with the same method. If you have more than 1, you should rename them here, if not, you can rename it when saving the file. 
7. Go to more>> (bottom right), and click save
<img src="./resources/Pasted%20image%2020261027171103.png" width="450">
8. Upload it to rockfish. The file should be very small and you could do it on login node (at least if you only have 1 ROI)

**while in Fiji:**
- check the total time points (or the time point you want to process)
- check which channels you want to process and their channel number
- check how many channels are there in total you plan on doing deconv

### Running the CLI

(Starts at line 22, also remember to change the time and number of cpus for sbatch settings. 4 hours should be fine for anything without deconv)

`INPUT`: absolute path to input czi file

`ROI_LIST`: absolute path to roi file, could technically be a list of files, but never tested

`OUTPUT_PATH`: absolute path to the parent directory of the output file

`OUTPUT_NAME`: name of the output file, without file extension


`CH_START`: first channel to be processed, starts from 0, order is the same as the channel order shown in Fiji (and napari)

`CH_END`: last channel to be processed, exclusive

*eg: CH_START=0 and CH_END=1 will be processing the first channel only, CH_START=0 and CH_END=2 will process the first two channels*

**processing multiple channels will merge them together, and you won't be able to change color separately on napari**


`T_START`: starting time point, starts from 0

`T_END`: ending time point, exclusive


`DECONV`: type TRUE or FALSE without quotation marks

`PSF_FILE`=("absolute path to psf file of the first channel, start a new line, and list absolute path to the psf of the second channel")

*if you are using deconv, you have to input the same number psf files as the number of channels that the original czi file has, even if you are not processing all of them*

DECON_ITER: deconv iteration number


example

`# Things to change
INPUT="/home/hcao34/data_slajoie1/LLSM_File_Input/BMDM/MP/MP-0.4/PBS/MP-PBS-03.czi"
ROI_LIST="/home/hcao34/roi/EH031_MP-0.4_PBS-03.roi"
OUTPUT_PATH="/home/hcao34/data_slajoie1/LLSM_File_Output/BMDM/MP/MP-0.4/PBS/EH031_PROCESSING"
OUTPUT_NAME="EH031_MP-0.4_PBS-03_CROPPED_DESKEWED_CHANNEL_1"
CH_START=0
CH_END=1
T_START=0
T_END=100

DECONV=FALSE
PSF_FILE=(
"/home/hcao34/LLSM_approx_PSFs/SiR_Actin_FarRed_APPROX_RAW_Y30deg_145nm_200nm.tif"
"/home/hcao34/LLSM_approx_PSFs/pHrodo_Red_APPROX_RAW_Y30deg_145nm_200nm.tif")
DECON_ITER=1

# End of things to change`

This file has 2 channels, I am only doing the first channels and for 10 time points. I am doing 1 iteration of deconvolution, and the psf path that corresponds to it should be on the first line (first psf file. The second file doesn's matter (because I am not processing a second channel), but it has to be there. 
