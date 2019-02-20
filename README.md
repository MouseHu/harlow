<p align="center">
  <img src="https://raw.githubusercontent.com/mtrazzi/two-step-task/master/results/arxiv/arxiv_40k/train/training_40k_meta_rl.gif">
</p>

This repository presents our attempt at reproducing the simulations regarding the two-step task and the harlow task as described in the two papers:
- [Learning to Reinforcement Learn, Wang et al., 2016](https://arxiv.org/pdf/1611.05763v1.pdf)
- [Prefrontal cortex as a meta-reinforcement learning system, Wang et al., 2018](https://www.biorxiv.org/content/biorxiv/early/2018/04/13/295964.full.pdf)


To reproduce the Harlow Task, we used *DeepMind Lab*, a 3D learning environment.

*DeepMind Lab* provides a suite of challenging 3D navigation and puzzle-solving
tasks for learning agents. Its primary purpose is to act as a testbed for
research in artificial intelligence, especially deep reinforcement learning.
For more infor about DeepMind Lab, you can checkout their repo [here](https://github.com/deepmind/lab).

## Getting Started

To download the repository:

`git clone https://github.com/mtrazzi/lab.git`

You also need to fetch the [`meta-rl`](https://github.com/mtrazzi/meta-rl.git) repository:

```
cd lab
git checkout floydhub
cd python
git submodule init
git submodule update
cd ..
```

Then install the repository:
```
sh install.sh
sh build.sh
```

To train the *Harlow Agent* present in `meta-rl` run:
`sh train.sh`


# Results

## Harlow task

<p align="center">
  <img src="/docs/template/harlow_task.gif" alt="Harlow Task">
</p>

## Two-step task

We reproduced the plot from [Prefrontal cortex as a meta-reinforcement learning system](https://www.biorxiv.org/content/biorxiv/early/2018/04/13/295964.full.pdf) (Simulation 4, Figure b), on the right). We launched n=8 trainings using different seeds, but with the same hyperparameters as the paper, to compare to the results obtained by Wang et al.

For each seed, the training consisted of 20k episodes of 100 trials (instead of 10k episodes of 100 trials in the paper). The reason for our number of episodes choice is that, in our case, the learning seemed to converge after around ~20k episodes for most seeds.

![reward curve](results/biorxiv/final/reward_curve.png)

After training, we tested the 8 different models for 300 further episodes (like in the paper), with the weights of the LSTM being fixed.

Here is the side by side comparison of our results (on the left) with the results from the paper (on the right):

![side by side](results/biorxiv/final/side_by_side.png)


# Credits

This work uses [awjuliani's Meta-RL implementation](https://github.com/awjuliani/Meta-RL).

For the two-step task, I worked closely with [Yasmine Hamdani](https://github.com/Yasmine-H), and for the harlow task and FloydHub installation, I couldn't have done without my dear friend [Kevin Costa](https://github.com/kcosta42), and the additional details provided kindly by [Jane Wang](http://www.janexwang.com/).

