# TUSTOOL
Tumor Segmentation Training Tool

### SUMMARY
Part of the planification process of radiotherapy consists of delimiting the injuries and the surrounding structures defining the treatment area and protecting the nearby tissues.

The role of specialists in this field will always be essential to ensure the effectiveness of the treatment and to avoid potential side effects derived from an inaccurate delimitation. Therefore, **a proper education in tumor delimitation during the residency of every radiation oncologist should be guaranteed**.

This project aims to enhance the training of medical residents in tumor segmentation so that the quality of their contours does not rely only on the number of patients diagnosed when they are working in the radiation oncology department. 

**The application presented in this repository (TUSTOOL) is meant to teach future radiation oncologists to identify and delimit tumors on medical images.**

**Medical residents can** self-register in the application, select cases by filtering them by module, difficulty level, and image modality, and eventually, segment their structures in an interface based on a tool called CERR (Computational Environment for Radiological Research). Once the segmentation is completed, a quantitative comparison with a gold standard is carried on by the system which generates objective feedback for the user.

**Radiation oncologists can** autonomously incorporate new medical cases into the system and can display the results obtained by the medical residents. In this way, TUSTOOL allows the continuous monitoring of students and works as a support mechanism to evaluate their performance in this field.

Note: All the resources provided for this application are in Spanish. However, the user manual comes with very intuitive visual support to make it accessible to everyone.

### TUSTOOL INSTALLATION

Follow the instructions in the document "manual_de_usuario".
The only requirements to download the application are:
  - MATLAB Runtime (https://es.mathworks.com/products/compiler/matlab-runtime.html)
  - A local folder with the gold standard of the cases ("Casos"). It is already located in the installation zip folder.

The provided demos can serve you as guidance on the different functionalities of TUSTOOL.

*Note*: Currently, this application can only be used by faculty or students of the Ramón y Cajal Hospital since only they have access to the REDCap server and own a token needed for its API.

### IMPLEMENTATION CODE

The application TUSTOOL communicates with the database REDCap (Research Electronic Data Capture) for the import/export of oncological cases that have been previously uploaded by a specialist.

<img width="603" alt="image" src="https://github.com/binigoromillo/TUSTOOL/assets/123977045/f0369e2b-2460-4241-bee6-b048c722e675">

The majority of this code has been implemented in Matlab due to the computational need for the statistical metrics used to evaluate the contours.

This implementation uses part of the code provided in CERR, a matlab open source for the development and exchange of results during radiotherapy planification. It enables the format conversion from DICOM to matfile, it provides a module for the visualization of cases in different views, and it provides some tools to contour anatomical structures.

The import and export operations through the REDCap API are implemented by integrating cURL to the Matlab code. This is done with the function *webwrite()* that sends queries throught the RESTful web service. 
These operations have been implemented in the following functions, that create a JSON file based on the input parameters and call the function *importar()* that handles the API query sent to REDCap: 
  - *importar_alumnos()*
  - *importar_especialistas()*
  - *importar_casos()*
  - *importar_resultados()*



