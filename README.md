
# Racing with Data Augmentation

This is the code repository of our racing car reinforcement learning project for the "EE618-Theory and Methods for Reinforcement Learning" EPFL course.
Our main contribution is a data augmentation wrapper in `utils/wrappers.py` and optimal task PPO hyperparameters in `hyperparameters/ppo.yml`. This repository forks rhe `rl-baselines3-zoo`, gathering many state-of-the-art reinforcement learning algorithms like PPO and SAC.

## Installation

This work uses the OpenAi's gym donkey car environment already integrated into this repository. It also uses the Donkey Car simulator. See the installation process below:

1. Install and unzip the Donkey Car Simulator [here](https://github.com/tawnkramer/gym-donkeycar/releases) and place it in this repository.
2. Install donkey car modules with:
```
conda install mamba -n base -c conda-forge
mamba env create -f install/envs/ubuntu.yml
conda activate donkey
cd donkeycar
pip install -e .[pc]
```

3. Install gym donkey car modules with:
```
cd gym-donkeycar
pip install -e .[gym-donkeycar]
```

4. Install parent repository packages with:
```
apt-get install swig cmake ffmpeg
pip install -r requirements.txt
```

## Augmentation Wrapper

The augmentation wrapper transforms Image observations with some data augmentation function re-updated every `rand_aug_frequency` iterations. In this project, these wrappers carry bother Image Transformation and Track augmentation.

### Train

After setting up the config file `hyperparameters/ppo.yml,` run the following command:

```
python train.py --algo ppo --env donkey-generated-track-v0 --eval-freq -1 --save-freq 100000 --env-kwargs exe_path:"'path/to/repo/EE618-Driving-Car/DonkeySimLinux/donkey_sim.x86_64'" port:"9091" --tensorboard-log ../log_sac/
```

1. Image Transformation

Set right hyperparamters under the `donkey-generated-track-v0/utils.wrappers.AugmentationWrapper` field in `ppo.yml`. To reproduce our best results, set `pool_name : cutout`.

2. Track Augmentation

To reproduce our best results, set `track_augment : True`.

### Test

To test on the training circuit:

```
python enjoy.py --algo ppo --env donkey-generated-track-v0 --exp-id exp_id -f logs/
```

If you want to test on another circuit, you can select it once the Unity Donkey Car Simulator is open.

## VAE

We use the code from https://github.com/araffin/learning-to-drive-in-5-minutes.

0. Record data
```
python record_data.py --max-steps 10000 -f  path-to-record/folder/ 
```

1. Train a VAE:
```
python -m vae.train --n-epochs 500 --verbose 0 --z-size 64 -f path-to-record/folder/
```

2. Explore Latent Space

```
python -m vae.enjoy_latent -vae logs/level-0/vae-8.pkl
```
