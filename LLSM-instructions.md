## LLSM processing pipeline instructions
### Get ROI from Fiji
1. Open the MIP of the corresponding file on Fiji
2. Rotate image 90 degrees to the left
![](./resources/Pasted%image%20261027164433.png)
3. Open ROI manager
![](./resorces/Pasted%image%20261027164748.png)
This window should pop up
![](./resources/Pasted%image%20261027165323.png)
4. Use the rectangle tool to draw out the ROI
![](./resources/Pasted%image%20261027165406.png)
5. While the ROI bounding box is selected (make sure that you see this yellow box), click Add (the first button in the ROI manager window)
![](.resources//Pasted%image%20261027165508.png)
6. There should be a chain of numbers on the left side of the window. That's the ROI. You can add more ROI with the same method. If you have more than 1, you should rename them here, if not, you can rename it when saving the file. 
7. Go to more>> (bottom right), and click save
![](./resources/Pasted%image%20261027171103.png)
8. Upload it to rockfish. The file should be very small and you could do it on login node (at least if you only have 1 ROI)

**while in Fiji:**
- check the total time points (or the time point you want to process)
- check which channels you want to process and their channel number
- check how many channels are there in total you plan on doing deconv

### Running the pipeline

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

`PSF_FILE`=("absolute path to psf file of the first channel"

"after a new line, absolute path to the psf of the second channel")

*if you are using deconv, you have to input the same number psf files as the number of channels that the original czi file has, even if you are not processing all of them*

DECON_ITER: deconv iteration number


example
![](./resources/Pasted%image%20261027180108.png)
