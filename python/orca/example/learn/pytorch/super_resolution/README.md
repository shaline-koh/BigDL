# Orca PyTorch Super Resolution example on BSDS300 dataset

We demonstrate how to easily run synchronous distributed Pytorch training using Pytorch Estimator of Project Orca in BigDL. This is an example using the efficient sub-pixel convolution layer to train on [BSDS3000 dataset](https://www2.eecs.berkeley.edu/Research/Projects/CS/vision/bsds/), using crops from the 200 training images, and evaluating on crops of the 100 test images. See [here](https://github.com/pytorch/examples/tree/master/super_resolution) for the original single-node version of this example provided by Pytorch. We provide two distributed PyTorch training backends for this example, namely "spark" and "ray".

## Prepare the environment
We recommend you to use [Anaconda](https://www.anaconda.com/distribution/#linux) to prepare the environment, especially if you want to run on a yarn cluster (yarn-client mode only).
```
conda create -n bigdl python=3.7  # "bigdl" is conda environment name, you can use any name you like.
conda activate bigdl
pip install pillow
conda install pytorch torchvision cpuonly -c pytorch  # command for linux
conda install pytorch torchvision -c pytorch  # command for macOS

# For spark backend
pip install bigdl-orca
pip install tqdm  # progress bar

# For ray backend
pip install bigdl-orca[ray]
pip install tqdm  # progress bar
```

## Prepare Dataset
By default dataset will be auto-downloaded on local mode and yarn-client mode.
If your yarn nodes can't access internet, run the `prepare_dataset.sh` to prepare dataset automatically.
```
bash prepare_dataset.sh
```
After running the script, you will see  **dataset (for local mode use)** and archive **dataset.zip (for yarn mode use)** in the current directory.

## Run example
You can run this example on local mode (default) and yarn-client mode.

- Run with Spark Local mode:
```bash
python super_resolution.py --cluster_mode local
```

- Run with Yarn-Client mode:
```bash
python super_resolution.py --cluster_mode yarn
```

You can run this example with spark backend (default) or ray backend. 

- Run with spark backend:
```bash
python super_resolution.py --backend spark
```

- Run with ray backend:
```bash
python super_resolution.py --backend ray
```

**Options**
* `--upscale_factor` The upscale factor of super resolution. Default is 3.
* `--batch_size` The number of samples per gradient update. Default is 64.
* `--test_batch_size` The number of samples per batch validate. Default is 10.
* `--lr` Learning Rate. Default is 0.01.
* `--epochs` The number of epochs to train for. Default is 2.
* `--cluster_mode` The mode of spark cluster. Either "local" or "yarn". Default is "local".
* `--backend` The backend of PyTorch Estimator. Either "ray" or "spark. Default is "spark".
* `--data_dir` The path of datesets. Default is "./dataset".

## Results

**For "ray" and "spark" backend**

You can find the result for training as follows:
```
===> Epoch 1 Complete: Avg. Loss: 9.2172
```
You can find the result for validation as follows:
```
===> Validation Complete: Avg. PSNR: 11.8249 dB, Avg. Loss: 0.0657
```