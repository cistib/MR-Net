# MR-Net
Code for the paper "Shape Registration with Learned Deformations for 3D Shape Reconstruction from Sparse and Incomplete Point Clouds", which is a deep learning network to reconstruct 3D cardiac mesh from stacked 2D contours (point cloud).
## Contents
- <a href="#Abstract">`Abstract`</a>
- <a href="#Network">`Network`</a>
- <a href="#Repo Contents">`Repo Contents`</a>
- <a href="#Package dependencies">`Package dependencies`</a>
- <a href="#Dataset">`Dataset`</a>
- <a href="#Training">`Training`</a>
- <a href="#Testing">`Testing`</a>
- <a href="#Demo">`Demo`</a>
- <a href="#Citation">`Citation`</a>

## Abstract:<a id="Abstract"/>
Shape reconstruction from sparse point clouds/images is a challenging and relevant task required for a variety of applications in computer vision and medical image analysis (e.g. surgical navigation, cardiac motion analysis, augmented/virtual reality systems). A subset of such methods, viz. 3D shape reconstruction from 2D contours, is especially relevant for computer-aided diagnosis and intervention applications involving meshes derived from multiple 2D image slices, views or projections. We propose a deep learning architecture, coined Mesh Reconstruction Network (MR-Net), which tackles this problem. MR-Net enables accurate 3D mesh reconstruction in real-time despite missing data and with sparse annotations. Using 3D cardiac shape reconstruction from 2D contours defined on short-axis cardiac magnetic resonance image slices as an exemplar, we demonstrate that our approach consistently outperforms state-of-the-art techniques for shape reconstruction from unstructured point clouds. Our approach can reconstruct 3D cardiac meshes to within 2.5-mm point-to-point error, concerning the ground-truth data (the original image spatial resolution is 1.8*1.8*10 mm^3). We further evaluate the robustness of the proposed approach to incomplete data, and contours estimated using an automatic segmentation algorithm. MR-Net is generic and could reconstruct shapes of other organs, making it compelling as a tool for various applications in medical image analysis.

## Network:<a id="Network"/>
The kernal idea is to use deep learning network to deforme the template mesh under the guidence of contours.
![image](https://github.com/XiangChen1994/MR-Net/blob/main/fig/MRNet.png)

## Repo Contents:<a id="Repo Contents"/>
This code is based on [Pixel2mesh](https://github.com/nywang16/Pixel2Mesh), where the GCN block and the mesh loss are mainly from it.
The point feature extraction is partially referred to [PointNet++](https://github.com/charlesq34/pointnet2).

## Package dependencies:<a id="Package dependencies"/>
This repository is based on Python2.7, Tensorflow and Tensorlayer.
The version of the main packages is as follows,
- Tensorflow==1.7.0
- tflearn

## Dataset:<a id="Dataset"/>
Our network is trained based on UKBB dataset. Each training sample should be organised as coutours-shape pair. The input contours (in the form of point clouds) are extracted from manual segmentation results, and the groun-truth is generated by traditional method (deforming a template mesh conditioned by the corresponding contours). If you want to train MR-Net by yourself but have no access to the UKBB, ACDC dataset could be another option.

The data folder should be like:
tree
`-- Data
    `-- Manual
        |-- samplexx.vtk
        |-- samplexx.vtk
		...
	`-- Shapes
        |-- samplexx.obj
        |-- samplexx.obj
		...
where input point clouds of contours are under '/Manual' folder and GT meshes are under '/Shapes' folder. You may need to change the path on the corresponding code files.

## Training:<a id="Training"/>
Use the following command to train the MR-Net.
```sh
 CUDA_VISIBLE_DEVICES=0 python train.py  --data_dir path/to/trainfile/
```

## Testing:<a id="Testing"/>
Use the following command to test the MR-Net. Chamfer Distance (CD), Earth Mover Distance (EMD), Hausdorff Distance (HD) and Point cloud to point cloud (PC-to-PC) error are evaluated in this paper.
```sh
CUDA_VISIBLE_DEVICES=0 python test.py --data_dir path/to/testfile/
```

## Demo:<a id="Demo"/>
To reconstruct 3D cardiac mesh with pretrained model from contours.
```sh
CUDA_VISIBLE_DEVICES=0 python demo.py --test_file demo/test.vtk
```

## Citation:<a id="Citation"/>
If you find this code useful in your research, please consider citing:
```
@article{chen2021shape,
  title={Shape registration with learned deformations for 3D shape reconstruction from sparse and incomplete point clouds},
  author={Chen, Xiang and Ravikumar, Nishant and Xia, Yan and Attar, Rahman and Diaz-Pinto, Andres and Piechnik, Stefan K and Neubauer, Stefan and Petersen, Steffen E and Frangi, Alejandro F},
  journal={Medical Image Analysis},
  volume={74},
  pages={102228},
  year={2021},
  publisher={Elsevier}
}
```