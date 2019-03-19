<p align="center">
  <img src="/docs/template/harlow_task.gif" alt="Harlow Task">
</p>

<p align="center">
  <b>In environment footage, captured via human player</b>
</p>

[![Run on FloydHub](https://img.shields.io/badge/Run%20on-FloydHub-blue.svg)](https://floydhub.com/run?template=https://github.com/mtrazzi/harlow)

##### This the code for my article [Meta-Reinforcement Learning](https://blog.floydhub.com/author/michaeltrazzi/) on FloydHub.

⚠ **Note**: This repository is for the Harlow task. For the two-step task see [this repository](https://github.com/mtrazzi/two-step-task) instead.⚠

This repository presents our attempt at reproducing the simulations regarding the harlow task as described in the two papers:
- [Learning to Reinforcement Learn, Wang et al., 2016](https://arxiv.org/pdf/1611.05763v1.pdf)
- [Prefrontal cortex as a meta-reinforcement learning system, Wang et al., 2018](https://www.biorxiv.org/content/biorxiv/early/2018/04/13/295964.full.pdf)

To reproduce the Harlow Task, we used *DeepMind Lab*, a 3D learning environment that provides a suite of challenging 3D navigation and puzzle-solving
tasks for learning agents. Its primary purpose is to act as a testbed for
research in artificial intelligence, especially deep reinforcement learning.
For more info about DeepMind Lab, you can checkout their repo [here](https://github.com/deepmind/lab).

# Getting Started

1. Clone DeepMind Lab:

```git clone https://github.com/mtrazzi/harlow.git```

2. Fetch the git submodule [`meta_rl`](https://github.com/mtrazzi/meta_rl.git):

```
cd harlow/python
git submodule init
git submodule update --remote
cd ..
```

3. Make sure that everything is correctly setup in `WORKSPACE` and `python.BUILD`

4. Install dependencies and build with bazel:

```
sh install.sh
sh build.sh
```

5. Train the *Harlow Agent* present in `meta_rl`:

```sh train.sh```

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

Most of our code is in [`python`](https://github.com/mtrazzi/harlow/tree/master/python).

In python, you'll find our git submodule [`meta-rl`](https://github.com/mtrazzi/meta_rl), that contains the three most important files: [`harlow.py`](https://github.com/mtrazzi/meta_rl/blob/master/harlow.py), [`meta_rl/worker.py`](https://github.com/mtrazzi/meta_rl/blob/master/meta_rl/worker.py) and [`meta_rl/ac_network.py`](https://github.com/mtrazzi/meta_rl/blob/master/meta_rl/ac_network.py).

Apart from that, the other essential files are:
- The Lua script for the Harlow Task Environment: [`game_scripts/levels/contributed/psychlab/factories/harlow_factory.lua`](https://github.com/mtrazzi/harlow/blob/master/game_scripts/levels/contributed/psychlab/factories/harlow_factory.lua)
- The file [`dmlab_module.c`](https://github.com/mtrazzi/harlow/blob/master/python/dmlab_module.c) that creates a Python API to use DeepMind Lab.
- The folder [data/brady_konkle_oliva2008](https://github.com/mtrazzi/harlow/tree/master/data/brady_konkle_oliva2008) that you can tweak to use your own dataset using the instructions in [Dataset](https://github.com/mtrazzi/harlow/https://github.com/mtrazzi/harlow/#Dataset) (the instructions in the README.md are to download the data for [this](https://www.pnas.org/content/105/38/14325) paper).

# Results

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

# Credits

This work uses [awjuliani's Meta-RL implementation](https://github.com/awjuliani/Meta-RL).

I couldn't have done without my dear friend [Kevin Costa](https://github.com/kcosta42), and the additional details provided kindly by [Jane Wang](http://www.janexwang.com/).
