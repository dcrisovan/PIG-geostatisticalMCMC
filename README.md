# PIG-geostatisticalMCMC
### Overview:
Subglacial topography is often generated using BedMachine or Bedmap products. The bed realizations generated using these products have a high degree of uncertainty and are not reliable in ice sheet modeling. The topography they generate is considered to be too smooth and not realistic. 
Sequential Gaussian simulated (SGS) subglacial topography produces a rougher, more realistic topography, but it is not accurate.

<img width="489" height="258" alt="image" src="https://github.com/user-attachments/assets/2efba719-b7e5-4bf5-9338-950d14db9f0a" />
<img width="491" height="258" alt="image" src="https://github.com/user-attachments/assets/9287cc93-3b61-46ac-8ca9-378b709409ec" />
<img width="490" height="260" alt="image" src="https://github.com/user-attachments/assets/bdb26fae-eead-4530-a78e-b09a6fbd6426" />

In 2026, Shao et al. (🥳) developed a method that uses a Markov Chain Monte Carlo (MCMC) approach with mass-conservation constraints to perturb geostatistical simulations and thus stochastically determine the interpolated measurement.

This project focuses on the application of this technique to Pine Island Glacier (PIG) in West Antarctica, flowing into the Amundsen Sea embayment. PIG is a fast-flowing ice stream that is associated with a significant degree of Antarctica's total ice loss.

<img width="457" height="369" alt="image" src="https://github.com/user-attachments/assets/241c9583-087c-4c6a-bc05-0e1aee490ba0" />
<img width="440" height="250" alt="image" src="https://github.com/user-attachments/assets/7bddf676-9934-44ef-886c-a9e77ec92aa2" />

### Environment:
This code was produced in a conda environment created using the provided `gstatsMCMC.yml` file.
Upon downloading the file in the appropriate location, the environment can be created in the user's terminal using the following code:

`conda env create -f gstatsMCMC.yml`

The environment can then be activated by entering:

`conda activate gstatsMCMC`

### Usage:
In order to reproduce the results achieved here:
1. Create the environment using the previously suggested method or the user's preferred method.
2. Crop the Antarctic bed elevation data to the Pine Island Glacier study area by running the `T1_LoadData.ipynb` notebook. The result can be further cropped into the refined polygoned area using the `cropStudyArea.ipynb` notebook.
3. Analyze the compiled bed data by running the `T2_StatisticalAnalysis.ipynb` notebook. This normalizes provided bed elevation data, calculates the semi-variogram, and fits it with a Gaussian model to generate an initial SGS bed topography.
4. Initiate the large-scale chain by running the `T3_LargeScaleChain.ipynb` notebook.
5. Once the large-scale chain's loss function approaches the loss of BedMachine, the small-scale can be initiated by running the `T4_SmallScaleChain.ipynb` notebook.

__Note:__ Given the large amount of radar measurements available for PIG, only 5% of the datapoints in the study area were include to decrease the computation time.

### Data:
Nilsson, J., Gardner, A. S. & Paolo, F. (2023). MEaSUREs ITS_LIVE Antarctic Grounded Ice Sheet Elevation Change. (NSIDC-0782, Version 1). [Data Set]. Boulder, Colorado USA. NASA National Snow and Ice Data Center Distributed Active Archive Center. https://doi.org/10.5067/L3LSVDZS15ZV. [describe subset used if applicable]. Date Accessed 11-18-2025.

Rignot, E., Mouginot, J. & Scheuchl, B. (2017). MEaSUREs InSAR-Based Antarctica Ice Velocity Map. (NSIDC-0484, Version 2). [Data Set]. Boulder, Colorado USA. NASA National Snow and Ice Data Center Distributed Active Archive Center. https://doi.org/10.5067/D7GK8F5J8M8R. [describe subset used if applicable]. Date Accessed 11-18-2025.

Morlighem, M. (2022). MEaSUREs BedMachine Antarctica. (NSIDC-0756, Version 3). [Data Set]. Boulder, Colorado USA. NASA National Snow and Ice Data Center Distributed Active Archive Center. https://doi.org/10.5067/FPSU0V1MWUB6. Date Accessed 11-18-2025.

https://tc.copernicus.org/articles/12/1479/2018/ (I don't know how to cite this yet)

### Software:
All libraries used? NumPy, GStatSim, etc.?
