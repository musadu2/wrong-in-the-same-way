# Coverage is not enough: finite-sample bias, not model variance, limits small-data machine-learned potentials

This GitHub repo includes the data analysis and machine-learned interatomic potentials (MLIPs) files and folders. 

## Process 
To reproduce our results, the two folders includes all the necessary files and data. Two steps to track is first preparing the training/validation/testing configurations and training models.

## Data Analysis: 
There are couple phases to prepare training/validation/testing configurations:
  - phase 1: Begin with downloading dftli3ps4.xyz dataset by accessing dft_li3ps4.rtf file which has a Google Drive link to data.
  - phase 2: Separate your merged data into individual configurations by using Separate_Configurations.ipynb.
  - phase 3: Convert the configurations into Smooth Overlap of Atomic Positions descriptor by using SOAP_Descriptor_Generator.ipynb. DFT_Li3PS4_outer_average_SOAP_16000.npy file should be saved. We provided it in Data_Analysis folder.
  - phase 5: Select your data with Farthest Point Sampling by using Data_Selection_Farthest_Point_Sampling.ipynb. After running Data_Selection_Farthest_Point_Sampling.ipynb, there will be bot individual training/validation/testing and merged configuration files. 

## MLIPs:
After the data is ready, second step is to train and test MLIPs: 
  - MACE: We use individual training/validation/testing files for developing MACE models. You can find our MACE hyper parameters inside mace_submit file for each number of training data cases. To validate the reproduced results, we also include training .log files.
  - Allegro: We use merged training/validation/testing files for developing Allegro models. You can find our Allegro hyper parameters inside training.yaml file for each number of training data cases. To validate the reproduced results, we also include submission and training .out files.
  
  
The phases are tied to issues, which you should track using ZenHub. 

For each phase, you will complete the necessary tasks and carry out the required reviews with your review team. 
You should work with your review team until everyone believes that all the required elements for the phase are in place.  
At this point, you can submit the materials to me for review via your repository. 

