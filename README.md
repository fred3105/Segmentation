# Segmentation - Legrand Frédéric
Image segmentation ECL23 with Liming Chen

# Questions
## Max pooling

It's possible to use convolution as this operation will also reduce the size of the layer and result in a similar effect.

Using strided convolutions instead of max pooling can be beneficial as it reduces the amount of operations and parameters in the model. 
However, max pooling helps prevent overfitting by providing a form of translation invariance. So replacing max pooling with strided convolutions may lead to worse performance due to overfitting, unless additional regularization is used.

## Skip connections

The skip connections allow the decoder network to utilize features from the encoder network. 
This helps the model have both a global broader view of the image and its context as well as a precise very localised knowledge. 
The model can thus reconstruct finer details in the output segmentation mask. 
Without the skip connections, the decoder would have limited knowledge of larger features extracted by the encoder, so the output segmentation would be less precise. 
The skip connections could in theory use other operations like addition or multiplication, but concatenation seems to produce the best results as it helps the network have both a localised and a general view of the image.

## FCN and Auto-encoder

A regular FCN keeps the spatial dimensions constant through the whole network. 
This means the receptive field of the network is limited, so the context captured is smaller. 
The encoder-decoder structure allows aggregating larger context by gradually reducing spatial dimensions, enabling the model to incorporate more global information. 
The expanding path then helps refine the localization. 
So the encoder-decoder structure is better suited for dense prediction tasks like segmentation because it can "Zoom in" to smaller details of the image while retaining a more global sense.

## Threshold for inference

In our case, precision is defined as the number of pixels that are correctly predicted as salt over the total number of pixels predicted as salt.

Recall is the number of pixels that are correctly predicted as salt over the total number of pixels that are really salt.

To optimize both precision and recall, we could make use of the ROC curve and more specifically the ROC AUC that gives us a number between 0 and 1 that indicates the performance of the model with respect to the inference threshold. 
The ROC AUC number gives us a direct way to compare inference threshold values and choose the best one.

A "perfect" parameterization would result in a ROC AUC value of 1.