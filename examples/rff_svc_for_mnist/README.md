Random Fourier Features: RFF for MNIST dataset
====

This python script provide an example for RFF SVC with MNIST dataset.
Our module for random fourier features (PyRFFF.py) needs scikit-learn as a backend of SVM solver therefore you need to install scikit-learn.
Also, this script support GPU inference of RFF SVC (only one-versus-the-rest is supported now). for running GPU inference, you need to install Tensorflow 2.x (Tensorflow 1.x is not supported).


## Installation

### Install Python packages

The training end validation script requires `docopt`, `scikit-learn` and, if you will run the inference on GPU, `tensorflow-gpu`.
If you don't have them, please run the following as root to install them:

```console
$ pip3 install docopt scikit-learn  # Necessary packages
$ pip3 install tensorflow-gpu       # Required only for GPU inference
```

### Docker image (alternative)

If you does not like to pollute your development environment, it is good idea to run everything inside Docker.
Scripts in this directory are execultable on [this docker image](https://hub.docker.com/repository/docker/tiskw/tensorflow).

```console
$ docker pull tiskw/tensorflow:2020-01-18    # Download docker image from DockerHub
$ cd PATH_TO_THE_ROOT_DIRECTORY_OF_THIS_REPO # Move to the root directory of this repository
$ docker run --rm -it --runtime=nvidia -v `pwd`:/work -w /work -u `id -u`:`id -u` tiskw/tensorflow:2020-01-18 bash
$ cd PATH_TO_THIS_DIRECTORY                  # Back to here (inside the docker container you launched).
```

If you don't need GPU support, `--runtime=nvidia` is not necessary.


## Dataset Preparation

Also, you need to download and convert MNIST data before running sample code by the following command:
```console
$ cd ../../dataset/mnist
$ python3 download_and_convert_mnist.py
```
Original MNIST data will be automatically downloaded, converted to .npy file and save them under mnist/ directory.


## Training

After generating MNIST .npy files, run sample scripts by the following command:

```console
$ python3 train_rff_svc_for_mnist.py kernel   # Run kernel SVC training
$ python3 train_rff_svc_for_mnist.py rff      # Run SVC with RFF training
```

Default hyper parameter settings are reccomended one, however, you are able change the parameters by command option.
The following command will generate `result.pickle` in which trained model and command arguments are stored.
See `train_rff_svc_for_mnist.py --help` for details.


## Inference

You are able to run inference by the following command:

```console
$ python3 valid_rff_svc_for_mnist.py cpu         # Inference on CPU using scikit-learn
$ python3 valid_rff_svc_for_mnist.py tensorflow  # Inference on GPU using Tensorflow
```


## Results of Support Vector Classification with RFF

In my computing environment (CPU: Intl Core i5 5250U, RAM: 4GB), I've got the following results:

| Method                   | Training time (sec) | Inference time (us) | Score (%) |
| :---------------------:  | :-----------------: | :-----------------: | :-------: |
| Kernel SVM               | 177.8 sec           | 4644.9 us           | 96.3 %    |
| SVM w/ RFF <br> d = 512  | 126.6 sec           | 39.0 us             | 96.5 %    |
| SVM w/ RFF <br> d = 1024 | 226.7 sec           | 96.1 us             | 97.5 %    |

As for inference using GPU, I've got the following result:

| Method     | Dimention of RFF | Device  | Batch size | Inference time (us) | Score (%) |
| :-------:  | :--------------: | :-----: | :---------:| :-----------------: | :-------: |
| SVM w/ RFF | 512              | CPU     | -          | 39.0 us             | 96.5 %    |
| SVM w/ RFF | 1024             | CPU     | -          | 96.1 us             | 97.5 %    |
| SVM w/ RFF | 1024             | TitanXp | 2,000      | 2.38 us             | 97.5 %    |

<div align="center">
  <img src="Inference_Time_and_Accuracy_on_MNIST.png" width="480" height="320" alt="Accuracy for each epochs in SVM with batch RFF" />
</div>

Where score means test accuracy of MNIST dataset and inference time means inference time for one image.

Commonly used techniques like data normalization and dimention reduction using PCA is also used in the above analysis.
See comments in the Python script for details.

Score of RFF is slightly better than kernel SVM, moreover, prediction time of RFF is amazingly faster.
On the otherhand, learning time of RFF can be longer than kernel SVM if dimention of RFF is large.

The following figures shows a tradeoff between accuracy and inference time of RFF.

<div align="center">
  <img src="figure_rff_and_orf_svc_for_mnist.png" width="480" height="320" alt="Accuracy for each epochs in SVM with batch RFF" />
</div>

