
[arch]: ./arch.png
[loss]: ./loss.png
[target]: ./target.png

## DEEP LEARNING - Follow Me Project ##

In this project, We train a deep neural network to identify and track a target in simulation. We use a fully convolutional neural network to distinguish a target from an image. It makes a drone to keep track the target. My result finally obtained 0.4078 using the Intersection over Union (IoU) metric.

## Network Architecture ##

The network is a simple encoder-decoder network. The encoder is used for extracting image features at different scales from an input images. The decoder is used for reconstructing an output image based on the features which made by the encoder. This network can perform a pixel-wise segmentation task by training data set with segmentation labeled images.
My network has three encoders and three decoders. Each encoder has 3x3 kernels with a stride of 2 followed by the batch norm. Each decoder upsamples the vector by a factor of 2, so the overview of the network shows symmetric shape. To connect the encoder and decoder, the net work uses a 1x1 convolution with 512 filters. This operation keeps spatial information of an input image that a fully connection layer is not able to do. The diagram of architecture that I finally applies is shown below.

![arch]

##Network Parameters##
I figured out the good parameters through some trial and error.

Epoch: It is determined by how much iterations is enough to converge the network loss to zero

Learning Rate: I chose 0.001 because it seemed faster than 0.0001 and more stable than 0.01

Batch Size: 32 as constrained by GPU memory

My network obtained the loss curves shown below. The training loss keeps going down along with epochs. The validation loss is not lager than the train loss at the final epoch, so it is expected that the network doesn't get overfitting.

![loss]

The sample result is below, the input image (left), ground truth labeled with colors (middle), and our predicted labels (right).
The target is highlighted in blue and other people in green. My network is able to perform segmentation to identify the region of these objects of interest.

![target]



## Summary ##

The drone can keep track the target in the simulation by using deep learning based segmentation technique. The segmentation performs the score of 0.4078 using the Intersection over Union (IoU) metric. But the score still looks quite low than I expected. In order to improve the performance, it should be considered to collect more data or implement argumentation. In general, the deep learning based system relies much on the size of data, quality of data. It is considered that huge data gives the higher accuracy and robustness in this task even if the same network architecture is applied.      
