# TUSTOOL
Tumor Segmentation Training Tool

### SUMMARY
Part of the planification process of radiotherapy consists of delimiting the injuries and the surrounding structures defining the treatment area and protecting the nearby tissues.

The role of specialists in this field will always be essential to ensure the effectiveness of the treatment and to avoid potential side effects derived from an inaccurate delimitation. Therefore, **a proper education in tumor delimitation during the residency of every radiation oncologist should be guaranteed**.

This project aims to enhance the training of medical residents in tumor segmentation so that the quality of their contours does not rely only on the number of patients diagnosed when they are working in the radiation oncology department. 

**The application presented in this repository (TUSTOOL) is meant to teach future radiation oncologists to identify and delimit tumors on medical images.**

**Medical residents can** self-register in the application, select cases by filtering them by module, difficulty level, and image modality, and eventually, segment their structures in an interface based on a tool called CERR (Computational Environment for Radiological Research). Once the segmentation is completed, a quantitative comparison with a gold standard is carried on by the system which generates objective feedback for the user.

**Radiation oncologists can** autonomously incorporate new medical cases into the system and can display the results obtained by the medical residents. In this way, TUSTOOL allows the continuous monitoring of students and works as a support mechanism to evaluate their performance in this field.

*Note*: Most titles and instructions provided in this application are in Spanish. However, the code includes comments in english and the user manual comes with very intuitive visual support to make it accessible to everyone.

### TUSTOOL INSTALLATION

*Note*: Currently, this application can only be used by faculty or students of the Ramón y Cajal Hospital since only they have access to the REDCap server and own a token needed for its API.

Follow the instructions in the document "manual_de_usuario".
The only requirements to download the application are:
  - MATLAB Runtime (https://es.mathworks.com/products/compiler/matlab-runtime.html)
  - A local folder with the gold standard of the cases ("Casos"). It is already located in the installation zip folder.

The provided demos can serve you as guidance on the different functionalities of TUSTOOL.

### CODE IMPLEMENTATION 

The application TUSTOOL communicates with the database REDCap (Research Electronic Data Capture) for the import/export of oncological cases that have been previously uploaded by a specialist.

<img width="603" alt="image" src="https://github.com/binigoromillo/TUSTOOL/assets/123977045/f0369e2b-2460-4241-bee6-b048c722e675">

The majority of this code has been implemented in Matlab due to the computational need for the statistical metrics used to evaluate the contours.

This implementation uses part of the code provided in CERR, a matlab open source for the development and exchange of results during radiotherapy planification. It enables the format conversion from DICOM to matfile, it provides a module for the visualization of cases in different views, and it provides some tools to contour anatomical structures.

The import and export operations through the REDCap API are implemented by integrating cURL into the Matlab code. This is done with the function *webwrite()* that sends queries through the RESTful web service. 

The import operations have been implemented in the following functions, which create a JSON file based on the input parameters and call the function *importar()* that handles the API query sent to REDCap: 
  - *importar_alumnos()*
  - *importar_especialistas()*
  - *importar_casos()*
  - *importar_resultados()*

In order to retrieve data from REDCap, we use the function *exportar()*.

#### Segmentation interface

For the segmentation interface, the following functions were created similarly to CERR but with some functionality and design modifications:
  - *sliceCallBack()*: Main function that creates and updates the images visualization interface. This function creates and handles two variables:
    - **planC**: Stores the information corresponding to the case that will be segmented (including the gold standard).
    - **stateC**: Stores the information corresponding to the state of the operations carried out during the segmentation (segmentation tools, selected view...).

The calls to *sliceCallBack()* that will be used in this applications are:
- *sliceCallBack('INIT')*: Initializes *planC*, *stateS* and *ref* (cell variable containing the gold standard) and adds the graphic components to the interface.

<img width="452" alt="image" src="https://github.com/binigoromillo/TUSTOOL/assets/123977045/5fb2d512-c4b6-43bc-b321-764019cff83b">

- *sliceCallBack('OPENNEWPLANC')*: Loads the images to the interface as well as all the information relative to the student and the case that is being segmented. 

<img width="454" alt="image" src="https://github.com/binigoromillo/TUSTOOL/assets/123977045/1c2a1802-e15a-499a-8017-3640b97803c5">

- *sliceCallBack('CONTOURMODE')*: Uses the functions *controlFrame()* and *contourControl()* to include the contouring tools into the interface so that the student can start segmenting.

<img width="408" alt="image" src="https://github.com/binigoromillo/TUSTOOL/assets/123977045/9a9ad9a6-3b98-4a48-8cb8-3e291170ca69">

Once the student has finished the segmentation, the "Finish" bottom will save the results and compute the similarity metrics with the gold standard stored in *ref*. Finally, the results of the metrics as well as the final calification are imported as a new register in the "Results" table in REDCap.

The function *sliceCallBack('RESULTSMODE') modifies the interface to display the results for each segmented structure. This interface is implemented with three main functions *contourFrame('RESULTS','INIT')*, *contourFrame('RESULTS','REFRESH')* and *contourFrame('RESULTS','SELECTSTRUCT')*

Example of the results bar:
<img width="200" alt="image" src="https://github.com/binigoromillo/TUSTOOL/assets/123977045/b424baca-aeee-407d-9a2f-ee8f301dd631">

The student will be able to see his segmentation and the gold standard simultaneously and in different views:
<img width="405" alt="image" src="https://github.com/binigoromillo/TUSTOOL/assets/123977045/f7ad0545-54bc-4470-aec4-c4caf2b1df69">
<img width="470" alt="image" src="https://github.com/binigoromillo/TUSTOOL/assets/123977045/18285dfb-f646-413f-afdb-705b283417b4">





