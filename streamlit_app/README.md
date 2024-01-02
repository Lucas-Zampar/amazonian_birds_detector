# Frame Extraction

# Content

Here, you will find:

## Files

- __app_utils.py__: auxiliary code to access and save frames.
- __app.py__: implementation of the graphical interface.

## Folders

-__cortes_por_especie__: folder where the cuts must be present. Due to space limitations, cuts are only available [in the full repository shared by Google Drive](https://drive.google.com/drive/folders/12ueqV4UuxU2ebdD4YYV4xpQZ3hxHhIk-?usp=drive_link).
- __local_dataset__: folder where the selected frames are saved.


# Installations

Initially, we created a virtual environment using the mamba manager:

```
mamba create -n APP python=3.9 -y
```

Then, we activate the newly created environment:

```
mamba activate APP
```

Then we install Streamlit:

```
pip install streamlit
```

Additionally, you must also install the Numpy and OpenCV packages:

```
pip install numpy
pip install opencv-python
```

This way, just start the application:

```
streamlit run app.py
```
