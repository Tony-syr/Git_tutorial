This file created by ***Tony Salloom*** in 20/10/2023 to make smooth instalation and use of ***mmdetection3d***. If you want to read the original README file click [here](README_original.md)

## Notice:
- To ease the installation, an image called mmdetection3d is created on server 10.112.1.1. It containes most of the requirements, you can jump directly to install mmdetectio3d ans spconv 2.x.
- Installing BEVFusion from the original Github repository needs an old version of mmcv, which I couldn’t install. So I suggest to install it with mmdetection3d.
- This version is built on Python 3.7, and never tested with Python 3.8.

## Install BEVFusion with mmdetection3d:
- Make sure you have Python 3.7: ```python --version ```.
- Install torch 1.9.0 and torchvision 0.10.0 with CUDA 11.1:
```shell
  pip install torch==1.9.0+cu111 torchvision==0.10.0+cu111 -f https://download.pytorch.org/whl/torch_stable.html
```
- Install MMEngine, MMCV and MMDetection
```shell
pip install -U openmim
Pip install mmengine==0.8.5
Pip install mmcv==2.0.1
Pip install mmdet==3.1.0
```
- install spconv 2.x using: 
```shell
pip install cumm-cu111
pip install spconv-cu111
```
- install mmdetectio3d: 
```shell
git clone https://github.com/open-mmlab/mmdetection3d.git -b dev-1.x
# where -b dev-1.x" means checkout to the `dev-1.x` branch.
cd mmdetection3d
pip install -v -e .
```
- To use BEVFusion project navigate to mmdetection3d folder then build the project
```python
python projects/BEVFusion/setup.py develop
```
- Modify the code as follows unless it's already done
1. In the file “/projects/BEVFusion/bevfusion/bevfusion.py”, Line 169 and Line 153, replace ```torch.autocast(...)``` with ```torch.cuda.amp.autocast()```.
2. In the file “/projects/BEVFusion/bevfusion/transfusion_head.py”, Line 227, replace ```torch.autocast(...)``` with ```torch.cuda.amp.autocast()```.
3. In the file "/data/mmdetection3d/projects/BEVFusion/bevfusion/depth_lss.py", Line 295, replace ``` 1e-5, 1e5 ``` with ``` 1e-4, 1e4 ```.

Now you can use the [demo](https://github.com/open-mmlab/mmdetection3d/tree/main/projects/BEVFusion) to verify the insallation for nuscenes dataset.

## Training:
Notice that what is written in the Github repository doesn’t work for me, so for training we follow the following
1- You should train the lidar-only detector first
```python
python tools/train.py projects/BEVFusion/configs/bevfusion_lidar_voxel0075_second_secfpn_8xb4-cyclic-20e_nus-3d.py --work-dir projects/BEVFusion/output/Lidar_only_model
```
