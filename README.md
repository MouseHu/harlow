<p align="center">
  <img src="/docs/template/harlow_task.gif" alt="Harlow Task">
</p>

<p align="center">
  <b>In environment footage, captured by human player</b>
</p>

[![Run on FloydHub](https://img.shields.io/badge/Run%20on-FloydHub-blue.svg)](https://floydhub.com/run?template=https://github.com/mtrazzi/harlow)

⚠ **Note**: This is the code for my article [Meta-Reinforcement Learning](https://blog.floydhub.com/author/michaeltrazzi/) on FloydHub. This repository is for DeepMind Lab and the Harlow task environment. For the git submodule containing all the tensorflow code and the DeepMind Lab wrapper, see [this repository](https://github.com/mtrazzi/meta_rl). For the two-step task see [this repository](https://github.com/mtrazzi/two-step-task) instead.⚠

Here, we try to reproduce the simulations regarding the harlow task as described in the two papers:
- [Learning to Reinforcement Learn, Wang et al., 2016](https://arxiv.org/pdf/1611.05763v1.pdf)
- [Prefrontal cortex as a meta-reinforcement learning system, Wang et al., 2018](https://www.biorxiv.org/content/biorxiv/early/2018/04/13/295964.full.pdf)

To reproduce the Harlow Task, we used *DeepMind Lab*, a 3D learning environment that provides a suite of challenging 3D navigation and puzzle-solving
tasks for learning agents. Its primary purpose is to act as a testbed for
research in artificial intelligence, especially deep reinforcement learning.
For more info about DeepMind Lab, you can checkout their repo [here](https://github.com/deepmind/lab).

# Getting Started

1. Clone the repository:

```$ git clone https://github.com/mtrazzi/harlow.git```

2. Change current directory to [`harlow/python`](https://github.com/mtrazzi/harlow/tree/master/python):

```$ cd harlow/python```

2. Fetch the git submodule [`meta_rl`](https://github.com/mtrazzi/meta_rl.git):

```
python$ git submodule init
python$ git submodule update --remote
```

4. Change current directory to the root of the repo:

```
python$ cd ..
```

3. Make sure that everything is correctly setup in `WORKSPACE` and `python.BUILD` (cf. section [Configure the repo](https://github.com/mtrazzi/harlow#configure-the-repo)).

4. Install dependencies and build with bazel:

```
harlow$ sh install.sh
harlow$ sh build.sh
```

5. Train the *meta-RL Agent*:

```
harlow$ sh train.sh
```

## Useful commands

To run the harlow environment as human, run:

```
harlow$ bazel run :game -- --level_script=contributed/psychlab/harlow
```

For a live example of the harlow agent, run

```
harlow$ bazel run :python_harlow --define graphics=sdl --incompatible_remove_native_http_archive=false -- --level_script contributed/psychlab/harlow  --width=640 --height=480
```

# Directory structure

``` bash
harlow
├── WORKSPACE
├── python.BUILD
└── python
    ├── meta-rl
    │   ├── harlow.py
    │   └── meta_rl
    │       ├── worker.py
    │       └── ac_network.py
    └── dmlab_module.c
└── data
    └── brady_konkle_oliva2008
        ├── 0001.png
        ├── 0002.png
        └── README.md
```

Most of our code is in the repository [`python`](https://github.com/mtrazzi/harlow/tree/master/python) where you'll find our git submodule [`meta-rl`](https://github.com/mtrazzi/meta_rl), that contains the three most important files: [`harlow.py`](https://github.com/mtrazzi/meta_rl/blob/master/harlow.py), [`meta_rl/worker.py`](https://github.com/mtrazzi/meta_rl/blob/master/meta_rl/worker.py) and [`meta_rl/ac_network.py`](https://github.com/mtrazzi/meta_rl/blob/master/meta_rl/ac_network.py).

⚠ **For more details about those three important files, check out the README of the [`meta-rl`](https://github.com/mtrazzi/meta_rl) repo, that also contains more information about the different architectures we tried and the on-going projects.** ⚠

Apart from that, the other essential files are:
- The Lua script for the Harlow Task Environment: [`game_scripts/levels/contributed/psychlab/factories/harlow_factory.lua`](https://github.com/mtrazzi/harlow/blob/master/game_scripts/levels/contributed/psychlab/factories/harlow_factory.lua)
- The file [`dmlab_module.c`](https://github.com/mtrazzi/harlow/blob/master/python/dmlab_module.c) that creates a Python API to use DeepMind Lab.
- The folder [data/brady_konkle_oliva2008](https://github.com/mtrazzi/harlow/tree/master/data/brady_konkle_oliva2008) that you can tweak to use your own dataset using the instructions in [Dataset](https://github.com/mtrazzi/harlow/blob/master/README.md#dataset) (the instructions in the README.md are to download the data for [this](https://www.pnas.org/content/105/38/14325) paper).

# Results

## Goal
We tried to reproduce the results from [Prefrontal cortex as a meta-reinforcement learning system](https://www.biorxiv.org/content/biorxiv/early/2018/04/13/295964.full.pdf) (see Simulation 5 in Methods). We launched n=5 trainings using 5 different seeds, with the same hyperparameters as the paper, to compare to the results obtained by Wang et al.

## Main differences
- we removed the CNN.
- we replaced the stacked LSTM with a 48 units LSTM (same as for the Harlow task).
- we drastically reduced the action space so that the agent could do only left and right actions (that would directly target the center of the image.
- we add artificial `NO-OPS` to force the agent to fixate for multiple frames (and to remove the noise at the beginning of an episode).
- we used a dataset of 42 pictures, instead of 1000 images samples form ImageNet.
- we used only 1 thread, on one CPU, instead of 32 threads on 32 GPUs.

For each seed, the training consisted in ~10k episodes (instead of 10^5 episodes per thread (32 threads) in the paper). The reason for our number of episodes choice is that, in our case, the learning seemed to have reached a threshold after ~7k episodes for all seeds.

## Dataset
For our dataset we used profile pictures of our friends at 42 (software enginneering education), resized to 256x256 (to tweak the dataset to your own needs, see [here](https://github.com/mtrazzi/harlow/blob/master/README.md#dataset)).

Example of a run of the agent on the dataset (after training):
<p align="center">
  <img src="/docs/template/frames.gif" alt="observations">
</p>

What the agent sees for the run above (after pre-processing):
<p align="center">
  <img src="/docs/template/agent_view.gif" alt="agent view">
</p>

## Reward Curve

Here is the reward curve (one color per seed) after ~10k episodes (which took approximately 3 days to train) on FloydHub's CPU:

<p align="center">
  <img src="/docs/template/reward_cuve_5_seeds_42_images.png" alt="reward curve 5 seeds 42 images">
</p>

# Configure the repo

## Dataset

To tweak the dataset:

1. add your own images in data/brady_konkle_oliva2008/. Use the following names: `0001.png`, `0002.png`, etc. The sizes of the images must be 256x256, like in the [original](https://github.com/deepmind/lab/tree/master/data/brady_konkle_oliva2008) `brady_konkle_oliva2008` dataset.
2. change `DATASET_SIZE` in [`game_scripts/datasets/brady_konkle_oliva2008.lua`](https://github.com/mtrazzi/harlow/blob/master/game_scripts/datasets/brady_konkle_oliva2008.lua) to your number of images.
3. change `TRAIN_BATCH` and `TEST_BATCH` to the number of images you will use respectively in train and test in [`game_scripts/levels/contributed/psychlab/factories/harlow_factory.lua`](https://github.com/mtrazzi/harlow/blob/master/game_scripts/levels/contributed/psychlab/factories/harlow_factory.lua).
4. change `DATASET_SIZE` in [`python/meta-rl/harlow.py`](https://github.com/mtrazzi/meta_rl/blob/master/harlow.py) to your number of images.
5. change `DATASET_SIZE` in [`python/meta-rl/meta_rl/ac_network.py`](https://github.com/mtrazzi/meta_rl/blob/master/meta_rl/ac_network.py) to your number of images.

## Linux/Python3 vs. MacOS/Python2.7

The DeepMind Lab release supports Python2.7, but you can find some documentation for Python3 [here](https://github.com/deepmind/lab/blob/master/docs/users/build.md#python-dependencies).

Currently, our branch [`python2`](https://github.com/mtrazzi/harlow/tree/python2) supports Python2.7 and MacOS, and our branch [`master`](https://github.com/mtrazzi/harlow) supports Python3.6 and Linux.
- The branch [`python2`](https://github.com/mtrazzi/harlow/tree/python2) should work on iMac's available at [42](https://www.42.us.org/) (software engineering education).
- The branch [`master`](https://github.com/mtrazzi/harlow) was tested on FloydHub's instances (using `Tensorflow 1.12` and `CPU`). To change for `GPU`, change `tf.device("/cpu:0")` with `tf.device("/device:GPU:0")` in [`harlow.py`](https://github.com/mtrazzi/meta_rl/blob/master/harlow.py).

## Dependencies

All the pip packages should be either installed on FloydHub or installed with [`install.sh`](https://github.com/mtrazzi/harlow/blob/master/install.sh).

However, if you want to run this repository on your machine, here are the requirements:
```
numpy==1.16.2
tensorflow==1.12.0
six==1.12.0
scipy==1.2.1
skimage==0.0
setuptools==40.8.0
Pillow==5.4.1
```

# Credits

This work uses [awjuliani's Meta-RL implementation](https://github.com/awjuliani/Meta-RL).

I couldn't have done without my dear friend [Kevin Costa](https://github.com/kcosta42), and the additional details provided kindly by [Jane Wang](http://www.janexwang.com/).
