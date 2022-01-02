# Autoencoder clustering
Kmeans clustering using features generated from auto encoder



# 1. Objective:
	This project has two main objectives Objective of this project (for this checkpoint) is to implement k-means clustering algorithm from scratch and evaluate the quality of clusters using average silhouette score and Dunn’s Index. In part - 2 we try to build a auto encoder where in we extract the encoded representation of the image to cluster the image data.

# 2. Data Set:
	Data set used for this project is CIFAR-10, which has a train data set of 50000 images and test data of 10000 images with 10 target classes. The dimensions of the images is 32 * 32, with all channels( RGB). For the purpose of k-means clustering, we only use the test data set with 10000 images.

# 3.   Pre-Processing:
	Since we are doing clustering, we perform the following pre-processing steps.
	1. Convert the coloured image to gray scale.
	2. Flatten the numpy arrays to shape 1024.
	3. Normalise the arrays by dividing every intensity value by 255.

# 4.   Clustering:
	Clustering is done using the following steps.

    1. Initialise the number of clusters k.
    2. Initialise cluster centres randomly.
    3. Calculate distance of each point to every cluster centres.
    4. Assign the point to the shortest distance cluster.
    5. Re calculate the cluster centres.
    6. Repeat from step III.Stop after there is no change in the clusters.
	
	Since we know that test data set has 10 target classes. Initialising the k to 10 and performing the clustering operation.





# 5. Evaluation of cluster quality is done using 2 metrics.

	1. Average silhouette score:
	The Silhouette Coefficient is calculated using the mean intra-cluster distance (a) and the mean nearest-cluster distance (b) for each sample. The Silhouette Coefficient for a sample is (b - a) / max(a, b).

    Average of all these scores is Average Silhouette score.

    For 10 clusters the silhouette score is 0.054.

    2. 	Dunn’s Index:
	Dunn’s index is the ration between the smallest distance between and observations not in the same cluster to the largest intra cluster distance.

    ![dunns](/assets/dunns.png)

    For 10 clusters the Dunn’s index value is 0.0899.


# 6. Auto encoders:
	Auto encoders are a class of models where we encode the input data into different features, such that these encoded features can be used to reconstruct the original input with minimum errors. 

    ![architecture](/assets/architecture.png)























	


For this task the model architecture used is depicted in the above screenshot. 

The layers does the following tasks.

Input layer accept the input as it is which is 32 * 32 image.
First Convolution layer has 3 filters, with padding “same” which keeps the output image shape same, irrespective of the size of the filter. 
Max pooling layer with filter size 2, picks the max pixel value in  2 * 2 area of the convoluted image. This reduces the size of the image by half.
The next convolution and max pooling layer perform the same function. The final output of max pooling layer is  an 8 * 8 image. This is where the encoder part ends.
The next layers signify the operations done on the image in reverse order, Where instead of max pooling upsampling is used and in the final layer, the input image is re constructed.

The Original image and the image reconstructed are compared using mean squared error as loss function. Our goal of training is to reduce mean squared error to get good reconstructed images, which indirectly signify good encoded representation.


Training the model on train of CIFAR for 10 epochs with a batch size of 32, results in the following loss values. Loss values are quite stable by epoch 8 and there is no further decrease in loss.


![epochs](/assets/epochs.png)















In the next step using only the encoder part of the model to get representations of the training set. Few examples of the original and encoded images are as shown below.
![img1](/assets/img1.png)
![img2](/assets/img2.png)
![img3](/assets/img3.png)
Using the encoded representation on the clustering algorithm described in 4. The ASC score is found to be 0.1040 and Dunn’s index is 0.02. with 10 clusters.