---
layout: home2
permalink: /naive-bayes/
title: Naive Bayesian Classifier Algorithm
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---
The Naive Bayesian Classifier Algorithm is a family of probabalistic supervised machine learning algorithms that assumes each feature is independent of other features inside a feature vector. 
<!--Each implementation is based on Bayes Rule, which is given by:
\\[  P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)} \\] -->
### MLE Naive Bayes
One implementation of Naive Bayes uses maximum likelihood estimation (MLE). MLE Naive Bayes is useful for classifying vector rows with ordinal data as features. Its equation is given by:
\\[ P(y,x_1 … x_d) = P(y) \prod^{d}_{j=1} P_j (x_j|y) \\]
Breaking down the algorithm more specifically, using the training dataset we first calculate the frequency of each feature in the train dataset in relation to each classification in the dataset. These frequencies allow us to calculate the prior probability for each feature or $q(y)$. We can then calculate the class conditional probability of the test vector belonging to a certain class by taking the product sum of the corresponding probabilities for each feature in the test set belonging to the classification label y. We then apply Bayes' rule by multiplying the product sum by the overall probability of the classification label occurring in the test set $q(y)$ to get our posterior probability. We then normalize these probabilities by dividing by the number of potential classes. The Naive Bayes classifier thus classifies each test vector using the label y which returned the highest probability. \
\
Using C++ and leaving out functions for simplicity, we can write:
```c++
std::vector<int> mle_naive_bayes_classifier(Eigen::MatrixXd validation, int validation_size, Eigen::MatrixXd training, int training_size, int length, bool verbose)
{

  /* Calculates the classification probabilities for each row in dataset and puts their predicted classification in a list. */

  if(verbose)
  {
  	printf("mode 2: mle\n");
  }

  std::vector<int> predictions;

  // calculate class frequencies, then
  // calculate overall classification probabilities i.e. compute q_j(y)

  std::vector<Eigen::MatrixXd> class_matricies_list = matricies_by_classification(training, training_size, length);

  std::vector<int> unique_classifications;
  std::vector<double> unique_classifications_count; 
  std::vector<double> unique_classifications_probabilities;
  
  int c = 0;
  
  for(auto v : class_matricies_list)
  {
  	unique_classifications.push_back(c++);
  }
  
  double total_count = 0.0;

  for(int i = 0; i < c; i++)
  {
	double class_count = (double) class_matricies_list[i].rows();
	unique_classifications_count.push_back(class_count);
	total_count += class_count;
  }		

  for(auto v : unique_classifications_count)
  {
  	double unique_classification_probability = 0.0;
	unique_classification_probability =  (double)v /(double)total_count;
  	unique_classifications_probabilities.push_back(unique_classification_probability);
  }

  // record the features of each vector corresponding to classification

  std::vector<Eigen::MatrixXd> list = matricies_by_classification(training, training_size, length);

  std::map<int,std::vector<std::map<int,double>>> dict;

  int current_class = 0;

  for(auto class_matrix : list)
  {
  	
	// each iteration corresponds to one classifications individual matrix

  	std::vector<std::map<int,double>> entry; 
	
	for(int i = 1; i < class_matrix.cols(); i++)
	{

		std::map<int,double> col_labels;

		Eigen::VectorXd feature_column = class_matrix.col(i);

		int max_label = get_max_feature_label(feature_column);
		int label_counts [max_label];

		int curr_len = 0;
		for(int j = 0; j <= max_label; j++)
		{
			label_counts[j] = 0;
			curr_len++;
		}
				
		int feature_column_length = len(feature_column);

		for(int j = 0; j < feature_column_length; j++)
		{
			int label = feature_column[j];
			label_counts[label] += 1;
		}


		for(int j = 1; j < max_label+1; j++)
		{
			col_labels[j] = (double) label_counts[j] / feature_column_length;
		}

		entry.push_back(col_labels);
	}

  	dict[current_class++] = entry;
  }
  
  int mat_num = 0;

  // now we can lookup: dict[y][x_n][feature] = p_j(x|y)
  // now compute the probability of each input vector belonging to a classification 

  for(int i = 0; i < validation_size; i++)
  {
  	Eigen::VectorXd row = validation.row(i);

	std::vector<double> probabilities;
	
	for(int j = 0; j < c; j++)
	{
		double p_y = unique_classifications_probabilities[j];
		
		double p_yx = p_y;

		for(int k = 1; k < len(row); k++)
		{
			int label = row[k];
			
			double p_xy = dict[j][k-1][label];
			p_yx *= p_xy; 
		}
		
		probabilities.push_back(p_yx);
	}

	std::vector<double> normalized_probabilities = normalize(probabilities); 

 	 // assign classification using argmax probability
  
	int pred = get_argmax(normalized_probabilities,c);
	predictions.push_back(pred);

  }

  return predictions;
}
```
On Unix based operating systems such as any distribution of GNU + Linux or Mac OS, the full implementation can be loaded with
```bash
git clone https://github.com/nathanenglehart/naive-bayes-cpp-241
cd naive-bayes-cpp-241
make
```
A help menu is available with:
```bash
./naive-bayes-cli -h
```
Now, using a synthetic dataset classifying rows as 0 or 1 using three feature vectors (train available [here](https://nathanenglehart.github.io/data/synth_cat_train-3.csv); test available [here](https://nathanenglehart.github.io/data/synth_cat_test-3.csv)), we can run:
```bash
./naive-bayes-cli data/bc/breast-cancer-wisconsin_cleaned.csv data/bc/cancer_test_cleaned.csv -m -v
```
Which, using 10 fold cross validation, returns an error rate of 5.2% error rate. \
\
Now, to graph the results we can first use bash and regex to save the results to a csv:
```bash
./naive-bayes-cli synth_cat_train-3.csv synth_cat_test-3.csv -m | sed -E 's/.*(.{1})$/\1/' | sed '$d' | awk '{print}' ORS=', ' | sed '$ s/..$//' > synth_cat_preds-3.csv
```
Then, using a python script, we can write:
```
#!/usr/bin/env python3

import numpy as np
import pandas as pd
from matplotlib import pyplot as plt

results = pd.read_csv("synth_cat_preds-3.csv", header=None, sep=",")
results = results.iloc[0]

data = pd.read_csv("synth_cat_test-3.csv", header=None) 
	
x_1 = np.array(data.iloc[:,1])
x_2 = np.array(data.iloc[:,2])
	
categories = results
colormap=np.array(['tab:blue','tab:orange'])

plt.scatter(x_1, x_2, s=8, c=colormap[categories], alpha=0.5)
plt.xlabel('x_1') 
plt.ylabel('x_2')
plt.show()
```
Which graphs the first categorical feature vector $x_1$ against the second categorical feature vector $x_2$, as such:

<p align="center"><img src="/images/nb-fig-x1x2.png" alt="/images/nb-fig-x1x2.png"/></p> 

Similarly, we can modify the script to graph $x_2$ against $x_3$:

<p align="center"><img src="/images/nb-fig-x2x3.png" alt="/images/nb-fig-x2x3.png"/></p> 

### Gaussian Naive Bayes
Another implementation of the Naive Bayes algorithm is Gaussian Naive Bayes. It is highly useful for classifying vector rows with continuous feature variables. The equation for Gaussian Naive Bayes is given by:
\\[ P(x_i|y) = \frac{1}{\sqrt{2\pi\sigma^2_y}}exp\bigg(- \frac{(x_i - \mu_y)^2}{2\sigma^2_y} \bigg) \\]
where $\sigma$ represents standard deviation and $\mu$ represents mean. \
\
Gaussian Naive Bayes assumes that the probabilities associated with each class are distributed using a normal distribution. Similar to a maximum likelihood estimator of Naive Bayes, we calculate the prior density using the overall frequency of each classification in our training dataset, however, the difference lies in how we calculate our class-conditional density. We calculate the class-conditional density by drawing from a Gaussian distribution. We are then able to predict the classification in a similar way to our maximum likelihood algorithm, by multiplying our class conditional probability by our prior for each classification. We then normalize our probabilities, using the total number of classifications, and classify each test vector using the highest probability. \
\
Again, using C++ and leaving out some functions for simplicity, we can write:
```c++
std::map<int, double> calculate_classification_probabilities(std::map<int, std::vector<std::vector<double>>> summaries, Eigen::VectorXd row, int size, bool verbose)
{

  /* Calculates the classification probabilities for a single vector with P(class = y | x_1, ..., x_2) = P(x_1 | class = y) * ... * P(x_n | class = y) * P(class = y) */

  std::map<int, double> probabilities;
  std::map<int, std::vector<std::vector<double>>>::iterator it;
  int classification_value = 0;

  if(verbose == true)
  {
    std::cout << "Row " << verbose_vector_count++ << ": [ ";
    for(auto v : row)
    {
      std::cout << v << " ";
    }
    std::cout << "]\n";

  }

  for(it=summaries.begin(); it != summaries.end(); ++it)
  {

    std::vector<std::vector<double>> entry = it->second;
    probabilities[classification_value] = double_vector_list_lookup(entry,0,2) / (size); // P(class = y)

    for(int i = 1; i < row.size(); i++) // P(class = y | x_1, ..., x_n)
    {
      double mean = double_vector_list_lookup(entry,i,0);
      double standard_deviation = double_vector_list_lookup(entry,i,1);
      double x = get_eigen_index(row,i);
      probabilities[classification_value] *= gaussian_pdf(x,mean,standard_deviation); 
    }

    if(verbose == true)
    {
      std::cout << "Class: " << classification_value << " Probability: " << probabilities[classification_value] << "\n";
    }

    classification_value++;
  }

  if(verbose == true)
  {
    std::cout << "\n";
  }

  return probabilities;
}

int predict(std::map<int, std::vector<std::vector<double>>> summaries, Eigen::VectorXd row, int size, bool verbose)
{

  /* Returns classification prediction for Gaussian NB. */

  std::map<int, double> probabilities = calculate_classification_probabilities(summaries, row, size, verbose);
  std::map<int, double>::iterator it;

  int best_label = -1;
  double best_probability = -1;

  for(it=probabilities.begin(); it != probabilities.end(); ++it)
  {
    if(best_label == -1 || it->second > best_probability)
    {
      best_probability = it->second;
      best_label = it->first;
    }
  }

  return best_label;
}

std::vector<int> gaussian_naive_bayes_classifier(Eigen::MatrixXd validation, int validation_size, Eigen::MatrixXd training, int training_size, int length, bool verbose)
{

  /* Calculates the classification probabilities for each row in dataset and puts their predicted classification in a list. */

  std::map<int, std::vector<std::vector<double>>> summaries = summarize_by_classification(training, training_size, length);
  std::vector<int> predictions;

  for(int i = 0; i < validation_size; i++)
  {
    int output = predict(summaries, validation.row(i), training_size, verbose);
    predictions.push_back(output);
  }

  return predictions;
}
```
Full code for both implementations available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/naive-bayes-cpp-241">https://github.com/nathanenglehart/naive-bayes-cpp-241</a>
### References
Barber, David. (2016). Bayesian Reasoning and Machine Learning. Cambridge University Press. \
\
Kuhn, Max and Johnson, Kjell. (2013). Applied Predictive Modeling, 2013. Springer.
