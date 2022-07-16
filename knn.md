---
layout: home2
permalink: /knn/
title: K-Nearest Neighbors Classifier Algorithm
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

The K nearest neighbors (KNN) classifier is a highly useful and popular tool for applications of data mining. KNN is a relatively simple, non-probabalistic supervised machine learning algorithm that classifies vector data points, each of which stores data and a classification, by computing the distance between an input vector and all other vectors. Upon examining all vectors in the dataset, KNN determines the plurality class of the K nearest vectors and assigns this ideal classification to the input vector. In KNN, distance can be measured using various $L^\mathcal{p}$ norms such as the Manhattan distance formula, the Chebyshev distance formula, and most commonly, the Euclidean distance formula which is given by: \\[ d(p,q) = \sqrt{\sum_{i=1}^n (q_i - p_i)^2} \\]
Therefore, using C++ and the Eigen linear algebra library, we can write:
```cpp
#include <cmath>
#include "eigen3/Eigen/Dense"
#include "eigen3/Eigen/StdVector"

double EuclideanDistance(Eigen::VectorXd a, Eigen::VectorXd b, int length)
{

    /* Returns the Euclidean Distance between two vectors of the same feature length, a and b. */

    double sum = 0;

    for(int i = 0; i < length; i++)
    {
        sum += pow(a.coeff(i) - b.coeff(i),2);
    }

    return sqrt(sum);
}
```
Then, distances between one vector and all other vectors in dataset can be computed with:
```cpp
#include <vector>
#include <algorithm>
#include "eigen3/Eigen/Dense"
#include "eigen3/Eigen/StdVector"

std::vector<double> distances(Eigen::VectorXd vector, int vector_length, Eigen::MatrixXd X, int X_size, double (*distance_function) (Eigen::VectorXd a, Eigen::VectorXd b, int length))
{

    /* Returns the distances for one input vector to every point in matrix X vector where the last entry in each vector is its classification. */

    std::vector<double>distances_list = { };

    for(int i = 0; i < X_size; i++)
    {
        Eigen::VectorXd x = X.row(i);
        distances_list.push_back(distance_function(vector.tail(vector_length-1),x.tail(vector_length-1),vector_length-1));
    }

    return distances_list;
}
```
Next, plurality class (the most common classification in a vector, e.g. the plurality class of $x = 1, 1, 2$ is $1$), can be written with:
```cpp
int plurality_class(std::vector<int> &classifications)
{

    /* Returns the most common classification in vector. */

    if (classifications.empty())
    {
        return -1;
    }

    sort(classifications.begin(), classifications.end());

    auto last = classifications.front();
    auto most_frequent = classifications.front();

    int max_frequency = 0;
    int current_frequency = 0;

    for (const auto &i : classifications)
    {
        if (i == last)
        {
            ++current_frequency;
        } else {
            if (current_frequency > max_frequency)
            {
                max_frequency = current_frequency;
                most_frequent = last;
            }

            last = i;
            current_frequency = 1;
        }
    }

    if (current_frequency > max_frequency)
    {
        max_frequency = current_frequency;
        most_frequent = last;
    }

    return most_frequent;
}
```
Finally, using the above functions, KNN can be implemented with:
```cpp
std::vector<int> knn(Eigen::MatrixXd M1, int M1_size, Eigen::MatrixXd M2, int M2_size, int K, double (*distance_function) (Eigen::VectorXd a, Eigen::VectorXd b, int length))
{

    /* Returns classifications of all instances in one dataset, M1, using another dataset, M2. */

    std::vector<int> predictions = { };

    for(int i = 0; i < M1_size; i++) // Computes the distances from one M1 row, x, to all vectors in M2 and store the classifications of the K smallest vectors.
    {
        std::vector<int> k_smallest_classifications = { };

        Eigen::VectorXd x = M1.row(i);
        std::vector<double> dists = distances(x,x.size()-1,M2,M2_size,*&distance_function);

      	std::vector<int> k_smallest = argpartition(dists, K); // Find the indicies of the K smallest distances
      	double k_smallest_arr[k_smallest.size()];
        std::copy(k_smallest.begin(), k_smallest.end(), k_smallest_arr);

        std::vector<int> labels = { }; // Create list of labels of K shortest distances.

        for(int j = 0; j < K; j++)
        {
            Eigen::VectorXd k_closest_vector = M2.row(k_smallest_arr[j]);
            labels.push_back(k_closest_vector.coeff(0));
        }

        int classification = plurality_class(labels); // Find the most commonly occuring label in list
        predictions.push_back(classification);
    }

    return predictions;
}
```
Next, in order to draw more accurate predictions than those one might come to by using an arbitrary K value, running N-fold cross-validation on different values of K and comparing the results in can be used to compute the K with the smallest error value. This will be the K that best fits the data. In other words, for each K value from KNN, one should split the data into N folds of data. Then, for N runs, assign one of these folds to be the test/validation data and the rest of the N-1 folds to be the training data, alternating the test/validation data on each run so that every fold will serve as the validation data once. For each of these tests, run KNN on all of the training folds, using the validation data as input. Then calculate the misclassification rate for each train fold output on the sealed validation fold. Averaging these misclassification rates give the misclassification rate for the K value tested in this run of N-fold cross-validation. Miscalculation rate can be computed with:
\\[ \text{Misclassification Rate} = \frac{1}{N} \sum_{n} I (\hat{t}_n \neq t_n) \\]
where $\hat{t}_n$ represents the classifier's output for a training point $n$, and $I(A)$ returns $1$ if $A$ is true and $0$ if $A$ is false. Whichever K value has the least average misclassification rate across N folds is the ideal K. This K is the one that should be used when adding a new vector to assign a classification. \
\
As such, to compute the misclassification rate, we can write:
```cpp
#include <vector>
#include <algorithm>

double misclassification_rate(std::vector<int> labels, std::vector<int> ground_truth_labels)
{

  /* Takes an array of labels and an array of ground truth labels and calculates the misclassification rate. */

  int incorrect = 0;

  std::vector<int>::iterator labels_it = labels.begin();
  std::vector<int>::iterator ground_truth_labels_it = ground_truth_labels.begin();

  for(; labels_it != labels.end() && ground_truth_labels_it != ground_truth_labels.end(); ++labels_it, ++ground_truth_labels_it)
  {
      if(*labels_it != *ground_truth_labels_it)
      {
        incorrect += 1;
      }
  }

  return (double) incorrect / labels.size();
}
```
Next, to split the dataset into $K$ folds, we can utilize the function:
```cpp
std::vector<Eigen::Matrix<double,Eigen::Dynamic,Eigen::Dynamic>, Eigen::aligned_allocator<Eigen::Matrix<double,Eigen::Dynamic,Eigen::Dynamic> > > split(Eigen::MatrixXd dataset, int K)
{

  	/* Returns shuffled list of K Eigen::MatrixXd folds, split from input dataset. */

	int place = 0;

	//create temporary std::vector to hold rows of dataset
	std::vector<Eigen::Vector<double,Eigen::Dynamic>, Eigen::aligned_allocator<Eigen::Vector<double,Eigen::Dynamic> > > temp;
	for(int i = 0; i < dataset.rows(); i++)
	{
		temp.push_back(dataset.row(i));
	}

	// shuffle std::vector

	auto random_number_generator = std::default_random_engine {};
	std::shuffle(std::begin(temp), std::end(temp), random_number_generator);

	// write shuffled rows into a new shuffled matrix

	Eigen::MatrixXd shuffled(dataset.rows(),dataset.cols());
	for( auto v : temp )
	{
		shuffled.row(place++) = v;
	}

	place = 0;

	std::vector<Eigen::Matrix<double,Eigen::Dynamic,Eigen::Dynamic>, Eigen::aligned_allocator<Eigen::Matrix<double,Eigen::Dynamic,Eigen::Dynamic> > > list; // does not like not regular ints as arguments, e.g. row len and fold len

	for(int i = 0; i < K; i++)
	{
		Eigen::MatrixXd fold(dataset.rows() / K,dataset.cols());

		for(int j = 0; j < dataset.rows() / K; j++)
		{
			Eigen::VectorXd x = shuffled.row(place++);
			fold.row(j) = x;
		}

		list.push_back(fold);
	}

	return list;
}
```
Finally to run $K$ fold cross validation, we can write:
```cpp
double kfcv(Eigen::MatrixXd dataset, int K, std::vector<int> (*classifier) (Eigen::MatrixXd train, int train_size, Eigen::MatrixXd validation, int validation_size, int optimal_parameter, double (*distance_function) (Eigen::VectorXd a, Eigen::VectorXd b, int length)), int param, double (*distance_function) (Eigen::VectorXd a, Eigen::VectorXd b, int length))
{
	/* Returns std::vector of error statistics from run of cross validation using given error function and classification function. */

	std::vector<Eigen::Matrix<double,Eigen::Dynamic,Eigen::Dynamic>, Eigen::aligned_allocator<Eigen::Matrix<double,Eigen::Dynamic,Eigen::Dynamic> > > folds = split(dataset,K);

	double total_error = 0;

	for(int i = 0; i < K; i++)
	{
		int length = dataset.rows() / K;
		int train_place = 0;
		int validation_place = 0;

		Eigen::MatrixXd validation(length * 1,dataset.cols());
		Eigen::MatrixXd train(length * (K-1),dataset.cols());

		int idx = 0;

		for(auto v : folds)
		{
			if(idx != i)
			{
				for(int j = 0; j < length; j++)
				{
					train.row(train_place++) = v.row(j);
				}
			}

			if(idx == i)
			{
				for(int j = 0; j < length; j++)
				{
					validation.row(validation_place++) = v.row(j);
				}
			}

			idx = idx + 1;
		}

		std::vector<int> truth_labels;

		idx = 0;

		for(int i = 0; i < validation.rows(); i++)
		{
			truth_labels.push_back(validation.coeff(i,0));
		}

		std::vector<int> predictions = classifier(validation,validation.rows(),train,train.rows(),param,*&distance_function); // change to distance function parameter

		double error = misclassification_rate(predictions,truth_labels);
		total_error += error;
	}

	return (double) total_error / K;
}
```
<!--KNN is commonly applied in many settings, especially in medical research. For instance, KNN can be used to classify malignant cancer cells based on cell data including cell measurements in a dataset available freely from the UCI Maching Learning Repository. \
\-->
Using this code on the 1936 Iris dataset which contains 150 flowers classified by species and their respective sepal and petal measurements, for $K = 1$ to $K = 135$, cross validation computed the following misclassification rates: \
\
<img src="/images/misclassification_rate_across_folds_iris.png" alt="/images/misclassification_rate_across_folds_iris.png"/> \
\
choosing $K = 19$ as the optimal value for minimizing the misclassification rate across folds. \
\
Full code for implementation available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/knn-cpp-241">https://github.com/nathanenglehart/knn-cpp-241</a>.

### References

Fisher, R.A. (1988). UCI Machine Learning Repository <a style="color: #f56a6a; !important" href="https://archive.ics.uci.edu/ml/datasets/iris">https://archive.ics.uci.edu/ml/datasets/iris</a>. Irvine, CA: University of California, School of Information and Computer Science.
