# DeepClean
# Step 1: Download the production repo
Download the production git-repo by running
```sh
git clone git@git.ligo.org:tri.nguyen/deepclean-prod.git
```
# Step 2: Set up the conda environment
Use the setup file environment.yml which is there in the downloaded repo. Run the command
```sh
cd deepclean-prod
conda env create -f environment.yml
```
# Step 3: Install the deepclean production repo
To do this, first activate the environment `dc-prod-py36` (the default name in the setup file) and do `pip install` from within the repo, ie,
```sh
conda activate dc-prod-py36
cd deepclean-prod
pip install .
```
Once this is done, you are all set to start the deepclean run.


# Run the deepclean 
## Step 1: Set up the `basic_example.ini` file
The content of an example `basic_example.ini` file is given below:
```sh
[config]

# General input/output setting
out_dir = outdir
out_file = H-H1_HOFT_DC-1251331218-1000.gwf
train_dir = train_dir
out_channel = H1:GDS-CALIB_STRAIN_DC
save_dataset = True
load_dataset = True
log = log.log

# GPU setting
device = cuda:0

# Dataset properties
# GPS time to clean
clean_t0 = 1251333218
clean_duration = 1000
# GPS time to train on
train_t0 = 1251331218
train_duration = 2000
# sampling rate
fs = 512
# channel list
chanslist = example_configs/chanslist.ini

# Preprocessing
# timeseries properties
train_kernel = 8
train_stride = 0.25
clean_kernel = 8
clean_stride = 4
window = hanning
pad_mode = median
# bandpass filter
filt_fl = 55
filt_fh = 65
filt_order = 8

# Training
train_frac = 0.9
batch_size = 32
max_epochs = 20
num_workers = 4
# optimizer
lr = 1e-3
weight_decay = 1e-5
# loss function
fftlength = 2
psd_weight = 1.0
mse_weight = 0.0
```

## Step 2: Prepare the list of witness channels (chanslist).
The first name in the list should be the target channel that is to be cleaned.
An example chanslist for H1 detector is here:
```sh
H1:GDS-CALIB_STRAIN 
H1:PEM-CS_MAINSMON_EBAY_1_DQ
H1:ASC-INP1_P_INMON
H1:ASC-INP1_Y_INMON
H1:ASC-MICH_P_INMON
H1:ASC-MICH_Y_INMON
H1:ASC-PRC1_P_INMON
H1:ASC-PRC1_Y_INMON
H1:ASC-PRC2_P_INMON
H1:ASC-PRC2_Y_INMON
H1:ASC-SRC1_P_INMON
H1:ASC-SRC1_Y_INMON
H1:ASC-SRC2_P_INMON
H1:ASC-SRC2_Y_INMON
H1:ASC-DHARD_P_INMON
H1:ASC-DHARD_Y_INMON
H1:ASC-CHARD_P_INMON
H1:ASC-CHARD_Y_INMON
H1:ASC-DSOFT_P_INMON
H1:ASC-DSOFT_Y_INMON
H1:ASC-CSOFT_P_INMON
H1:ASC-CSOFT_Y_INMON
```
Write this to a file and name it as `chanslist.ini`. If a different name is used, remember to also change it in the `basic_example.ini`.

## Step 3: Training
To do this, run the script `run_train.py` from the terminal with `basic_example.ini` as the default argument.
```sh
python ./path/to/the/script/run_train.py basic_example.ini
```
This will produce the training model and loss function in `train_dir` as you have set in the `basic_example.ini`

## Step 4: Cleaning
To do this, run the script `run_clean.py` from the terminal with `basic_example.ini` as the default argument.
```sh
python ./path/to/the/script/run_clean.py basic_example.ini
```
It use the model in `train_dir` to clean the data, and produce the cleaned data in `out_dir` as you have set in the `basic_example.ini`.
