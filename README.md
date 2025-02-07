## BOSCH'S AGE AND GENDER DETECTION MODEL

---
### DESCRIPTION:
We had to come up with a solution that could estimate Age & Gender of people from a surveillance video feed. The pain points included :
* <u>Low-resolution surveillance</u> footages made it difficult to extract features to predict age & gender. And as a result, Super Resolution Models didn't perform well.
* <u>Cameras at varying height</u> posed a challenge as the viewing angles became more complicated.
* <u>Occlusion</u> between objects made it difficult to assign a unique tracking-ID to a person throughout the video.

After experimenting with various models, reading the literature available and trying what worked best for us, we came up with a pipeline which makes use of <b>YOLOX for Object Detection, ByteTrack tracking algorithm for Object Tracking, and finally a custom model that estimates the age & gender</b>.

### RESULTS:
![Results we achieved by running our model on a low-resolution surveillance footage from VIRAT Ground Dataset](./final_output/outputForReadme.gif)

### USAGE: 

#### Setup :

> In case the below Setup fails due to some reason, please go to this colab instance created by us and run the commands there. Finally just upload your evaluation files(images/videos) on the colab instance ([LINK HERE](https://colab.research.google.com/drive/1Q96qgB7mVweHwGtjGt_LscQqG2hDbKk-?usp=sharing)) and correspondingly update the test code. (The same colab notebook is available as `MP_BO_T10_COLAB.ipynb` in the repo as well.)

Before proceeding ensure that you are inside the `MP_BO_T10_CODE/` directory.

Installing the dependencies:
```
# Changing the working directory into the repository
cd <TODO : insert repo name here>
# Installing the dependencies
pip3 install -r requirements.txt
pip3 install 'git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI'
pip3 install --upgrade --no-cache-dir gdown
```
 Downloading the pretrained models directory & storing them as `/pretrained`. Please note, if these commands do not work please download the zip file manually from the drive link provided [here](https://drive.google.com/file/d/18qPkmJIZ4fID5fA309uc8UA0HDMUjF4U/view?usp=sharing) and extract it as a directory `/pretrained`.
```
gdown "18qPkmJIZ4fID5fA309uc8UA0HDMUjF4U"
unzip pretrained
rm -rf pretrained.zip
```

Finally your `/pretrained/` directory should look as follows
 ```
pretrained
    ├── New_32CL_5LR_43Epoc
    └── bytetrack_x_mot17.pth.tar
 ```

Finally run the setup:
```
python3 setup.py develop
```
#### Testing on images/videos:

> The output csv file will be stored under the `/final_output` directory as `predictions.csv`.
> The csv file follows the format : `frame_id, track_id, x, y, w, h, age_actual, gender`

To test on videos :
```
python3 tools/demo_track.py video -c pretrained/bytetrack_x_mot17.pth.tar --path "<$path_to_video>" 
```

To test on a single image :
```
python3 tools/demo_track.py image -c pretrained/bytetrack_x_mot17.pth.tar --path "<$path_to_image>" 
```

To test on a directory of images :
```
python3 tools/demo_track.py image -c pretrained/bytetrack_x_mot17.pth.tar --path "<$path_to_directory_of_images>" 
```
