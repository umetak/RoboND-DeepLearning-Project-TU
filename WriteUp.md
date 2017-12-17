
[arch]: ./arch.png
[loss]: ./loss.png
[target]: ./target.png

## DEEP LEARNING - Follow Me Project ##

In this project, We train a deep neural network to identify and track a target in simulation. We use a fully convolutional neural network to distinguish a target from an image. It makes a drone to keep track the target. My result finally obtained 0.4078 using the Intersection over Union (IoU) metric.

## Network Architecture ##

The network is a simple encoder-decoder network. The encoder is used for extracting image features at different scales from an input images. The decoder is used for reconstructing an output image based on the features which made by the encoder. This network can perform a pixel-wise segmentation task by training data set with segmentation labeled images.
My network has three encoders and three decoders. Each encoder has 3x3 kernels with a stride of 2 followed by the batch norm. Each decoder upsamples the vector by a factor of 2, so the overview of the network shows symmetric shape. To connect the encoder and decoder, the network uses a 1x1 convolution with 512 filters. This operation keeps spatial information of an input image that a fully connection layer is not able to do. The diagram of architecture that I finally applies is shown below.

![arch]

## Techniques and concepts in network layers ##

 As we can see in the diagram above, the 1x1 convolution is used for connecting the encoder and decoder in the middle of network. It helps in reducing the dimensionality of the layer. A fully-connected layer of the same size would result in the same number of features. In addition, replacement of fully-connected layers with convolutional layers has another advantage during inference that the trained network can be fed  images of any size.

## The advantage of encoding/decoding images by this network architecture ##

In the Encoder-Decoder networks, the encoder downsamples an input image to generate a high dimensional feature vector. As a result, the input has been compressed to this vector. Otherwise, the decoder upscales this vector to capture the global context of the scene, then generate a semantic segmentation mask as the output. The advantage of this network architecture is it can perform end-to-end joint learning of semantics and location at the same time. There is another technique applied in the network, so called the skip connections. They connect some non-adjacent layers together. The idea is that during learning the next layer will learn the concepts of the previous layer plus the input of that previous layer. This would work better than just learn a concept without a reference that was used to learn that concept.


## Network Parameters ##

I figured out the good parameters through some trial and error.

Epoch: It is determined by how much iterations is enough to converge the network loss to zero

Learning Rate: I chose 0.001 because it seemed faster than 0.0001 and more stable than 0.01

Batch Size: 32 as constrained by GPU memory

My network obtained the loss curves shown below. The training loss keeps going down along with epochs. The validation loss is not lager than the train loss at the final epoch, so it is expected that the network doesn't get overfitting.

![loss]

The sample result is below, the input image (left), ground truth labeled with colors (middle), and our predicted labels (right).
The target is highlighted in blue and other people in green. My network is able to perform segmentation to identify the region of these objects of interest.

![target]

##  Does it work for other objects?  ##

The FCN network in this project would be applied to other segmentation tasks such as for cars, pets, etc. In order to achieve that, we have to collect huge training data for target objects along with labeled ground truth. Then, we should train the network again from the scratch by feeding that data with new classes(labels).   

## Summary ##

The drone can keep track the target in the simulation by using deep learning based segmentation technique. The segmentation performs the score of 0.4078 using the Intersection over Union (IoU) metric. But the score still looks quite low than I expected. In order to improve the performance, it should be considered to collect more data or implement argumentation. In general, the deep learning based system relies much on the size of data, quality of data. It is considered that huge data gives the higher accuracy and robustness in this task even if the same network architecture is applied.      
