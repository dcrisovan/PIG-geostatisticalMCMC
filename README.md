# PIG-geostatisticalMCMC

### Overview:
Subglacial topography is often generated using BedMachine or Bedmap products. The bed realizations generated using these products have a high degree of uncertainty and are not reliable in ice sheet modeling. The topography they generate is considered to be too smooth and not realistic. 
Sequential Gaussian simulated (SGS) subglacial topography produces a rougher, more realistic topography, but it is not spatially accurate.

<p align="center">
  <img width="489" height="258" alt="image" src="https://github.com/user-attachments/assets/2efba719-b7e5-4bf5-9338-950d14db9f0a" />
  <img width="491" height="258" alt="image" src="https://github.com/user-attachments/assets/9287cc93-3b61-46ac-8ca9-378b709409ec" />
  <img width="490" height="260" alt="image" src="https://github.com/user-attachments/assets/bdb26fae-eead-4530-a78e-b09a6fbd6426" />
</p>

Thus, Shao et al. developed a method that uses a Markov Chain Monte Carlo (MCMC) approach with mass-conservation constraints to perturb geostatistical simulations and thus stochastically determine the interpolated measurement. 

Four Jupyter notebooks were created that allows a user to perform the aforementioned technique. The first tutorial, `T1_LoadData.ipynb`, allows the user to set the study area, load appropriate datasets, and visualize the ice stream data. The second tutorial, `T2_StatisticalAnalysis.ipynb`, normalizes the provided bed elevation data, calculates the semi-variogram, and fits it with a Gaussian model to generate an initial SGS bed topography. The third tutorial, `T3_LargeScaleChain.ipynb`, initiates the large-scale chain; this notebook also compares the chain's loss to BedMachine, where once the chain's loss approaches BedMachine, the next tutorial should be started. The fourth tutorial, `T4_SmallScaleChain.ipynb`, initaties the small-scale chain, ideally resulting in a loss that is lower than BedMachine's. The latter two tutorials also allow visualization of results to ensure proper execution along the way.

This project focuses on the application of this technique to Pine Island Glacier (PIG) in West Antarctica, flowing into the Amundsen Sea embayment. PIG is a fast-flowing ice stream that is associated with a significant degree of Antarctica's total ice loss.

The polar stereographic coordinates of the vertices of the rectangular study area are:\
`xmin = -1800250`\
`xmax = -1100250`\
`ymax = 30250`\
`ymin = -400250`
<p align="center">
  <img width="457" height="369" alt="image" src="https://github.com/user-attachments/assets/241c9583-087c-4c6a-bc05-0e1aee490ba0" />
  <img width="420" height="250" alt="image" src="https://github.com/user-attachments/assets/7bddf676-9934-44ef-886c-a9e77ec92aa2" />
</p>


### Results:
Following execution of T1-T4, the subglacial topography of PIG was generated using the geostatistical MCMC approach:
<p align="center">
<img width="589" height="360" alt="Screenshot 2025-11-29 at 2 50 05 PM" src="https://github.com/user-attachments/assets/49cf3f0f-3593-4ee4-b6e4-620606739852" />
</p>
This topography is realistically rough while also maintaining spatial accuracy.

### Difficulties and respective alterations made:
- In order to further narrow down PIG's large dataset, the rectangular study area was first masked according to its high velocity areas (`T1_LoadData.ipynb`) and subsequently cropped using the provided `cropStudyArea.ipynb` notebook.
- Given this large dataset, only 5% of the datapoints in the study area were included in the various variogram calculations to decrease the computation time and mitigate memory issues.
- When running the large-scale chain, only the last bed was saved due to memory issues (`T3_LargeScaleChain.ipynb`). This resulted in more difficulties when trying to visualize the changing bed.
- The large-scale chain's loss did not approach BedMachine's loss. This is not ideal and should have been resolved prior to initiating the small-scale chain; however, it is presumed this issue is related to PIG's exceptionally large dataset, and this issue should be revisited with a large-scale chain script that is tailored to handle such datasets.   

<p align="center">
<img width="450" height="333" alt="largeScale_PIG_loss" src="https://github.com/user-attachments/assets/a9643351-7a45-42cf-8ba5-c4da945db0d4" />
</p>

- The small-scale chain's loss also exhibited issues, presumably related to the aforementioned large-scale chain's problems: upon the chain's initiation, the loss began at a much larger value than expected. Moreover, multiple convergence errors and a time constraint did not permit the chain to run for sufficient time. This should be performed again once the large-scale chain is resolved.

<p align="center">
<img width="450" height="333" alt="smallScale_PIG_loss" src="https://github.com/user-attachments/assets/45bdef96-21bb-4314-a598-f2c328fd9c2e" />
</p>

- The block sizes for the large- and small-scale chains were appropriately chosen following initiation of the respective chains and analysis to ensure a proper acceptance rate (the textbook value is 0.234 ()). 

### Environment:
This code was produced and executed in a conda environment created using the provided `gstatsMCMC.yml` file.
This allows the user to create an appropriate coding environment with the needed libraries to run the given tutorial files. 

Upon downloading the file in the appropriate location, the environment can be created in the user's terminal using the following code:

`conda env create -f gstatsMCMC.yml`

The environment can then be activated by entering:

`conda activate gstatsMCMC` into the terminal.

### Usage:
In order to reproduce the results achieved here:

1. Create the environment using the previously suggested method or the user's preferred method.
2. Crop the Antarctic bed elevation data to the Pine Island Glacier study area by running the `T1_LoadData.ipynb` notebook. The result can be further cropped into the refined polygoned area using the `cropStudyArea.ipynb` notebook.
3. Analyze the compiled bed data by running the `T2_StatisticalAnalysis.ipynb` notebook.
4. Initiate the large-scale chain by running the `T3_LargeScaleChain.ipynb` notebook.
5. Once the large-scale chain's loss function approaches the loss of BedMachine, the small-scale can be initiated by running the `T4_SmallScaleChain.ipynb` notebook.
6. Review the "Difficulties and respective alterations made" section to review changes made to the original code.

### Data:
All of the data used can be found in this DropBox link:
https://www.dropbox.com/scl/fo/2mnpqux1jcj3m49tarh9d/AGnKQhjdemYGTg1ajkZR7ek?rlkey=djylgmxv9evjqbyxa0i6393xw&st=7y4694mf&dl=0
(Files were too large to be uploaded independently).\
The citations can be found below.

Nilsson, J., Gardner, A. S. & Paolo, F. (2023). MEaSUREs ITS_LIVE Antarctic Grounded Ice Sheet Elevation Change. (NSIDC-0782, Version 1). [Data Set]. Boulder, Colorado USA. NASA National Snow and Ice Data Center Distributed Active Archive Center. https://doi.org/10.5067/L3LSVDZS15ZV. [describe subset used if applicable]. Date Accessed 11-18-2025.

Rignot, E., Mouginot, J. & Scheuchl, B. (2017). MEaSUREs InSAR-Based Antarctica Ice Velocity Map. (NSIDC-0484, Version 2). [Data Set]. Boulder, Colorado USA. NASA National Snow and Ice Data Center Distributed Active Archive Center. https://doi.org/10.5067/D7GK8F5J8M8R. [describe subset used if applicable]. Date Accessed 11-18-2025.

Morlighem, M. (2022). MEaSUREs BedMachine Antarctica. (NSIDC-0756, Version 3). [Data Set]. Boulder, Colorado USA. NASA National Snow and Ice Data Center Distributed Active Archive Center. https://doi.org/10.5067/FPSU0V1MWUB6. Date Accessed 11-18-2025.

van Wessem, J. M., van de Berg, W. J., Noël, B. P., van Meijgaard, E., Amory, C., Birnbaum, G., Jakobs, C. L., Krüger, K., Lenaerts, J. T., Lhermitte, S., Ligtenberg, S. R., Medley, B., Reijmer, C. H., van Tricht, K., Trusel, L. D., van Ulft, L. H., Wouters, B., Wuite, J., & van den Broeke, M. R. (2018). Modelling the climate and surface mass balance of polar ice sheets using racmo2 – part 2: Antarctica (1979–2016). The Cryosphere, 12(4), 1479–1498. https://doi.org/10.5194/tc-12-1479-2018 

### Software:
__Numpy__\
Harris, C.R., Millman, K.J., van der Walt, S.J. et al. (2020). Array programming with NumPy. Nature 585, 357–362. DOI: 10.1038/s41586-020-2649-2. (Publisher link).

__Matplotlib__\
Hunter, J.D. (2007). Matplotlib: A 2D Graphics Environment. Computing in Science & Engineering, vol. 9, no. 3, pp. 90-95.

__Pandas__\
The pandas development team. (2020). pandas-dev/pandas: Pandas (latest) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.3509134

__gstatsMCMC__\
Shao, N., MacKie, E.J., Field, M.J., McCormack, F.S. A Markov chain Monte Carlo approach for geostatistically simulating mass-conserving subglacial topography. [Manuscript submitted for publication]

__gstatsim__\
MacKie, E.J., Field, M.J., Wang, L., Yin, Z., Schoedl, N., & Hibbs, M. (2022). GStatSim (1.0). Zenodo. https://doi.org/10.5281/zenodo.7230276

__SciKit-Learn__\
Pedregosa, et al. (2011). Scikit-learn: Machine Learning in Python, JMLR 12, pp. 2825-2830.

__SciKit-GStat__\
Mälicke, M. (2021). SciKit-GStat 1.0: A SciPy flavoured geostatistical variogram estimation toolbox written in Python. *Geoscientific Model Development*, 15(6), 2505-2525

### Contributors:
Dara Crisovan\
Niya Shao\
Emma "Mickey" MacKie
<p align="left"> <img width="100" height="100" alt="lab logo" src="https://github.com/user-attachments/assets/93e6f4c0-95d6-417d-9eed-29e1470602a4" /> </p>
