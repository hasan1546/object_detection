
# object_detection
## Requirements
#### 1. Visual Studio 2019 with C++ Build Tools is required for building tensorflow v2.4.0 
https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&rel=16
#### 2.  Visual C++ 2015 build is required for installing pycocotools 
https://go.microsoft.com/fwlink/?LinkId=691126

### List of CUDA enabled devices https://developer.nvidia.com/cuda-gpus
#### If you have CUDA enabled device download and install drivers and Cuda toolkit.
#### 3. Install NVIDIA DRIVER https://www.nvidia.com/Download/index.aspx
#### 4. Install Install CUDA TOOLKIT v11.1 https://developer.download.nvidia.com/compute/cuda/11.1.1/local_installers/cuda_11.1.1_456.81_win10.exe
#### 5. Download cuDNN https://developer.nvidia.com/compute/machine-learning/cudnn/secure/8.0.5/11.1_20201106/cudnn-11.1-windows-x64-v8.0.5.39.zip
After downloading extract the file. Copy ***bin , include, lib*** to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.1
#### Add CUDA to your path.
 Press **window+R** and run **control sysdm.cpl**. Go to **Advanced >Environment Variable**. Click **New...**  
```
Variable name: CUDA_PATH
Variable value:C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.1
```
6. Download Anaconda https://www.anaconda.com/products/individual

## STEP 1
## Create and activate virtual environment.
Open **Command Promt**
```
conda create -n tensorflow pip python=3.8

conda activate tensorflow
```
## Install tensorflow-gpu
```
pip install tensorflow-gpu
```
#### If you do not have GPU
```
pip install tensorflow
```
##### Test your installation 
```
python
  >>> import tensorflow as tf
  >>> print(tf.__version__)
  >>> exit()
```
```
mkdir TensorFlow
cd C:\TensorFlow
```
## Download Model
```
conda install -c anaconda git
git clone https://github.com/tensorflow/models.git
cd models\research
protoc object_detection\protos\*.proto --python_out=.
```
Close **Command Prompt**.
## Open Anaconda prompt
**Install pre-requisites:**
```
conda activate tensorflow
python -m pip install --upgrade pip
pip install cython
conda install -c anaconda protobuf
pip install pillow
pip install lxml
pip install Cython
pip install contextlib2
pip install jupyter
pip install matplotlib
pip install pandas
pip install opencv-python
```
**Install pycocotools using following command**(Visual C++ 2015 build is required)
```
pip install git+https://github.com/philferriere/cocoapi.git#subdirectory=PythonAPI 
```
```
cd C:\TensorFlow\models\research
```
```
copy object_detection\packages\tf2\setup.py .
python -m pip install .
```
## Check if everything is working perfectly.
```
python object_detection\builders\model_builder_tf2_test.py
```
### Open another anaconda prompt
```
cd C:\TensorFlow
git clone https://codeload.github.com/tzutalin/labelImg.git
```
```
conda install pyqt=5
conda install -c anaconda lxml
pyrcc5 -o libs/resources.py resources.qrc
python labelImg.py
```

## Export pre-trained test.record and train.record
```
C:\TensorFlow
git clone https://github.com/tanvir1546/object-detection.git
```
```
cd C:\TensorFlow\scripts\preprocessing
python generate_tfrecord.py -x C:\Tensorflow\workspace\training_demo\images\train -l C:\Tensorflow\workspace\training_demo\annotations\label_map.pbtxt -o C:\Tensorflow\workspace\training_demo\annotations\train.record
python generate_tfrecord.py -x C:\Tensorflow\workspace\training_demo\images\test -l C:\Tensorflow\workspace\training_demo\annotations\label_map.pbtxt -o C:\Tensorflow\workspace\training_demo\annotations\test.record
```
## place the downloaded model in pre-trained model
**Download ssd mobilenet models from here**.
http://download.tensorflow.org/models/object_detection/tf2/20200711/ssd_mobilenet_v2_fpnlite_320x320_coco17_tpu-8.tar.gz
Download and extract the file. Copy the file to **C:\TensorFlow\workspace\training_demo\pre-trained-models**
~~https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md~~
copy only **pipeline.config** to models/my_ssd_mobilenet_v2_fpnlite and edit **pipeline.config** 
```
Line 3. num_classes: 2(type of object u r detecting. )
Line 135. batch_size: 5
Line 165. fine_tune_checkpoint: "pre-trained-models/ssd_mobilenet_v2_fpnlite_320x320_coco17_tpu-8/checkpoint/ckpt-0"
Line 171. fine_tune_checkpoint_type: "detection"
Line 175. label_map_path: "annotations/label_map.pbtxt"
Line 177. input_path: "annotations/train.record"
Line 185. label_map_path: "annotations/label_map.pbtxt"
Line 189. input_path: "annotations/test.record"
```
### Start training and forget about this . This might take upto 5 hours or more. 
```
cd C:\TensorFlow\workspace\training_demo
python model_main_tf2.py --model_dir=models\my_ssd_mobilenet_v2_fpnlite --pipeline_config_path=models\my_ssd_mobilenet_v2_fpnlite\pipeline.config
```
## Monitoring training with tensorboard- open another anaconda promt
```
conda activate tensorflow
cd C:\TensorFlow\workspace\training_demo
tensorboard --logdir=models\my_ssd_mobilenet_v2_fpnlite
```
## Exporting the Inference Graph
```
cd C:\TensorFlow\workspace\training_demo

python .\exporter_main_v2.py --input_type image_tensor --pipeline_config_path .\models\my_ssd_mobilenet_v2_fpnlite\pipeline.config --trained_checkpoint_dir .\models\my_ssd_mobilenet_v2_fpnlite\ --output_directory .\exported-models\my_mobilenet_model
```


## Evaluating the Model
```
cd C:\TensorFlow\workspace\training_demo
python model_main_tf2.py --pipeline_config_path models\my_ssd_mobilenet_v2_fpnlite\pipeline.config --model_dir models\my_ssd_mobilenet_v2_fpnlite --checkpoint_dir models\my_ssd_mobilenet_v2_fpnlite --alsologtostderr
```

## Using the model
```
cd C:\TensorFlow\workspace\training_demo
python TF-image-od.py
```


### Exporting the Model as tf_lite

```
conda activate tensorflow
```
```
cd C:\TensorFlow\workspace\training_demo
```
```
python export_tflite_graph_tf2.py --pipeline_config_path models\my_ssd_mobilenet_v2_fpnlite\pipeline.config --trained_checkpoint_dir models\my_ssd_mobilenet_v2_fpnlite --output_directory exported-models\my_tflite_model
```
### Creating a New Environment and Installing TensorFlow
```
conda deactivate
```
```
conda create -n tflite pip python=3.8
```
```
conda activate tflite
```
```
pip install tensorflow
```
```
python
```
```
Python 3.7.9 (default, Aug 31 2020, 17:10:11) [MSC v.1916 64 bit (AMD64)] :: Anaconda, Inc. on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
>>> print(tf.__version)
```

```
2.4.0
```

### Converting the Model to TensorFlow Lite
look at the [TensorFlow Lite converter](https://www.tensorflow.org/lite/convert/). The usage of this program is as so
```
python convert-to-tflite.py
```


### labels.txt
Preparing our Model for Use ```exported-models\my_tflite_model\saved_model``` as ```labels.txt```. 
Trick is your delete all the extra things in lebel_map.pbtxt jst write down the class name.

# Part 4 - How to Run TensorFlow Lite Object Detection Models on the Raspberry Pi 



### Step 1. Download this repository and create virtual environment

Next, clone this GitHub repository by issuing the following command. The repository contains the scripts we'll use to run TensorFlow Lite, as well as a shell script that will make installing everything easier. Issue:

```
git clone https://github.com/tanvir1546/object_detection/raspi.git
```

```
mv raspi tflite
cd tflite
```



```
sudo pip3 install virtualenv
```

Then, create the "tflite-env" virtual environment by issuing:

```
python3 -m venv tflite-env
```

This will create a folder called tflite1-env inside the tflite1 directory. The tflite1-env folder will hold all the package libraries for this environment. Next, activate the environment by issuing:

```
source tflite-env/bin/activate
```



### Step 1c. Install TensorFlow Lite dependencies and OpenCV

```
bash get_pi_requirements.sh
```

### Step 1d. Set up TensorFlow Lite detection model
copy model.tflite and labels.txt from laptop to raspberry pi.


## Step 2 - Run Edge TPU Object Detection Models on the Raspberry Pi Using the Coral USB Accelerator






















#### Use JETSON Nano
Everything is same except we need to install tensorflow-gpu




### Step 2. Build TensorFlow From Source to quantize the saved model

#### Step 2a. Install MSYS2
[MSYS2 website](https://www.msys2.org/). After installing, open MSYS2 and issue:

```
pacman -Syu
```

After it's completed, close the window, re-open it, and then issue the following two commands:

```
pacman -Su
pacman -S patch unzip
```


#### Step 2c. Update Anaconda and create tensorflow-build environment
```
conda update -n base -c defaults conda
conda update --all
```

```
conda create -n tensorflow-build pip python=3.8
conda activate tensorflow-build
```


Update pip by issuing:

```
python -m pip install --upgrade pip
```

We'll use Anaconda's git package to download the TensorFlow repository, so install git using:

```
conda install -c anaconda git
```

Next, add the MSYS2 binaries to this environment's PATH variable by issuing:

```
set PATH=%PATH%;C:\msys64\usr\bin
```

#### Step 2d. Download Bazel and Python package dependencies
```
pip install six numpy wheel
pip install keras_applications --no-deps
pip install keras_preprocessing --no-deps
```

```
conda install -c conda-forge bazel=3.1.0
```

#### Step 2d. Download TensorFlow source and configure build
```
mkdir C:\tensorflow-build
cd C:\tensorflow-build
```

```
git clone https://github.com/tensorflow/tensorflow.git 
cd tensorflow 
``` 

```
git checkout r2.4
```


```
python ./configure.py
```

#### Step 2e. Build TensorFlow package


```
bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package 
```


```
bazel-bin\tensorflow\tools\pip_package\build_pip_package C:/tmp/tensorflow_pkg 
```
Wheel file is in  C:\tmp\tensorflow_pkg.

#### Step 2f. Install TensorFlow and test it out!

```
pip3 install C:/tmp/tensorflow_pkg/tensorflow-2.4.0-cp38-cp38-win_amd64.whl
```

## Check

```
python
```

Once the shell is opened, issue these commands:

```
>>> import tensorflow as tf
>>> tf.__version__
```

```
exit()
```

### Step 3. Use TOCO to Create Optimzed TensorFlow Lite Model, Create Label Map, Run Model
Although we've already exported a frozen graph of our detection model for TensorFlow Lite, we still need run it through the TensorFlow Lite Optimizing Converter (TOCO) before it will work with the TensorFlow Lite interpreter. TOCO converts models into an optimized FlatBuffer format that allows them to run efficiently on TensorFlow Lite. We also need to create a new label map before running the model.

#### Step 3a. Create optimized TensorFlow Lite model































