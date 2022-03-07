## cnn_transfer_learning_covid

# Description
The objective of this project is to build a convolutional neural network to do diagnosis of COVID pneumonia.
The presence of pneumonia is one of the most important factors that defines prognosis of COVID patients and determining its presence is key to determine the appropriate course of action.
The CNN is implemented usign PyTorch and is based on ResNet-18, the structure of the original network which did clissification between a 1000 classes is modified to do binary classification between CT images with pneumonia and healthy images.
The CNN is first trained using transfer learning and compared to a model trained from scratch. 
The pretrained model was trained on ImageNet and only the last 2 layers are trained on the training data set which includes approximately a 1000 images of each class.
CLass 0 was defined as the normal images and Class 1 as the ones with COVID.

# Results
The pretrained model achieved a sensitivity of 0.96 for the images with COVID and 0.935 for the normal ones (this is specificity as in the medical literature.).
It had a precision of 0.936 for the images with pneumonia and 0.959 for the normal class (this is positive predictive value and negative predictive value respectively).
The model trained from scratch had a sensitivity of 0.96 for the images with COVID and 0.895 for the normal ones.
It had a precision of 0.9014 for the images with pneumonia and 0.957 for the normal class.


# Comments
As it can be seen, the results between the two models do not differ much. The accuracy, sensitivity and precision on the test set are higher than 0.9 for both of them. Transfer learning does not seem to have a great contribution to the model. There are some reasons that could explain this. First, ResNet-18 was pretrained on ImageNet, which is a databased not based on CT images. Therefore, the image patterns might not be similar to the ones in the current dataset (). Secondly, although transfer learning can be useful when the model is pretrained on a larger dataset and then optimized in the smaller dataset, that does not seem to be the case here. The current dataset has around a 1000 samples in both classes, which does not seem very small and might be enough to train the model from scratch. On the other hand, transfer learning did reduce the computational cost of training the model, mainly because less parameters had to be trained in every epoch. For these reasons, transfer learning might not bring a lot of benefits in this case. Besides the decrease in the computational cost there was only a slight change in the model overall performance.

It is interesting to look at the visualization of the kernels in the first convolutional layer, where the the weights of the pretrained model have some geometrical paterns that seem object oriented as it was trained on ImageNet, on the other side the ones from the second model have no strong recongizable patterns.
Looking at the gradient of loss with respect to input also gives some insight. In the model trained from scratch the parts of the image which correspond to the lungs seem to have more influence. They are more strongly highlited in the gradient of the loss plot.

There are some challenges that would arise if this model was to be implemented in a setting with real patients to aid in the diagnosis of pneumonia. First, as with any other diagnostic method some of the metrics that are closely related with the prevalence of the condition would change. This is the case of the precision (or positive and negative predictive values).  In the sample used for this project the prevalence was around 50% since both classes had almost the same amount of images, but the prevalence of respiratory diseases usually changes around the year and is expected to do even more so during a pandemic.

Secondly, the classification in this model is based on a single computed tomography image whereas in real practice a whole CT scan can include easily more than a 100 images of one patient. If the model was applied on all of those images and the diagnosis was based on the detection of COVID in at least one of them, it can be assumed that the false positive rate would rise (or what is the same the precision for the diagnosis would decrese, the positive predictive value), as it would depend on the risk of all the images together. This could prove to be particularly troublesome during a pandemic when it is not desieable to spend part of the limited resources on healthy patients.

In the presented sample the are only two classes but in a real population there would be at least a third one, which is patients with pnuemonia but not caused by COVID. That is why it would be interesting to build a model using this third class.

One possible application of this model could be to pair it with the expertise of the person doing the diagnosis, where the model would only be used on those images requested by the user. In this case the risk of false positives would decreased and it could be helpful in cases when there is doubt. Visualizing the gradient of loss with respect to the input could help understand if the model is giving relevance to the suspected parts of the image. However it would be prudent to consider the appearence of selection bias in this case, since this "suspicious" images could be poorly represented in the original sample that was used to train the model.

Using this kind of technologies to aid in diagnosis could be advantageous, since it is a well known fact that not all populations are equal and they also change in time, hence, the ability to retrain the model using the population of each center or to retrain it to adapt to new epidemiologic settings could be crucial.
