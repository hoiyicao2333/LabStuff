# LLSM processing pipeline instructions

##  CLI slurm job for a single file

(Starts at line 22, also remember to change the time, output log location, and number of cpus for sbatch settings. 4 hours should be fine for anything without deconv)

| Variable      | Description                                                                                                                                                                                                                                                                          |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `INPUT`       | Absolute path to the input CZI file                                                                                                                                                                                                                                                  |
| `ROI_LIST`    | Absolute path to the ROI file. Could technically be a list of files, but this has never been tested. (See: click here to see how to get an ROI file in Fiji)                                                                                                                         |
| `OUTPUT_PATH` | Absolute path to the parent directory of the output file                                                                                                                                                                                                                             |
| `OUTPUT_NAME` | Name of the output file, without file extension                                                                                                                                                                                                                                      |
| `CH_START`    | First channel to be processed. Starts from 0; order is the same as the channel order shown in Fiji (and napari)                                                                                                                                                                      |
| `CH_END`      | Last channel to be processed, exclusive. E.g., `CH_START=0` and `CH_END=1` processes the first channel only; `CH_START=0` and `CH_END=2` processes the first two channels                                                                                                            |
| `T_START`     | Starting time point. Starts from 0                                                                                                                                                                                                                                                   |
| `T_END`       | Ending time point, exclusive                                                                                                                                                                                                                                                         |
| `DECONV`      | Type `TRUE` or `FALSE` without quotation marks                                                                                                                                                                                                                                       |
| `PSF_FILE`    | Absolute path to the PSF file of the first channel. Start a new line and list the absolute path to the PSF of the second channel. If you are using deconvolution, you must input as many PSF files as the original CZI file has channels, even if you are not processing all of them |
| `DECON_ITER`  | Deconvolution iteration number                                                                                                                                                                                                                                                       |

### example: 

<img src="resources/Pasted%20image%2020261027180108.png" width="900">

This file has 2 channels, I am only doing the first channels and for 10 time points. I am doing 1 iteration of deconvolution, and the psf path that corresponds to it should be on the first line (first psf file. The second file doesn's matter (because I am not processing a second channel), but it has to be there. 

## Batch processing

`-in` 	txt file that lists absolute path to all the file in order

`-roi` 	list of roi file for each input, in order of inputs

`-o` 	(optional) path list of path to the output directory, in order of inputs. Defaults to the mirror path in LLSM_File_Output

### example: 
`sbatch scr4_slajoie1/LLSMBatch.sh -in temp/input.txt -roi roi.txt`

temp/input.txt

<img src="resources/Pasted%20image%2020261029214532.png" width="900">

temp/roi.txt

<img src="resources/Pasted%20image%2020261029214656.png" width="400">

without giving an output.txt, the outputs would be 
1. /home/hcao34/data_slajoie1/LLSM_File_Output/hypothetical_dir1/PM_ADDED-03_CROPPED_DESKEWED_BATCH.ome.zarr
2. /home/hcao34/data_slajoie1/LLSM_File_Output/hypothetical_dir1/PRE-NIST-03_CROPPED_DESKEWED_BATCH.ome.zarr
3. /home/hcao34/data_slajoie1/LLSM_File_Output/hypothetical_dir2/PBS-01_CROPPED_DESKEWED_BATCH.ome.zarr

To specify the output dir, attach an additional file that lists its path
`sbatch scr4_slajoie1/LLSMBatch.sh -in temp/input.txt -roi roi.txt -o temp/output.txt`

temp/output.txt

<img src="resources/Pasted%20image%2020261029220147.png" width="300">

This would put the output files to 
1. /home/hcao34/alt_path1/PM_ADDED-03_CROPPED_DESKEWED_BATCH.ome.zarr
2. /home/hcao34/alt_path2/PRE-NIST-03_CROPPED_DESKEWED_BATCH.ome.zarr
3. /home/hcao34/alt_path3/PBS-01_CROPPED_DESKEWED_BATCH.ome.zarr


## Get ROI from Fiji
1. Open the MIP of the corresponding file on Fiji
2. Rotate image 90 degrees to the left
<img src="./resources/Pasted%20image%2020261027164433.png" width="250">
3. Open ROI manager
<img src="./resources/Pasted%20image%2020261027164748.png" width="250">
This window should pop up
<img src="./resources/Pasted%20image%2020261027165323.png" width="200">
4. Use the rectangle tool to draw out the ROI
<img src="./resources/Pasted%20image%2020261027165406.png" width="450">
5. While the ROI bounding box is selected (make sure that you see this yellow box), click Add (the first button in the ROI manager window)
<img src="./resources/Pasted%20image%2020261027165508.png" width="450">
6. There should be a chain of numbers on the left side of the window. That's the ROI. You can add more ROI with the same method. If you have more than 1, you should rename them here, if not, you can rename it when saving the file. 
7. Go to more>> (bottom right), and click save
<img src="./resources/Pasted%20image%2020261027171103.png" width="250">
8. Upload it to rockfish. The file should be very small and you could do it on login node

**Also check these in Fiji if you are running the CLI for single file:**
- check the total time points (or the time point you want to process)
- check which channels you want to process and their channel number
