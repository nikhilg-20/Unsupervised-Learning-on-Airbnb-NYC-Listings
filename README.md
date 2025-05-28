# NYC Airbnb Data: An Unsupervised Learning Approach

This project explores hidden patterns in New York City Airbnb listings using unsupervised learning techniques. The aim is to uncover distinct groups of listings based on pricing, availability, and review behaviors—without using any predefined labels.

We applied key methods such as Principal Component Analysis (PCA), matrix completion, K-Means clustering, and hierarchical clustering to analyze and interpret real-world data from Inside Airbnb.

---

## Features Used

•⁠  ⁠⁠ price ⁠  
•⁠  ⁠⁠ minimum_nights ⁠  
•⁠  ⁠⁠ maximum_nights ⁠  
•⁠  ⁠⁠ number_of_reviews ⁠  
•⁠  ⁠⁠ reviews_per_month ⁠  
•⁠  ⁠⁠ availability_365 ⁠  
•⁠  ⁠⁠ calculated_host_listings_count ⁠  
•⁠  ⁠⁠ accommodates ⁠  
•⁠  ⁠⁠ bathrooms ⁠  
•⁠  ⁠⁠ bedrooms ⁠  

---

## Project Question

Can we uncover hidden groups of Airbnb listings that reveal different pricing strategies, availability patterns, and hosting behavior?

Identified patterns include:

•⁠  ⁠Budget-friendly listings with high availability  
•⁠  ⁠Luxury or commercial listings with fewer reviews  
•⁠  ⁠High-volume host listings with distinct review behavior  

---

##  Techniques Applied

•⁠  ⁠PCA for dimensionality reduction and visualization  
•⁠  ⁠Matrix completion with simulated missingness  
•⁠  ⁠K-Means clustering (using PCA-reduced features)  
•⁠  ⁠Hierarchical clustering (single, average, complete linkage)  
•⁠  ⁠Scree plots and dendrograms for interpretation  

---

## Course Information

•⁠  ⁠Course: DATA 5322 – Statistical Machine Learning II  
•⁠  ⁠Term: Spring 2025  
•⁠  ⁠Instructor: Dr. Ariana Mendible
•⁠  ⁠Assignment: Practical Homework 4 – Unsupervised Learning  
•⁠  ⁠Institution: Seattle University  

---

## Blog Post

➡️ https://prateekmax21.github.io/Unsupervised-Learning-on-Airbnb-NYC-Listings/


---

## References

1.⁠ ⁠Inside Airbnb Dataset – http://insideairbnb.com/get-the-data.html  
2.⁠ ⁠Jolliffe, I. T. (2002). Principal Component Analysis. Springer.   
3.⁠ ⁠scikit-learn Documentation – https://scikit-learn.org/stable/

---

Built with Python, pandas, matplotlib, scikit-learn, and seaborn
