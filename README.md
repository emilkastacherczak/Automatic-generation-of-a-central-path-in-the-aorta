# Aorta-Central-Path-Extraction
This repository contains a Python-based algorithm implemented in a Jupyter notebook to extract the central path of the aorta from a 3D volume dataset. The central path is essential in medical image analysis, particularly in vascular modeling, surgical planning, and hemodynamic simulations.

# Data base
The database used to develop the aortic central path generation algorithm was downloaded from the public “Vascular Model Repository” (https://vascularmodel.com). From a total set of 275 models were selected 35 images of a healthy aorta, taken using magnetic resonance imaging and computed tomography. 2 models come from rabbits, the rest of the aortas are of human origin, divided into 6 female and 16 male (the rest unspecified). All images also contain a perfect central path, which serves as a reference to evaluate the effectiveness and accuracy of the algorithm.

# Requirements
Essential packages and libraries:
- for handling medical data (nrrd, pyvista, SimpleITK),
- for numerical analysis and simplified visualization (numpy, pandas, seaborn, matplotlib),
- for parsing XML files (xml.etree.ElementTree).

# Method overview
1. Loading data

   The data is retrieved from folders located on Google Drive. These folders contain segmented aortas (.nrrd files) and their corresponding central paths, which serve as ground truth (.pth files). File paths are matched using the nrrd_path_pairs function, which returns a dictionary with keys corresponding to the numerical identifiers of individual cases.

    Each aorta is then loaded as a 3D binary mask along with headers from the file that include important metadata, such as the origin of the coordinate system (space origin) and the size of a single voxel (space directions - spacing).

2. Data tranformation

   Metadata is used in later stages to convert physical distance to voxel units according to formula:

      voxelindex = (physical index - origin) / spacing
   
   Data processing includes padding to align the dimension and removing branches so that the algorithm focuses only on the main member of the vessel.

3. Skeletonization

    The skeletonization of the aorta is based on a distance map and the detection of local maxima, which are recorded as path points and then interpolated.
