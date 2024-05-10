__Project created for the following kaggle competition__: @misc{2el1730-machine-learning-project-january-2024,
    author = {nouzir},
    title = {2EL1730 Machine Learning Project - Jan. 2024},
    publisher = {Kaggle},
    year = {2024},
    url = {https://kaggle.com/competitions/2el1730-machine-learning-project-january-2024}
}. 


__The dataset comes from this conference paper__ : https://openaccess.thecvf.com/content/CVPR2021W/EarthVision/html/Verma_QFabric_Multi-Task_Change_Detection_Dataset_CVPRW_2021_paper.html


# Project Overview

This project focuses on advanced feature engineering and model tuning for a machine learning task. The following sections outline the key steps and decisions made during the project's development.

## Section 1: Feature Engineering

I created new features for this project to enhance the performance of my algorithms. First, I used a 'one-hot encoder' to transform categorical data into numerical form. Columns containing non-numeric values were encoded in this way, creating multiple new features corresponding to the unique values. I also encoded 'empty' values as 'N/A.' Dates that couldn't be used directly were transformed, with the intervals between consecutive dates generating new features.

I then worked with polygon data. Using coordinates, I created the following features: area, perimeter, aspect ratio, and centroid coordinates (with separate columns for x and y). The code to generate these features heavily relied on the Shapely module. I observed a significant increase in algorithm accuracy after implementing these features.

Continuing with this approach, I created new features for the red, green, and blue color channels, calculating the average value over five dates for each color. Each new feature added improved model accuracy. However, adding features for the difference in color values between dates had the opposite effect, reducing accuracy. This could be due to dimensionality, as adding these features increased the feature space by 12 times (4 differences between 5 dates, and this multiplied by the 3 colors). I then tested whether I had encountered the "curse of dimensionality" by comparing model accuracy with and without these new features. The best results came from the features present before adding the last set.

To determine the optimal feature set, I used a trial-and-error approach, observing which combinations yielded the best results for a given model.

## Section 2: Model Tuning and Comparison

I tested several classifier models, including nearest neighbors, SVM, boosting, and random forests. The nearest neighbors algorithm was quickly dismissed due to poor results. I then tried using an SVM, but the data's non-linear nature prevented the algorithm from finding a separating hyperplane, causing the code to run indefinitely. 

Next, I implemented a random forest model using sklearn. The initial results, with about 81% accuracy, were promising, prompting further parameter tuning. This led to an accuracy score of about 89% on Kaggle, a substantial improvement. However, I aimed for the top-ranking scores, which were around 97%.

I obtained the best parameters through grid search, calculating the f1_score for each combination and choosing the setup with the highest score. I then turned to gradient boosting, which relies on decision trees and benefits from gradient optimization, making it effective for large, complex datasets. This proved true for my data, as the feature space was extensive. The gradient boosting model delivered a better f1_score and faster execution times.

To avoid overfitting, I employed early stopping and ensemble pruning techniques. These strategies helped control model complexity and prevented excessive fitting to the training data. The final choice of models was based on their f1_score, with preference given to those delivering the best results. After thorough parameter tuning and cross-validation, I selected the best-performing model and validated it with Kaggle scores to ensure generalization.

Ultimately, my best Kaggle score was 0.97404, securing the 6th position in the ranking.
