# Using the PyTorch DLAMI with AWS Graviton\-based EC2 instances featuring GPUs<a name="tutorial-graviton-pytorch"></a>

The AWS Deep Learning AMI is ready to use with AWS Graviton\-based EC2 instances featuring GPUs, and comes optimized for PyTorch\. The Graviton GPU PyTorch DLAMI includes a Python environment pre\-configured with [PyTorch](http://aws.amazon.com/pytorch), [TorchVision](https://pytorch.org/vision/stable/index.html), and [TorchServe](https://pytorch.org/serve/) for deep learning training and inference use cases\. Check the [release notes](http://aws.amazon.com/releasenotes/deep-learning-ami-graviton-gpu-pytorch-1-10-ubuntu-20-04/) for additional details on the Graviton GPU PyTorch DLAMI\.

**Topics**
+ [Verify PyTorch Python Environment](#tutorial-graviton-pytorch-environment)
+ [Run Training Sample with PyTorch](#tutorial-graviton-pytorch-training)
+ [Run Inference Sample with PyTorch](#tutorial-graviton-pytorch-inference)

## Verify PyTorch Python Environment<a name="tutorial-graviton-pytorch-environment"></a>

Connect to your G5g instance and activate the base Conda environment with the following command:

```
source activate base
```

Your command prompt should indicate that you are working in the base Conda environment, which contains PyTorch, TorchVision, and other libraries\.

```
(base) $
```

Verify the default tool paths of the PyTorch environment:

```
(base) $ which python
/opt/conda/bin/python

(base) $ which pip
/opt/conda/bin/pip

(base) $ which conda
/opt/conda/bin/conda

(base) $ which mamba
/opt/conda/bin/mamba
```

Verify that Torch and TorchVersion are available, check their versions, and test for basic functionality:

```
(base) $ python
Python 3.8.12 | packaged by conda-forge | (default, Oct 12 2021, 23:06:28)
[GCC 9.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch, torchvision
>>> torch.__version__
'1.10.0'
>>> torchvision.__version__
'0.11.1'
>>> v = torch.autograd.Variable(torch.randn(10, 3, 224, 224))
>>> v = torch.autograd.Variable(torch.randn(10, 3, 224, 224)).cuda()
>>> assert isinstance(v, torch.Tensor)
```

## Run Training Sample with PyTorch<a name="tutorial-graviton-pytorch-training"></a>

Run a sample MNIST training job:

```
git clone https://github.com/pytorch/examples.git
cd examples/mnist
python main.py
```

Your output should look similar to the following:

```
...
Train Epoch: 14 [56320/60000 (94%)]    Loss: 0.021424
Train Epoch: 14 [56960/60000 (95%)]    Loss: 0.023695
Train Epoch: 14 [57600/60000 (96%)]    Loss: 0.001973
Train Epoch: 14 [58240/60000 (97%)]    Loss: 0.007121
Train Epoch: 14 [58880/60000 (98%)]    Loss: 0.003717
Train Epoch: 14 [59520/60000 (99%)]    Loss: 0.001729
Test set: Average loss: 0.0275, Accuracy: 9916/10000 (99%)
```

## Run Inference Sample with PyTorch<a name="tutorial-graviton-pytorch-inference"></a>

Use the following commands to download a pre\-trained densenet161 model and run inference using TorchServe:

```
# Set up TorchServe
cd $HOME
git clone https://github.com/pytorch/serve.git
mkdir -p serve/model_store
cd serve

# Download a pre-trained densenet161 model
wget https://download.pytorch.org/models/densenet161-8d451a50.pth >/dev/null

# Save the model using torch-model-archiver
torch-model-archiver --model-name densenet161 \
    --version 1.0 \
    --model-file examples/image_classifier/densenet_161/model.py \
    --serialized-file densenet161-8d451a50.pth \
    --handler image_classifier \
    --extra-files examples/image_classifier/index_to_name.json  \
    --export-path model_store 

# Start the model server
torchserve --start --no-config-snapshots \
    --model-store model_store \
    --models densenet161=densenet161.mar &> torchserve.log

# Wait for the model server to start
sleep 30

# Run a prediction request
curl http://127.0.0.1:8080/predictions/densenet161 -T examples/image_classifier/kitten.jpg
```

Your output should look similar to the following:

```
{
  "tiger_cat": 0.4693363308906555,
  "tabby": 0.4633873701095581,
  "Egyptian_cat": 0.06456123292446136,
  "lynx": 0.0012828150065615773,
  "plastic_bag": 0.00023322898778133094
}
```

Use the following commands to unregister the densenet161 model and stop the server:

```
curl -X DELETE http://localhost:8081/models/densenet161/1.0
torchserve --stop
```

Your output should look similar to the following:

```
{
  "status": "Model \"densenet161\" unregistered"
}
TorchServe has stopped.
```
