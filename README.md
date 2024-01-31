
![236798_Happy birds eating seeds inside a feeder_xl-1024-v1-0 (1)](https://github.com/Lucas-Zampar/detector_de_passaros_amazonicos/assets/75434421/5d79e9f7-30e7-440b-855e-41657af449be)

# Amazonian Birds Detector

Contact: 
lucas.fzampar@gmail.com 
clay.palmeira@gmail.com

Birds attract human attention due to their beauty and diversity, which encourages people to observe them for leisure. In this process, it is possible to record birds in images and share them on citizen science platforms such as [WikiAves](https://www.wikiaves.com.br/) and [eBird](https://ebird.org/home ). With this, people can contribute significantly to scientific research that aims to understand and preserve bird species.

The Amazon region can offer an excellent bird watching experience given the diversity of existing species. Some of them have even adapted to urban environments, making it possible to find birds feeding even in residential gardens. This way, local residents can observe birds by attracting them through food placed in open feeders.

In this context, the opportunity to use webcams to record Amazonian birds feeding at residential feeders was noted. Furthermore, the question was whether it would be possible to use artificial intelligence, through Deep Learning models, to automatically detect the species of the recorded birds.

Through this project, it was possible to answer this question by developing a Deep Learning model capable of detecting 5 species of Amazonian birds that frequent residential feeders. The application of the model is demonstrated in the video below.

__NOTE__: The name of the species is in Portuguese. The corresponding translation can be seen below:

- canário-do-amazonas (orange-fronted yellow-finch)
- chupim (shiny cowbird)
- rolinha (ground dove)
- sanhaço-da-amazônia (blue-gray tanager)
- sanhaço-do-coqueiro (palm tanager)

https://github.com/Lucas-Zampar/detector_de_passaros_amazonicos/assets/75434421/7eede525-d633-4972-8065-a0ed4a6c1abe



# Objectives

The general objective of this project was to develop a Deep Learning based approach to automatically detect Amazonian bird species from a residential context. To this end, the following specific objectives were defined:

- Create a set of images of birds that frequent a residential feeder;
- Annotate the images taken for the object detection task;
- Train and evaluate Deep Learning models in different configurations to detect the species of these birds;
  
# Proposal 

## Dataset

The study had access to the feeder of a residence in the state of Amapá, Brazil, which can be seen in the figure below.

<img src="https://github.com/Lucas-Zampar/detector_de_passaros_amazonicos/assets/75434421/a0c60cdd-c422-49c0-9599-b37959cf570e" alt="comedouro" height=60% width=60%>

Three Logitech C270 HD webcams were installed at the feeder, connected to a laptop in order to record the birds feeding as shown in the figure below. The recordings were captured using a [script with the help of the OpenCV library](https://github.com/Lucas-Zampar/detector_de_passaros_amazonicos/blob/main/dataset/dataset_utils/script_opencv.py).

<img src="https://github.com/Lucas-Zampar/amazonian_birds_detector/assets/75434421/027f2f59-b4f8-4ca3-bad7-ee706f2f1519" alt="esquema_gravacao" height=60% width=60%>

The images in the dataset were obtained from frames extracted from these recordings. In order to facilitate extraction, a [Streamlit application](https://github.com/Lucas-Zampar/detector_de_passaros_amazonicos/tree/main/streamlit_app) was developed, demonstrated in the video below. Through it, it is possible to select random or specific frames from the recordings, in addition to filtering them by date and predominant species.

https://github.com/Lucas-Zampar/detector_de_passaros_amazonicos/assets/75434421/f92f70fb-00c9-42a9-be28-88ab50c10324

Bird species were determined through consultations carried out on the citizen science platforms [WikiAves](https://www.wikiaves.com.br/) and [eBird](https://ebird.org/home). Thus, it was possible to identify five species popularly known in Portuguese as __canário-do-amazonas__ (orange-fronted yellow-finch), __chupim__ (shiny cowbird), __rolinha__ (ground dove), __sanhaço-da-amazônia__ (blue-gray tanager) and __sanhaço-do-coqueiro__ (palm tanager).

The acquired images were annotated using the [RoboFlow](https://roboflow.com/) platform. In total, 940 images and 1,836 annotations were collected in Pascal VOC format. The distribution of annotations by species can be seen in the graph below. The produced dataset can be found in the [dataset folder](https://github.com/Lucas-Zampar/detector_de_passaros_amazonicos/tree/main/dataset).

<img src="https://github.com/Lucas-Zampar/detector_de_passaros_amazonicos/assets/75434421/278633e3-965a-4707-a144-de9da2c4d431" alt="esquema_gravacao" height=60% width=60%>

## Training 

Among the Deep Learning algorithms to detect objects, it is possible to find [Faster R-CNN](https://arxiv.org/abs/1506.01497) which falls into the category of two-stage detectors. In this category, the model first proposes regions with possible objects called RoI (Region of Interest). It then uses these regions to perform detections. In general, two-stage detectors tend to be more accurate. For this reason, it was decided to use Faster R-CNN.

In this work, there were two consecutive training phases called respectively:

- __preliminary phase__: In the preliminary phase, 30% of the data was preserved in a set called partial, corresponding to 282 images and 560 annotations. This decision was made to define a baseline with less training data. The need to define a baseline arises due to the lack of previous studies. Therefore, it is necessary to define a model that serves as a basis for analysis to be compared and improved later, which will be done in the next phase.

- __final phaes__: In the final phase, a single definitive model was trained using the same training configuration as the baseline. However, all data corresponding to 940 images and 1,836 annotations was used at this phase. 70% of the data was intended for training, while the remaining 30% for validation. After training, the performance of the definitive model was compared with that of the baseline. 

The training was conducted using the framework [IceVision](https://github.com/airctic/icevision) which uses the models made available by [MMDetection](https://github.com/open-mmlab/mmdetection). The configuration selected to train both the baseline and the definitive model can be seen in the table below.

|  Hyperparameters                          | Values                   |
|-------------------------------------------|--------------------------|
| Backbone                                  | ResNeXt101 32x4d FPN 1x  |
| Epochs                                    | 20                       |
| Training dataset shuffling                | No                       |
| Learning rate                             | 10-4                     |
| Batch size                                | 1                        |
| Image size                                | 896x896                  |
| Image size during presizing               | 1024x1024                |


The evaluation was conducted by the framework [FiftyOne](https://github.com/voxel51/fiftyone) using mean Average Precision (mAP) metric defined by the evaluation criteria of the [COCO](https://cocodataset.org/#detection-eval) dataset. Considering the IoU threshold at 50%, the baseline and the definitive model achieved the following results:


|       Model       |    mAP  | 
|:-----------------:|:-------:|
| Baseline          | 0.9459  |
| Definitive model  | 0.9833  | 

There was a percentage growth of 3.95\% in the mAP ratio, demonstrating that training with more data benefited the definitive model.


In addition to mAP, precision and recall metrics were used to verify the performance of the models in relation to each species, considering the IoU threshold at 50%. The results can be viewed in the table below:


| Species             	| Baseline precision 	| Definitive model precision  	| Baseline recall   	| Definitive model recall   	|
|----------------------	|-------------------	|-----------------------------	|---------------------	|------------------------------	|
| canário-do-amazonas  	|  0.9344           	| 0.9515                      	| 0.9828              	| 0.9849                       	|
| chupim               	|  0.9000           	| 0.9725                      	| 0.9231              	| 1.0000                       	|
| rolinha              	|  0.9643           	| 0.9663                      	| 1.0000              	| 0.9773                       	|
| sanhaço-da-amazônia  	|  0.8519           	| 0.9877                      	| 1.0000              	| 1.0000                       	|
| sanhaço-do-coqueiro  	|  0.8462           	| 0.9200                      	| 0.8800              	| 0.9787                       	|
| __Mean__                	|  __0.8993__           	| __0.9596__                      	| __0.9572__              	| __0.9882__                       	|


In general, the definitive model presented an mean percentage gain in precision of 6.7% compared to the baseline, going from 89.93% to 95.96%. The mean recall had a smaller percentage growth of 3.24%, going from 95.72% to 98.82%.


# Conclusion 

Through this work, it was possible to create a set containing 940 images of Amazonian birds feeding at a residential feeder and 1,836 annotations in Pascal VOC format distributed among the 5 species of these birds(canário-do-amazonas, chupim, rolinha, sanhaço-da-amazônia e sanhaço-do-coqueiro).The dataset collected is [publicly available](https://github.com/Lucas-Zampar/detector_de_passaros_amazonicos/tree/main/dataset/total_dataset).

Furthermore, a Faster R-CNN model trained with this data was produced in order to automatically detect bird species. The model achieved mAP of 98.33% when considering the IoU threshold at 50%. The mean precision and mean recall of the model were 95.96% and 98.82% respectively.
 

# Next Steps

In view of this, there is an opportunity for future work:

- As a short-term proposal, more images will be extracted and annotated from existing recordings. The produced model will partially annotate the images in order to reduce human labor.
- As a medium-term proposal, a new set of images extracted from webscrapping will be created. As a requirement, images must contain birds of a single species highlighted. Here, it is hypothesized that pre-training the models with these images will help them learn richer species features than if they were trained directly and only with images obtained from webcams.
- As a long-term proposal, the prototype of a system capable of collecting new images in other homes will be developed. To this end, we imagine making a feeder in smaller proportions, in addition to using the Raspberry PI 4 board connected to a camera to record the birds. Furthermore, we imagine studying the feasibility of performing inferences locally in an edge computing approach.

# Project Structure

The project root contains the following folders:

- __project_codes__: folder where all notebooks and other project implementations are located.
- __dataset__: folder where the datasets are located.
- __faster_rcnn__: folder where the models' checkpoints are located. Unfortunately, due to storage limitations, the full content is only available [in the full repository shared by Google Drive](https://drive.google.com/drive/folders/12ueqV4UuxU2ebdD4YYV4xpQZ3hxHhIk-?usp=sharing).
- __streamlit_app__: folder where the implementation of the frame extraction application developed in Streamlit is located.





