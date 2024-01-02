# Content

You will find in this folder:

## Files 

- __detect_image.ipynb__: notebook used to annotate an image.
- __detect_vidoe.ipynb__: notebook used to annotate a video.
- __make_report_csv_files.ipynb__: notebook used to generate tables with the results of each model.
- __train_model.ipynb__: notebook used to train and evaluate the models.
- __img_demo_X.png__: demonstrative image for annotation.
- __video_demo.mp4__: demonstrative video for annotation.

__WARNING__: notebooks need the _checkpoints_ of the models in the `../faster_rccnn/` folder. Please download from [the full repository shared by Google Drive.](https://drive.google.com/drive/folders/12ueqV4UuxU2ebdD4YYV4xpQZ3hxHhIk-?usp=drive_link) &#9888;


## Folders

- __bar_graphs__: contains CSV files with the proportion of annotations per species found in each dataset, as well as a notebook that generates bar graphs from them.
- __project_utils__: folder containing files with wrappers implemented in Python for IceVision, FifityOne and OpenCV functions frequently used during the development of the work.

# Instalation

In order to run notebooks on a local machine, it is necessary to install the IceVision and FifityOne framework. It is worth noting that IceVision only supports Linux and MacOS.

Initially, we created a virtual environment using the mamba manager:

```
mamba create -n ICE python=3.9 -y
```

Then, we activate the newly created environment: 

```     
mamba activate ICE
```

In order to access the object detection models, it is necessary to install the mmcv-full and mmdet packages respectively:

```
pip install mmcv-full==1.3.17 -f https://download.openmmlab.com/mmcv/dist/cu102/torch1.10.0/index.html
pip install mmdet==2.17.0
```

With this, it is now possible to install version 0.12 of the IceVision framework used in this work:

```
pip install icevision[all]==0.12.0
```

It is important to highlight that version 0.12 of IceVision is incompatible with versions of the `setuptools` package higher than 59.5.0. So, you need to reduce the version of this package:

```
pip uninstall setuptools -y
pip install setuptools==59.5.0 
```

The same is valid for the `sahi` package which must be lower than version 0.10:

```
pip uninstall sahi -y
pip install sahi==0.8.19
```

As well as the `numpy` package which must be lower than version 1.25:

```
pip uninstall numpy -y
pip install numpy==1.23
```

With this, the FiftyOne framework can now be installed:

```
pip install fiftyone
```

Finally, it is necessary to install Jupyter Notebook to access the content present in the notebooks:

```
pip install jupyterV
```


__WARNING__: the IceVision framework is experimental. Therefore, new versioning problems not identified during the work may arise in the future. &#9888;










