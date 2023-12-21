## TemporalStdMaxer: Maximize Temporal Context with Pooling & Standard Deviation for Temporal Action Localization


We release the training and testing code for  EPIC-Kitchen 100 (verb, noun).

## Operating Systems and Specs
* Ubuntu 18.04.5 LTS
* NVIDIA RTX A6000 
* NVIDIA-SMI 520.61.05 - Driver Version: 520.61.05 - CUDA Version: 11.8

## Table of Contents
1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Usage](#usage)
6. [Citation](#citation)

## Introduction
In this work, we focus on replacing the complex self-attention mechanism with our straightforward method, TemporalStdMaxer. This method introduces a combination of max pooling, average pooling, and standard deviation computation, offering a more adaptable and flexible approach to capturing diverse temporal characteristics. The key modification in TemporalStdMaxer has the potential to significantly enhance performance, especially in challenging datasets such as Epic-Kitchens. The TemporalStdMaxer implementation involves the application of max pooling and average pooling along the temporal dimension, followed by the calculation of standard deviations. This process dynamically selects the vector with a higher standard deviation, emphasizing diverse temporal features. The simplicity of TemporalStdMaxer contributes to its effectiveness in capturing intricate temporal dynamics within video. 
<div align="center">
  <img src="figures/common_architecture.png" width="1100px"/>
</div>

## Installation
#### a. Install packages
```bash
conda create -n TemporalMaxer python=3.9
conda activate TemporalMaxer
conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.3 -c pytorch -y
python -m pip install -r requirements.txt
pip install -e ./
```
#### b. Build NMS
Part of NMS is implemented in C++. The code can be compiled by

```shell
cd ./libs/utils; python setup.py install; cd ../..
```
The code should be recompiled every time you update PyTorch.

## Usage
### Reproduce Our Results on EPIC-Kitchens 100
##### Download Features and Annotations
- Download epic_kitchens.tar.gz from this [link](https://1drv.ms/u/s!AmoaChPnSuIOmwPfVx8f4wQYluKM?e=WyuEBZ), md5sum `6cbf312eb5025c0abbbf5d0eaa61e556`.
- Make `data` folder in the current code directory.
- The data folder structure should look like the following:
```bash
# This folder
├── configs
│   ├── temporalmaxer_epic_slowfast_noun.yaml
│   └── temporalmaxer_epic_slowfast_verb.yaml
│   └── ........
├── data 
│   ├── epic_kitchens
│       ├── annotations
│       └── features
├── eval.py
├── figures
├── libs
    ........
```
- Train and test EPIC-Kitchens 100 `VERB`
```bash
# training
./scripts/epic_verb/train.sh
# testing
./scripts/epic_verb/test.sh
```

- Train and test EPIC-Kitchens 100 `NOUN`
```bash
# training
./scripts/epic_noun/train.sh
# testing
./scripts/epic_noun/test.sh
```

* The results should be:

| Method                  |   0.1  |   0.2  |   0.3  |   0.4  |   0.5  |   Avg  |
|-------------------------|--------|--------|--------|--------|--------|--------|
| TemporalStdMaxer (verb) | 27.99  | 26.87  | 25.69  | 22.93  | 19.87  | 24.67  |
| TemporalStdMaxer (noun) | 26.64  | 25.92  | 24.14  | 21.55  | 17.97  | 23.30  |


