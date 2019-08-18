# Optical Sequencing Data Analysis Apps
This set of five MATLAB apps is for guided data analysis of optical sequencing experiments. From raw Raman spectrosocpy measurements, these apps are used for determining content in DNA gene blocks and applying bioinformatic tools for gene identification. Prior to use of these apps, two sets of Raman spectroscopy measurements should have been collected: (1) calibration blocks and (2) gene blocks.

Input/output requirements for using the apps are as follows...

App 1: Signal Processing and Normalization ----------------------------------------

Inputs: This app reads in raw Raman spectroscopy measurements. Due to the varying output formats and filetypes of different spectrometers, the user must generate and format the data file according to specifications. This file must be an Excel .xls or .xlsx type. Each sample must have two tabs allotted for it: shift data on a tab labeled ‘(sample name)_shift’ with one sample per consecutive column, and intensity data on a tab labeled ‘(sample name)_int’ with one sample per consecutive column. The user must specify which sample is to be analyzed by entering the correct sample name into the app.

Outputs: The output filename is ‘(Calibration or gene) blocks final processed and normalized data.xlsx’. It is formatted in the same way as the input file – each sample has two tabs allotted for it: shift data on a tab labeled ‘(sample name)_shift’ with one sample per consecutive column, and intensity data on a tab labeled ‘(sample name)_int’ with one sample per consecutive column.

App 2: Intensity Measurements ----------------------------------------

Inputs: Output from App 1.

Outputs: The output filename is ‘(Calibration or gene) blocks intensities.xlsx’. It has tree tabs with information and data. Sample names with one per consecutive row are on tab ‘SampleNames’. Mean intensity values (averaged from each of the replicates) are on tab ‘Intensities_mean’ with one sample per consecutive row corresponding to their names, and four columns – one for each of the nucleobases in the order A-G-C-T. Standard deviation of intensity values (deviations from each of the replicates) are on tab ‘Intensities_std’ with one sample per consecutive row corresponding to their names, and four columns – one for each of the nucleobases in the order A-G-C-T.

App 3: Calibrations ----------------------------------------
* Note this app is only run on the calibration blocks, not the gene blocks

Inputs: There are two inputs for this app. The first input is the calibration key (i.e., the actual contents of the calibration blocks). The user must generate and format this file according to specifications. This file must be an Excel .xls or .xlsx type. The file has two tabs with information and data. Sample names with one per consecutive row are on tab ‘SampleNames’. The content of the samples are on tab ‘Key’ with one sample per consecutive row corresponding to their names, and four columns – one for each of the nucleobases in the order A-G-C-T. The second input is the output from App 2 (for calibration blocks).

Outputs: The output filename is ‘Calibrations.xlsx’. The only data used from the calibrations in the next apps is the ‘AvgFits’ tab. It has the calibration data for each nucleobase in consecutive columns. The first row is the slope, and the second row is the intercept (all intercepts will be zero if the ‘lock intercept at zero’ option is selected). This app also outputs information that users might find necessary for plotting the trends that are displayed in the app: ‘SortSample…’ tab names show data sorted for each sample, while ‘CompiledSample…’ tab names show data sorted for each content value. ‘SortSampleNames’ shows the sample names sorted by increasing content for each nucleobase (columns in the order A-G-C-T), ‘SortSampleContent’ shows the sorted content, ‘SortSampleIntensities_mean’ shows the sorted mean intensities, and ‘SortSampleIntensities_std’ shows the sorted standard deviation of intensities. ‘CompiledSampleContent’ shows the sorted content values for each nucleobase (columns in the order A-G-C-T, samples that had equal content values combined together), ‘CompiledSampleIntensities_mean’ shows the sorted mean intensities, and ‘CompiledSampleIntensities_std’ shows the sorted standard deviation of intensities. Tabs called ‘LstdFits’ and ‘RstdFits’ are also included in the output. These are the left and right standard deviation linear fits, displayed as the green lines on the app. They are not used in the further analysis.

App 4: Content Prediction ----------------------------------------
* Note this app is only run on the gene blocks, not the calibration blocks

Inputs: There are two inputs for this app. The first input is the output from App 3. The second input is the output from App 2 (for gene blocks). The user must also specify the length of the blocks.

Outputs: The output filename is ‘Content prediction.xlsx’. It has three tabs with information and data used for the final app. ‘SampleNames’ lists the sample names in consecutive rows. ‘BlockLengths’ lists the block lengths in consecutive rows corresponding to the names. ‘FinalPredictCont’ lists the predicted content in consecutive rows corresponding to the names, with each nucleobase content being in consecutive columns in the order A-G-C-T. This app also outputs information that users might find helpful for analyzing how the predictions were made. ‘RAWCont_avg’, ‘RAWCont_Lstd’, and ‘RAWCont_Rstd’ show the raw predicted content (in the same format as the ‘FinalPredictCont’ tab) using the average, left standard deviation, and right standard deviation fits respectively. ‘NormRAWCont’ shows the predicted content normalized to equal 1 for the four nucleobases. ‘MultNormRAWCont’ shows the normalized content multiplied by the block lengths. ‘RoundMultNormRAWCont’ shows the rounded integer values for predicted content. ‘CorrRoundMultNormRAWCont’ shows the corrected final integer values for predicted content.

App 5: Diagnostics ----------------------------------------

Inputs: There are several inputs and settings to adjust for this app. The first input is the output from App 4. The second input is the FASTA database file with genes/genetic biomarker sequences. The following settings can be adjusted as necessary:
•	Database type: ‘resistance’, ‘genetic’, ‘cancer’, ‘other’ – specifying the type of FASTA database
•	Penalty score: The score given to a gene when no content matches are found for a particular block during the alignment. A value of 0.1 is recommended for starting and for normal analysis.
•	Thresholding multiplier: Used to increase the sensitivity of thresholding, or how many genes are removed from the analysis after each successive block. A value of >1 corresponds to a LESS SENSITIVE state (i.e., more genes remaining per block). A value of <1 corresponds to a MORE SENSITIVE state (i.e., fewer genes remaining per block).
•	Thresholding factors: Can toggle thresholding using each of the six probability factors and the content score. For most applications, all factors and content score should be used.
•	Entropy screening:  Select to use (‘Yes’) or not use (‘No’) entropy screening, which removes blocks with a high number of permutations (e.g., 10-mer blocks with the 2-2-3-3 content have >25,000 permutations). The cutoff can be adjusted as well.
•	Random mixing of blocks: ‘Yes’ randomly mixes the blocks before running the BOCS algorithm. ‘No’ runs BOCS with the order in which they are read into the app. The user can also specify the order they want the blocks to be used by entering a comma-separated list of integers correspond to the block numbers.
•	Tracking level: This value specifies how many of the top genes are output.

Outputs: This final diagnostics app outputs a text file, with filename ‘BOCS analysis – (database type) database.txt’. The file has multiple sections with data from the analysis. The ‘RUNTIME’ section displays how long the analysis took in minutes. The ‘SETTINGS’ section displays the user-defined settings. ‘BLOCKS_INPUT’ shows the blocks read into the app, with columns in the following order: block number, block length, permutations, content in the order A-G-C-T. ‘BLOCKS_ANALYZED’ shows the blocks (after entropy screening, if it is used) that were actually used in the analysis, with columns in the following order: block number, block length, permutations, content in the order A-G-C-T. ‘REM_GENES_SPECIFICITY’ shows block number, how many genes from the database remain after each block, and the calculated specificity. ‘CLASS’ shows the ranking of genes by sub-class (and the more broad class if the ‘resistance’ database type is used) for the top n gene sub-classes, where n is the tracking level. Columns are in the following order: block number, n unique sub-classes, combined scores for those n sub-classes, n unique classes, combined scores for those n classes. The ‘CS_ALL’ section displays the content score for each of the genes (one gene per row) in the database as each of the blocks is read (successive block scores in each consecutive column, where numbering starts at zero).

Within the 'Sample FASTA databases' folder, there are four FASTA files available for running with App 5...
1. The MEGARes antibiotic resistance database
Lakin, S. M.; Dean, C.; Noyes, N. R.; Dettenwanger, A.; Ross, A. S.; Doster, E.; Rovira, P.; Abdo, Z.; Jones, K. L.; Ruiz, J.; et al. MEGARes: An Antimicrobial Resistance Database for High Throughput Sequencing. Nucleic Acids Res. 2017, 45 (D1), D574–D580.
2. The COSMIC cancer database
Forbes, S. A.; Beare, D.; Gunasekaran, P.; Leung, K.; Bindal, N.; Boutselakis, H.; Ding, M.; Bamford, S.; Cole, C.; Ward, S.; et al. COSMIC: Exploring the World’s Knowledge of Somatic Mutations in Human Cancer. Nucleic Acids Res. 2015, 43 (D1), D805–D811.
3. A custom compiled database of other genetic diseases
Inspired by the NIH Undiagnosed Diseases Network
4. The Pseudomonas aeruginosa PAO1 genome (with an OXA class D beta-lactamase gene from the MEGARes database inserted)
Stover, C. K.; Pham, X. Q.; Erwin, A. L.; Mizoguchi, S. D.; Warrener, P.; Hickey, M. J.; Brinkman, F. S. L.; Hufnagle, W. O.; Kowalik, D. J.; Lagrou, M.; et al. Complete Genome Sequence of Pseudomonas Aeruginosa PAO1, an Opportunistic Pathogen. Nature 2000, 406, 959–964.
