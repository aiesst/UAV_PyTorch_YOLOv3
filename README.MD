# YOLO3 (Detection, Training, and Evaluation)

## Dataset and Model

Dataset | mAP |  Config | Model
:---:|:---:|:---:|:---:
[UAV Detection (1 class)](https://drive.google.com/drive/folders/136XcXknce4PebgN-dr3be26JtP0HkZy5) | 1.0 | check zoo |  [uav_wh.h5](https://drive.google.com/open?id=1KdtDmibwTvUViSGllOI7XEhO1FvG8h7d) 

## Todo list:
- [x] Build UAV Datasets
- [x] Make YOLO3 better
- [x] Yolo3 detection
- [x] Yolo3 training
- [x] mAP Evaluation
- [x] Multi-GPU training
- [x] Prediction


## Training

### 1. Data preparation 

Download the UAV dataset from  (https://drive.google.com/drive/folders/136XcXknce4PebgN-dr3be26JtP0HkZy5)

Organize the dataset into 3 folders:

+ train,the folder contains train images and train annotations,the format of annotations is mainly VOC format and YOLO format.

+ val,the folder is same as train folder.

+ test,the folder is all images.

NOTICE: If the validation set is empty, the training set will be automatically splitted into the training set and validation set using the ratio of 0.9.

### 2. Edit the configuration file
The configuration file is a json file, which looks like config.json:

Download pretrained weights for backend at:

https://drive.google.com/open?id=1Iuu_fMKqW9cq2ymPw564-7NAN8FkPGny

NOTICE: This weights must be put in the root folder of the repository. They are the pretrained weights for the backend only and will be loaded during model creation. The code does not work without this weights.**

### 3. Generate anchors for your dataset (optional)

`python gen_anchors.py -c config.json`

Copy the generated anchors printed on the terminal to the ```anchors``` setting in ```config.json```.

### 4. Start the training process

`python train.py -c config.json`

By the end of this process, the code will write the weights of the best model to file uav.h5 (or whatever name specified in the setting "saved_weights_name" in the config.json file). The training process stops when the loss on the validation set is not improved in 3 consecutive epoches.

### 5. Perform detection using trained weights on image, set of images, video, or webcam
`python predict.py -c config.json -i /path/to/image/or/video`

It carries out detection on the image and write the image with detected bounding boxes to other folder.

## Evaluation

`python evaluate.py -c config.json`

Compute the mAP performance of the model defined in `saved_weights_name` on the validation dataset defined in `valid_image_folder` and `valid_annot_folder`.
