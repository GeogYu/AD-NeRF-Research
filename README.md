# AD-NeRF-Research

Set up environment:


# Install MiniConda
MINICONDA_INSTALLER_SCRIPT=Miniconda3-4.5.4-Linux-x86_64.sh
MINICONDA_PREFIX=/usr/local
wget https://repo.continuum.io/miniconda/$MINICONDA_INSTALLER_SCRIPT
chmod +x $MINICONDA_INSTALLER_SCRIPT
./$MINICONDA_INSTALLER_SCRIPT -b -f -p $MINICONDA_PREFIX

# Update MiniConda
conda install --channel defaults conda python=3.6 --yes
conda update --channel defaults --all --yes

# Install MiniConda featuretools from conda-forge
conda install --channel conda-forge featuretools --yes

# Clone Original Research Paper Code
git clone https://github.com/YudongGuo/AD-NeRF.git

# Create Environment
source <path_of_conda.sh>
conda env create -f ./drive/MyDrive/AD-NeRF/environment.yml

# Install pytorch & pytorch3d
pip install torch
git clone https://github.com/facebookresearch/pytorch3d.git
source <path_of_conda.sh> && conda init bash
conda activate adnerf
cd pytorch3d && pip install -e .

# Upload this file to ./AD-NeRF/data_util/face_tracking/3DMM 
https://drive.google.com/file/d/1PL8vCIRVg1FtyXW5JKJ4hPibl0uawq26/view?usp=sharing

# Execute following command to convert BFM 
cd ./AD-NeRF/data_util/face_tracking/
python convert_BFM.py

# Training model
  ##################
  Training video must be in 25fps, mp4 format with encoding accepted by NeRF
  ##################
  # Preprocess video with audio for Training
  cd <Path_to_AD-NeRF_folder>  
  source <path_of_conda.sh> && conda init bash && conda activate adnerf
  conda info --envs
  bash process_data.sh <VideoFileName.mp4>
  
  # Training Head
  cd <Path_to_AD-NeRF_folder>  
  source <path_of_conda.sh> && conda init bash && conda activate adnerf
  python ./NeRFs/HeadNeRF/run_nerf.py --config ./dataset/<Name>/HeadNeRF_config.txt

  # Copy latest Trained Head.tar file into ./dataset/<Name>/logs/<Name_com>
  cd <Path_to_AD-NeRF_folder>
  source <path_of_conda.sh> && conda init bash && conda activate adnerf
  python ./NeRFs/TorsoNeRF/run_nerf.py --config ./dataset/<Name>/TorsoNeRF_config.txt

# Generate audiofile.npy
  source <path_of_conda.sh> && conda init bash && conda activate adnerf
  python3 ./data_util/deepspeech_features/extract_ds_features.py --input=<Path_to_Audio_File.wav>

  # Render Video
  cd <Path_to_AD-NeRF_folder>  
  source /usr/local/etc/profile.d/conda.sh && conda init bash && conda activate adnerf
  python ./NeRFs/TorsoNeRF/run_nerf.py --config ./dataset/Obama/TorsoNeRFTest_config.txt --aud_file=<Path_to_AudioFile.npy> --test_size=-1





