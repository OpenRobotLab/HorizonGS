# Horizon-GS: Unified 3D Gaussian Splatting for Large-Scale Aerial-to-Ground Scenes

### [Project Page](https://city-super.github.io/horizon-gs/) | [Paper](https://arxiv.org/pdf/2412.01745)

[Lihan Jiang*](https://jianglh-whu.github.io/), [Kerui Ren*](https://github.com/tongji-rkr), [Mulin Yu](https://scholar.google.com/citations?user=w0Od3hQAAAAJ), [Linning Xu](https://eveneveno.github.io/lnxu), [Junting Dong](https://jtdong.com/), [Tao Lu](https://github.com/inspirelt), [Feng Zhao](https://scholar.google.co.uk/citations?user=r6CvuOUAAAAJ&hl=en), [Dahua Lin](http://dahua.site/), [Bo Dai](https://daibo.info/) ✉️ <br />

## Overview

Implementation of Horizon-GS, a novel approach built upon Gaussian Splatting techniques, tackles the unified reconstruction and rendering for aerial and street views. Horizon-GS addresses the key challenges of combining these perspectives with a new training strategy, overcoming viewpoint discrepancies to generate high-fidelity scenes.

<p align="center">
<img src="assets/teaser.png" width=100% height=100% 
class="center">
</p>

## Installation

1. Clone this repo:

```
git clone https://github.com/city-super/Horizon-GS.git --recursive
cd Horizon-GS
```

2. Install dependencies

```
SET DISTUTILS_USE_SDK=1 # Windows only
conda env create --file environment.yml
conda activate horizon_gs
```

Here we use [gsplat](https://docs.gsplat.studio/main/) to unify the rendering process of different Gaussians. Considering the adaptation for 2D-GS, we choose [gsplat version](https://github.com/FantasticOven2/gsplat.git) which supports 2DGS.

## Data

First, create a ```data/``` folder inside the project path by 

```
mkdir data
```

Next, download the following data, and place them under a desired direcory, e.g. data/:

- The HorizonGS data is available in [Hugging Face](https://huggingface.co/datasets/BoDai/HorizonGS).
- The UCGS dataset are provided by the paper author [here](https://drive.google.com/file/d/1DjSB7GqORz5rUvB3KaAMIiGwX1mtmRjb/view). 
- The MatrixCity dataset can be downloaded from [Hugging Face](https://huggingface.co/datasets/BoDai/MatrixCity/tree/main)/[Openxlab](https://openxlab.org.cn/datasets/bdaibdai/MatrixCity)/[百度网盘[提取码:hqnn]](https://pan.baidu.com/share/init?surl=87P0e5p1hz9t5mgdJXjL1g). 

## Training

For training a small scene like Block_small, first generate the config and then run it:

```
# generate config, we have provided the config for all datasets in the config folder
python preprocess/data_preprocess.py --config config/<dataset>/config.yaml

# train coarse
python train.py --config config/<dataset>/coarse.yaml

# train fine
python train.py --config config/<dataset>/fine.yaml
```

For training a large scene like Block_A, first generate the config and then run it:

```
# generate config
python preprocess/data_preprocess.py --config config/<dataset>/config.yaml

# train coarse of each chunk
python train.py --config config/<dataset>/coarse.yaml

# train fine of each chunk
python train.py --config config/<dataset>/fine.yaml

# merge all chunks
python merge.py -m <path to trained model> --config config/<dataset>/config.yaml
```

## Evaluation

We keep the manual rendering function with a similar usage of the counterpart in [3D-GS](https://github.com/graphdeco-inria/gaussian-splatting), one can run it by 

```
python render.py -m <path to trained model> # Generate renderings
python metrics.py -m <path to trained model> # Compute error metrics on renderings
python export_mesh.py -m <path to trained model> # Export mesh
```

## Contact

- Lihan Jiang: mr.lhjiang@gmail.com
- Kerui Ren: renkerui@pjlab.org.cn

## Citation

If you find our work helpful, please consider citing:

```bibtex
@inproceedings{jiang2025horizon,
  title={Horizon-GS: Unified 3D Gaussian Splatting for Large-Scale Aerial-to-Ground Scenes},
  author={Jiang, Lihan and Ren, Kerui and Yu, Mulin and Xu, Linning and Dong, Junting and Lu, Tao and Zhao, Feng and Lin, Dahua and Dai, Bo},
  booktitle={Proceedings of the Computer Vision and Pattern Recognition Conference},
  pages={26789--26799},
  year={2025}
}
```

## LICENSE

Please follow the LICENSE of [3D-GS](https://github.com/graphdeco-inria/gaussian-splatting).

## Acknowledgement
We thank all authors from the following repositories for their excellent work:
- [3D Gaussian Splatting for Real-Time Radiance Field Rendering](https://github.com/graphdeco-inria/gaussian-splatting)
- [2D Gaussian Splatting for Geometrically Accurate Radiance Fields](https://github.com/hbb1/2d-gaussian-splatting)
- [Scaffold-GS: Structured 3D Gaussians for View-Adaptive Rendering](https://github.com/city-super/Scaffold-GS)
- [Octree-GS: Towards Consistent Real-time Rendering with LOD-Structured 3D Gaussians](https://github.com/city-super/Octree-AnyGS)
- [VastGaussian: Vast 3D Gaussians for Large Scene Reconstruction](https://github.com/kangpeilun/VastGaussian)
- [A Hierarchical 3D Gaussian Representation for Real-Time Rendering of Very Large Datasets](https://github.com/graphdeco-inria/hierarchical-3d-gaussians)
- [Drone-assisted Road Gaussian Splatting with Cross-view Uncertainty](https://github.com/SainingZhang/uc-gs/)
- [gsplat](https://github.com/nerfstudio-project/gsplat)
