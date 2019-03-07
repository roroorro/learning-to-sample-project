# Learning to sample variations project

Variations on S-NET from "learning to sample" - https://github.com/orendv/learning_to_sample


### Installation

The original code was tested on Python 2.7.12, TensorFlow 1.2.1, CUDA 8.0 and cuDNN 5.1.1 on Ubuntu 16.04.
We tested our new variaitons on Python 2.7.15, Tensorflow 1.12.0, CUDA 9.0 and cuDNN 7.2.1 on Ubuntu 16.04.
The original setup tested by the original code should probably work as well.


You may need to install h5py and wget.

As explained in the original github, you might need to change the first few lines of the make file to point to your nvcc, tensorflow and cuda libraries. Also other changes might be needed depending on your setup.

Compile the structural losses using the make file:

```
cd structural_losses/
make
```





### Usage

#### PointNet training

To train PointNet on ModelNet40 or MNIST run one of the following commands providing the saving directory:

```
python train_classifier.py --log_dir log/baseline/PointNet1024

python train_classifier_mnist.py --log_dir log/baseline/PointNet1024mnist
```

#### Unsupervised S-NET

To train S-NET with unlabled data run one of the following commands providing the saved PointNet model, number of points to sample and log directory:

```
python train_SNET_unsupervised.py --classifier_model_path log/baseline/PointNet1024/model.ckpt --num_out_points 64 --log_dir log/SNET64UNSUPERVISED

python train_SNET_unsupervised_mnist.py --classifier_model_path log/baseline/PointNet1024mnist/model.ckpt --num_out_points 32 --log_dir log/SNET32UNSUPERVISEDmnist
```

To evaluate run:

```
python evaluate_SNET_unsupervised.py --sampler_model_path log/SNET64UNSUPERVISED/model.ckpt --dump_dir log/SNET64UNSUPERVISED/eval --num_out_points 64

python evaluate_SNET_unsupervised.py --sampler_model_path log/SNET64UNSUPERVISEDmnist/model.ckpt --dump_dir log/SNET64UNSUPERVISEDmnist/eval --num_out_points 32
```


#### Unsupervised S-NET with threashold

To use a threashold when training, run the following command providing the previous arguments as well as the threashold value

```
python train_SNET_unsupervised_threashold.py --unsupervised_threashold 0.8 --classifier_model_path log/baseline/PointNet1024/model.ckpt --num_out_points 64 --log_dir log/SNET64UNSUPERVISEDTH8
```

To evaluate run:

```
python evaluate_SNET_unsupervised.py --sampler_model_path log/SNET64UNSUPERVISEDTH8/model.ckpt --dump_dir log/SNET64UNSUPERVISEDTH8/eval --num_out_points 64
```


#### S-NET with smaller dataset

To train S-NET only on a part of the MNIST dataset run the following command providing the precentage of the data you want to use. For example if you want to use 20% run:

```
python train_SNET_mnist.py --classifier_model_path log/baseline/PointNet1024mnist/model.ckpt --num_out_points 16 --log_dir log/SNET16mnistp2 --part_of_data 0.2
python evaluate_SNET_mnist.py --sampler_model_path log/SNET16mnistp2/model.ckpt --dump_dir log/SNET16mnistp2/eval --num_out_points 16
```

### Acknowledgment

Our project and code are based on the work by Oren Dovrat et al. available at https://github.com/orendv/learning_to_sample.
We want to thank the authors for publishing their code.

We also want to thank on the work by Qi et al. available at https://github.com/charlesq34/pointnet for publishing their code.

