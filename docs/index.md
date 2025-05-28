

By Nikhil Ghugare & Prateek Pagare

[Github Repository](https://github.com/Prateekmax21/Unsupervised-Learning-on-Airbnb-NYC-Listings)


## Introduction: Letting the Data Speak

Imagine you're browsing Airbnb for a place to stay in New York City. You scroll through endless options, small studios in Harlem, cozy lofts in Brooklyn, luxurious brownstones in Manhattan. Each one is priced differently, available on different days, and has a different vibe. Overwhelming, right?

Now imagine trying to make sense of thousands of these listings, not as a tourist, but as a data scientist. Can we uncover patterns in all this chaos? Are there groups of listings that naturally go together, like budget stays that are always open, or expensive listings hosted by professionals?

That’s the journey we took in this project.

We used unsupervised learning, a type of machine learning that doesn’t need labels. Instead of telling the model what’s what, we let the data speak for itself.

Our tools included:
- PCA (Principal Component Analysis): to simplify and visualize complex data
- Matrix Completion: to handle missing values like in the real world
- K-Means Clustering: to let listings group themselves based on behavior
- Hierarchical Clustering: to explore nested structures and relationships

All of this was applied to publicly available Airbnb data for New York City. Our goal? To find natural groupings and insights that could help both hosts and Airbnb itself better understand their marketplace.

---

## What We Used: The Dataset

We pulled our data from [Inside Airbnb](http://insideairbnb.com/get-the-data.html), which provides real-world Airbnb listings scraped from the site.

From the full dataset, we focused on the following numeric columns to describe a listing:

- `price`
- `minimum_nights`
- `maximum_nights`
- `number_of_reviews`
- `reviews_per_month`
- `availability_365`
- `calculated_host_listings_count`
- `accommodates`
- `bathrooms`
- `bedrooms`

These features give us a snapshot of how a listing behaves, how expensive it is, how long you can stay, how many reviews it gets, and how active the host is.

Before we could do anything useful, though, we had to clean up the mess…

---

## Cleaning Up the Chaos

Real-world data is messy. Airbnb listings are no exception. Some listings had missing prices, blank review scores, or unrealistic values like minimum stays of 1000 nights.

Here’s what we did:
- Dropped rows with missing or blank values
- Converted `price` from strings like "$150" to numbers
- Removed extreme outliers (like absurd night limits)
- Standardized all features so they’re on the same scale

Why standardize? Because we don’t want features like `price` (which can be in the thousands) to overpower features like `bathrooms` (usually between 1 and 3).

Now we were ready to dig in.

---

## Peeking Under the Hood with PCA

Imagine trying to understand listings using 10 different features. That’s like trying to visualize a 10-dimensional shape, it’s impossible for the human brain. Enter PCA: a technique that transforms complex data into fewer "principal components" that still retain most of the information.

We applied PCA to our cleaned, standardized data. Here’s what we found:
- PC1 (the first principal component) explained ~26% of the variance
- The first 3 PCs together explained ~58%
- The first 5 PCs captured over 80% of the variance

This told us we could safely reduce our dataset to 2 or 3 dimensions for analysis without losing much information.

We visualized this in two key plots:

**Scree Plot**: shows how much variance each PC explains  
**Cumulative Variance Plot**: adds them up to show total variance

![PCA](https://github.com/user-attachments/assets/517d3c97-f9ee-4cfd-9858-677fa6566e8c)

These plots helped us decide to keep the first 2–3 PCs for clustering.

---

## What If Some Data Is Missing?

To mimic real-world messiness, we randomly removed about 5% of the data and tried to fill it back in using matrix completion, a clever trick based on PCA.

Using the top 5 components, we reconstructed the missing values and then compared them to the originals. The correlation was ~0.51, not perfect, but pretty solid for data we pretended not to know.

This experiment showed that PCA isn’t just for simplification, it can also help recover missing data!

---

## Letting the Data Organize Itself: K-Means Clustering

With our simplified data in hand (PC1 and PC2), we used K-Means to group listings into natural clusters.

But how many clusters should we use? We tested different values of `k` (2 through 6) and used two tools to help us decide:

**Elbow Plot (Inertia)**: Shows when adding more clusters stops improving fit  
**Silhouette Scores**: Measures how tightly grouped each cluster is

![Kmeans clusters](https://github.com/user-attachments/assets/70f60ff0-5551-4032-80cd-7a0d1d1ef90b)

Both pointed to `k = 3` as the sweet spot.

Then, we plotted the listings again, now colored by cluster. The result was clear: K-Means had found three distinct groups.

![cluster k=3](https://github.com/user-attachments/assets/71548f9d-b312-4651-964e-43cb47f20569)

This plot shows how the listings are grouped into three clusters using K-Means on the first two principal components. Each color represents a different cluster.

![centroid](https://github.com/user-attachments/assets/583d1442-af35-4299-b005-ed5777d2799d)

Here we show the same clusters as before, but with red "X" markers representing the centroid of each cluster.

---

## What Were the Clusters?

To find out what these clusters meant, we looked at the average values of each feature per group.

Cluster 0:
- Low price
- High availability
- Many reviews
- Likely budget-friendly, highly active hosts

Cluster 1:
- High price
- Large number of bedrooms and accommodates
- Possibly commercial or luxury listings

Cluster 2:
- Medium price
- Low reviews
- Long minimum stays
- Could be occasional or passive rentals

These weren’t just statistical groups, they told real stories about different types of hosts and listings.

---

## Going Deeper with Hierarchical Clustering

Unlike K-Means, hierarchical clustering doesn’t require us to pick the number of clusters up front. It creates a tree-like diagram called a dendrogram that shows how listings group together at different levels.

We tested three linkage strategies:
- Complete Linkage: merges groups with the farthest points
- Average Linkage: merges based on average distance
- Single Linkage: merges closest individual points (can get "stringy")

![complete linkage](https://github.com/user-attachments/assets/dc543ec8-2d86-429c-847b-1a21a4137fac)

This dendrogram groups listings using complete linkage. It gives a nested view of cluster similarity.

![complete linkage with cut height 4 5](https://github.com/user-attachments/assets/2722c0f0-14b3-4271-956c-6067dcebe999)

The red dashed line shows a cut at height 4.5, dividing the tree into 3 main clusters that align well with K-Means.

![average linkage](https://github.com/user-attachments/assets/8ed74354-64b0-40ee-840a-25c847e73599)

Average linkage merges clusters based on the average pairwise distances between them. It gives more balanced clusters compared to single linkage.

![single linkage](https://github.com/user-attachments/assets/54a1c8f4-b5a5-494b-9ffe-76219896a8aa)

Single linkage tends to form long "chains" of points. It can be sensitive to noise and often creates elongated clusters.

---

## What We Learned

- PCA is super useful for reducing complexity while keeping the signal  
- Matrix completion with PCA isn’t perfect, but works well for filling small gaps  
- K-Means clustering found clear and interpretable groups in the data  
- Hierarchical clustering gave us another lens to confirm our findings  

Together, these tools let us uncover real, meaningful patterns, all without knowing anything about the listings ahead of time.

---

## Why It Matters

This kind of analysis can help:
- Hosts see where their listing fits in the market
- Airbnb optimize search and recommendations
- Analysts understand marketplace behavior

It also shows how unsupervised learning can be powerful in the messy, unlabeled world of real data.

---

## References

1. Inside Airbnb Data: http://insideairbnb.com/get-the-data.html  
2. Jolliffe, I. T. (2002). Principal Component Analysis. Springer.  
3. Tan, P.-N., Steinbach, M., & Kumar, V. (2019). Introduction to Data Mining.  
4. scikit-learn documentation: https://scikit-learn.org/stable/  
5. ISLP Lecture Notebooks: Ch12-1.ipynb, Ch12-2.ipynb  
6. GeeksforGeeks PCA Tutorial: https://www.geeksforgeeks.org/principal-component-analysis-pca/
7. scikit-learn Documentation: https://scikit-learn.org/stable/
8. Clustering in ML : https://scikit-learn.org/stable/modules/clustering.html
9. deploy github pages : https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

---

This blog is based on our Practical Homework for DATA 5322: Statistical Machine Learning II (Spring 2025). It reflects everything we learned and everything we discovered in course.
