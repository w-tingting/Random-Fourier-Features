Random Fourier Features RFF Batch SVC for MNIST
====

This python script provides an example of RFF SVC with batch learning on MNIST dataset.
Our module for Random Fourier Features (PyRFFF.py) needs scikit-learn as a backend of SVM solver therefore you need to install scikit-learn.

However, you do not need to pay much attention to this example because
[non-batch learning approach](../rff_svc_for_mnist/README.md)
(i.e. usual SVC training using all dataset) now shows higher performance than the batch learning approach.

The reason why I implemented the batch learning-based RFF SVC module is that
the batch learning showed higher accuracy than non-batch learning in my previous implementation.
The author's guess is that batch learning exceeds non-batch learning in case of a larger and more complicated dataset than MNIST.


## Preparation

If you don't have scikit-learn, please run the following as root to install it:

```console
$ pip3 install scikit-learn
```

Also, you need to download and convert MNIST data before running the sample code by the following command:

```console
$ cd ../../dataset/mnist
$ python3 download_and_convert_mnist.py
```

Original MNIST data will be downloaded automatically and converted to .npy file.


## Usage

After generating MNIST .npy files, run the sample script by the following command:

```console
$ python3 main_rff_batch_svc_for_mnist.py
```

You possibly have many warnings from LIBLINEAR (e.g. `ConvergenceWarning: Liblinear failed to converge, ...`).
You can ignore these warnings by redirecting STDERR to /dev/null like the following:

```console
    $ python3 sample_rff_batch_svc_for_mnist.py 2> /dev/null
```


## Results of Support Vector Classification with batched RFF

In my computing environment (CPU: Intl Core i5 5250U, RAM: 4GB), I've got the following results:

| Method                           | Training time (sec) | Prediction time (us)| Score (%) |
| :------------------------------: | :-----------------: | :-----------------: | :-------: |
| SVM w/ batched RFF <br> d = 1024 | 2062.2 sec          | 108.6 us            | 96.4 %    |

<div align="center">
  <img src="./figure_rff_batch_svc_for_mnist.png" width="480" height="320" alt="Accuracy for each epoch in SVM with batch RFF" />
</div>

As the author pointed out at the top of this document, the above results are worse than [usual SVC training](../rff_svc_for_mnist/README.md).
However, the author's guess is that batch learning exceeds non-batch learning in case of a larger and more complicated dataset than MNIST.

