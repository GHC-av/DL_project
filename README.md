# Deep Learning Project
## Pneumonia Detection from Chest X-ray Images

Pneumonia is a form of acute respiratory infection that affects the lungs and is a leading cause of morbidity and mortality. Chest X-ray represents the initial investigation of choice in most cases. In this paper, two different types of architectures to detect the presence of pneumonia from chest X-ray images are compared. The first model is a shallow convolutional neural network, the second one is based on DenseNet architecture. Both the methods leverage on transfer learning, the models were initially pre-trained on a separate data set collecting chest X-ray images of healthy people and patients infected by Covid-19. The data sets used for training and pre-training are of similar size and cannot be considered as exceptionally large. For this reason, data augmentation was used both during training and pre-training. The results obtained confirm that deep learning and convolutional neural network can be an automated and effective tool in diagnosing pneumonia.

### Methods and Dataset
**Dataset**
This problem is a binary classification one where the inputs are labelled chest X-ray images and the output is one of the two classes: pneumonia and normal (not pneumonia). 
The data set was obtained from [Kaggle](https://www.kaggle.com/paultimothymooney/chest-xray-pneumonia)
and it contains chest X-ray images (anterior-posterior) selected from pediatric patients of the age one to five year old from Guangzhou Women and Children’s Medical Center. As described by the data set provider, all chest radiographs were initially screened for quality control by removing all low quality or unreadable scans. The diagnoses for the images were then graded by two expert physicians before being cleared for training the AI system. To reduce the odds of any grading errors, the evaluation set was also checked by a third expert.

In the original version the validation set is very small (only 16 examples). For this reason, it was decided to use an [alternative version](https://www.kaggle.com/tolgadincer/labeled-chest-xray-images) of the data set where the validation data set has been merged with the train set. The validation data set will be specified during the model training as a hold-out set with size 20% of the training set. This precaution should ensure a more robust model.
The data set used is composed by two main folders: train set and test set. In total there are 5.873 chest X-ray images: 5.249 from the training set and the remaining 624 from the test set. 

Pneumonia images are three times more frequent than normal images. This might suggest that the data set is imbalanced and that we would need to use different class weights during training.

Data augmentation was performed to increase the number of images. In this way our model will have more example to learn from, reducing the chances of setting up a network that learns patterns too specific to the dataset at hand. Data augmentation and normalization are performed at the same time using a generator from Keras.

Both the CNN from scratch and the DenseNet leverage on transfer learning. The networks were pre-trained for 50 epochs on another set of chest X-ray images from Kaggle (CoronaHack). This data set collects chest X-ray of healthy vs pneumonia (from Covid-19 infection) affected patients. The weights generated after training the model on the CoronaHack dataset were saved in an h5 file and then loaded before compiling the model for training on the target data set. Transfer learning allows using pre-trained weights instead of initializing the parameter randomly. In this way, at time 0 the model is already able to detect relevant patterns for image classification instead of starting from scratch. During training time, the layers were not frozen and the model could update the weights as needed.
Pneumonia can be associated to areas of opacity (seen as white) on chest x-rays images. Intuitively, with transfer learning, the models during pre-training learn the filters/weights that can highlight and spot such opaque areas. 

