# HIBRID
HIBRID is built on two pipelines: [STAMP](https://github.com/KatherLab/STAMP) for image preprocessing and [MARUGOTO](https://github.com/KatherLab/marugoto/tree/survival) for model training. Please follow the instructions below to run the code.
## Installation 
For setting up a local environment, note that the following steps are for Ubuntu Linux systems. First, install OpenSlide using either the command below or the official installation instructions:
```
apt update && apt install -y openslide-tools libgl1-mesa-glx # libgl1-mesa-glx is needed for OpenCV
```
Second, install conda on your local computer, create an environment with Python 3.10, and activate it:
```
conda create -n stamp python=3.10
conda activate stamp
conda install -c conda-forge libstdcxx-ng=12
```
Then, install the STAMP package via pip:
```
pip install git+https://github.com/KatherLab/STAMP
```
Next, initialize STAMP and obtain the required configuration file, config.yaml, in your current working directory, by running the following command:
```
stamp init
```
To download required resources such as the weights of the feature extractor, run the following command:
```
stamp setup
```
This will trigger a prompt asking for your Hugging Face access key for the UNI model weights.
## Preprocessing
Ensure the Config file is filled with the correct PATH (Details can be viewed in the form of comments in the Config file)
```
stamp --config /preprocess/stamp/config.yaml preprocess  
```

## Example Commands
### Train
```
python model/marugoto/train.py \
-ct /path/to/clinical_table.csv \
-st /path/to/slide_table.csv \
-o /path/to/output_location \
-f /path/to/feature_directory \
-t OS OS_E DFS DFS_E
```
### Deploy
```
python model/marugoto/eval.py \
-ct /path/to/clinical_table.csv \
-st /path/to/clinical_slide_table.xlsx \
-o /path/to/eval_results \
-f /path/to/feature_directory \
-m /path/to/model_output \
-c cohort_name \
-t OS OS_E DFS DFS_E
```
### Additional Information
```
ct = clini table, using format:|PATIENT|FILENAME|OS|OS_E|DFS|DFS_E|
st = slide table, using format:|PATIENT| (required but redundant as slide info read from ct)
o = output location
f = feature directory
t = stats: OS overall survival, OS_E os event (i.e. dead/alive), DFS disease free status, DFS_E DFS event
m = model path (location of .pth output from train.py script)
c = cohort (additional name for output of eval.py)
```
