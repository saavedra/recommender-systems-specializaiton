# Recommender Systems Capstone Project

## Part 1

### Business Goals

The purpose of this project is to create a recommendation engine for the “Back to School” campaign of Nile-River.com. 

In physical stores, it is common to see around 31% of annual sales happening during the month of August, while our website only reaches 23%. The goal of this project is to increase our sales during this critical period.

Several factors must be considered in the development of this system:

- **Sales Distribution**: Sales during this time of the year are spread across products from various categories. Our recommender system should emphasize diversity to address the different potential interests of our customers.
  
- **Price Range**: Sales are also spread across a wide range of price points. Therefore, we shouldn't focus only on high-ticket or high-volume products but should offer options across different price levels.

- **Product Variety**: Nile-River.com’s advantage lies in its extensive and varied inventory, offering products not typically found elsewhere. We want to surprise our customers by recommending more serendipitous and unexpected items.

Our recommendations will be shown in two key places:
- The "Back to School" landing page
- The "Related Products" section

Both sections will display 5 personalized recommendations for each user.

To address the above considerations, the performance of our recommender system will be evaluated based on the following metrics:
- **Accuracy**: Since we are showing our top 5 recommendations, we expect these items to be highly relevant. Therefore, accuracy will be our primary metric.
- **Diversity**: We expect our recommendations to include different product categories (e.g., it wouldn’t be ideal to recommend five types of pens). At the same time, we want to display a variety of price points.
- **Serendipity**: To highlight the vastness of our catalog, we plan to include unexpected items in our recommendations.

### Algorithm Evaluation

The following algorithms have been selected for evaluation:
- **TFIDF Content-based Recommender**: This will help optimize for diversity across product categories and provide off-the-shelf recommendations for new users.
- **User-User Collaborative Filtering**: This will help capture taste similarities between users, which can improve accuracy, especially for sparse datasets.
- **Matrix Factorization**: A more robust method designed for larger datasets. It can be optimized to produce strong accuracy (precision at k = 5) and also uncover latent dimensions that might correlate with unexpectedness.

All available models will be evaluated, as we already have the necessary results.

### Metrics

We will use the following metrics to measure our system's performance:
- **Mean Absolute Error (MAE)**: This metric measures the model's ability to predict the actual ratings given by users. It will be the primary metric to minimize during optimization. We choose the absolute error to make it easily comparable with the scale of the ratings.
  
- **Precision @ k = 5**: This will reflect the accuracy of our top recommendations. Given that we have limited placements, we expect these items to be highly relevant to the user.

- **Intra-list Diversity**: This measures how different the recommended items are from each other in terms of category and price range. We want to ensure our recommendations feature variety across product categories and price points.

- **Serendipity Score**: This evaluates the "unexpectedness" of the recommendations based on popularity. The score is calculated by determining the fraction of recommended items that are among the most popular, compared to the total number of recommendations.

### Constructing Hybrid Algorithms

For new users (with no history), we will initially serve content-based recommendations. As we gather more information about them, they can transition to receiving recommendations from similar users (user-user similarity). To determine the proper balance (and when to drop content-based filtering), simulations will be conducted with different weightings in increments of 10%:

- 90% CB - 10% UxU
- 80% CB - 20% UxU
- 70% CB - 30% UxU
- ...
- 10% CB - 90% UxU

These experiments will be run at different levels of engagement (e.g., when a user buys 1 product, 2 products, etc.).

The same procedure will be followed with pairs of:
- Content-Based and Matrix Factorization
- User-User Collaborative Filtering and Matrix Factorization

In total, 27 experiments will be conducted and compared based on the three metrics outlined in the previous section. The optimal combination will be the one that best meets our multi-faceted business goals.


## Part 2

Table 1 summarizes the metrics for each of the algorithm results on the full dataset.

|               | CBF   | Item-item | User-User | Matrix Factorization | Item-item |
|---------------|-------|-----------|-----------|----------------------|-----------|
| **MAE**       | 0.45  | 0.43      | 0.40      | 0.53                 | 0.53      |
| **Accuracy @ 5** | 0.40  | 0.65      | 0.96      | 0.30                 | 0.40      |
| **Diversity (Categories)** | 41    | 106       | 150       | 9                    | 5         |
| **Diversity (Price)**      | 3.27  | 3.01      | 2.95      | 3.02                 | 4.00      |
| **Serendipity** | 0.91  | 0.80      | 0.65      | 0.91                 | 0.91      |

As can be seen, **User-User Collaborative Filtering** is the best at predicting user ratings, provides the greatest accuracy, and, above all, is remarkably better than the alternatives at coming up with a diverse result set. On the other hand, it is the worst-performing at delivering surprising results, as well as presenting a lower variation of price.

It is worth noting that **Matrix Factorization**, the most sophisticated approach, delivers underwhelming results on the given dataset. One reason to choose it is that it is more efficient to run on bigger datasets. Also, it is a parametric model, so the choice of the correct number of latent factors is a determinant of the prediction results.


## Part 3

For producing the hybrid models, I combined the predictions from all available models in pairs, while varying the weightings for each.

From the available models:
- Content-based Filtering
- User-User Collaborative Filtering
- Item-Item Collaborative Filtering
- Matrix Factorization
- Personally Scaled Predictions (PersBias)

I created a total of 10 combinations:
- CBF + MF
- CBF + USER_USER
- CBF + ITEM_ITEM
- CBF + PERSBIAS
- MF + USER_USER
- MF + ITEM_ITEM
- MF + PERSBIAS
- USER_USER + ITEM_ITEM
- USER_USER + PERSBIAS
- ITEM_ITEM + PERSBIAS

Each combination was run 9 times, varying the weighting from 0.1 - 0.9 to 0.9 - 0.1.

In total, 90 experiments were run, and for each one, the mean absolute error, precision at k = 5, diversity, and serendipity were measured. The experiments were run using Python.

Table 2 shows the full results, sorted by precision at 5. The winning model, 20% CBF and 80% User_User, achieves 99% precision with a very decent mean absolute error of 0.39. This hybrid also produces good diversity and serendipity.


| model_a | model_b | weight_a | weight_b | mae  | precision_at_5 | diversity | serendipity |
|---------|---------|----------|----------|------|----------------|-----------|-------------|
| cbf     | user_user | 0.2 | 0.8 | 0.39 | 0.99 | 126 | 0.60 |
| cbf     | user_user | 0.1 | 0.9 | 0.39 | 0.97 | 138 | 0.62 |
| user_user | item_item | 0.9 | 0.1 | 0.39 | 0.97 | 142 | 0.64 |
| mf      | user_user | 0.1 | 0.9 | 0.40 | 0.96 | 139 | 0.62 |
| user_user | persbias | 0.9 | 0.1 | 0.40 | 0.96 | 137 | 0.62 |
| user_user | persbias | 0.8 | 0.2 | 0.39 | 0.96 | 123 | 0.62 |
| user_user | item_item | 0.8 | 0.2 | 0.38 | 0.96 | 141 | 0.65 |
| mf      | user_user | 0.2 | 0.8 | 0.39 | 0.95 | 125 | 0.62 |
| cbf     | user_user | 0.3 | 0.7 | 0.38 | 0.95 | 112 | 0.61 |
| user_user | item_item | 0.7 | 0.3 | 0.37 | 0.95 | 127 | 0.66 |
| mf      | user_user | 0.3 | 0.7 | 0.40 | 0.94 | 106 | 0.61 |
| user_user | persbias | 0.7 | 0.3 | 0.40 | 0.94 | 103 | 0.63 |
| cbf     | user_user | 0.4 | 0.6 | 0.38 | 0.93 | 96  | 0.61 |
| user_user | item_item | 0.6 | 0.4 | 0.37 | 0.91 | 116 | 0.69 |
| user_user | persbias | 0.6 | 0.4 | 0.40 | 0.91 | 86  | 0.63 |
| mf      | user_user | 0.4 | 0.6 | 0.40 | 0.90 | 85  | 0.60 |
| user_user | item_item | 0.4 | 0.6 | 0.38 | 0.90 | 107 | 0.73 |
| user_user | item_item | 0.5 | 0.5 | 0.37 | 0.90 | 110 | 0.72 |
| cbf     | user_user | 0.5 | 0.5 | 0.38 | 0.89 | 82  | 0.63 |
| mf      | user_user | 0.5 | 0.5 | 0.42 | 0.89 | 69  | 0.61 |
| user_user | persbias | 0.5 | 0.5 | 0.42 | 0.88 | 66  | 0.63 |
| user_user | item_item | 0.3 | 0.7 | 0.39 | 0.84 | 103 | 0.76 |
| user_user | persbias | 0.4 | 0.6 | 0.43 | 0.83 | 54  | 0.66 |
| cbf     | user_user | 0.6 | 0.4 | 0.39 | 0.82 | 69  | 0.66 |
| mf      | user_user | 0.6 | 0.4 | 0.43 | 0.81 | 57  | 0.62 |
| user_user | item_item | 0.2 | 0.8 | 0.40 | 0.80 | 103 | 0.78 |
| mf      | user_user | 0.7 | 0.3 | 0.45 | 0.75 | 43  | 0.66 |
| cbf     | user_user | 0.7 | 0.3 | 0.40 | 0.74 | 59  | 0.68 |
| user_user | item_item | 0.1 | 0.9 | 0.42 | 0.69 | 104 | 0.79 |
| user_user | persbias | 0.3 | 0.7 | 0.46 | 0.67 | 40  | 0.72 |
| cbf     | item_item | 0.2 | 0.8 | 0.42 | 0.65 | 92  | 0.82 |
| cbf     | item_item | 0.1 | 0.9 | 0.43 | 0.64 | 99  | 0.82 |
| mf      | item_item | 0.2 | 0.8 | 0.44 | 0.63 | 92  | 0.81 |
| cbf     | item_item | 0.4 | 0.6 | 0.42 | 0.63 | 81  | 0.83 |
| cbf     | item_item | 0.3 | 0.7 | 0.42 | 0.63 | 87  | 0.82 |
| cbf     | user_user | 0.8 | 0.2 | 0.42 | 0.63 | 48  | 0.77 |
| item_item | persbias | 0.8 | 0.2 | 0.44 | 0.63 | 90  | 0.82 |
| mf      | item_item | 0.1 | 0.9 | 0.43 | 0.63 | 98  | 0.82 |
| item_item | persbias | 0.9 | 0.1 | 0.43 | 0.62 | 98  | 0.82 |
| cbf     | item_item | 0.5 | 0.5 | 0.42 | 0.61 | 71  | 0.84 |
| item_item | persbias | 0.7 | 0.3 | 0.44 | 0.61 | 86  | 0.82 |
| mf      | item_item | 0.3 | 0.7 | 0.44 | 0.60 | 86  | 0.81 |
| cbf     | item_item | 0.6 | 0.4 | 0.42 | 0.58 | 62  | 0.84 |
| mf      | user_user | 0.8 | 0.2 | 0.48 | 0.58 | 25  | 0.74 |
| mf      | item_item | 0.4 | 0.6 | 0.45 | 0.58 | 78  | 0.81 |
| item_item | persbias | 0.6 | 0.4 | 0.45 | 0.56 | 75  | 0.82 |
| cbf     | item_item | 0.7 | 0.3 | 0.43 | 0.56 | 53  | 0.84 |
| mf      | item_item | 0.5 | 0.5 | 0.46 | 0.55 | 64  | 0.82 |
| user_user | persbias | 0.2 | 0.8 | 0.48 | 0.54 | 25  | 0.76 |
| item_item | persbias | 0.5 | 0.5 | 0.46 | 0.52 | 61  | 0.84 |
| mf      | item_item | 0.6 | 0.4 | 0.47 | 0.49 | 52  | 0.83 |
| cbf     | user_user | 0.9 | 0.1 | 0.43 | 0.48 | 42  | 0.84 |
| cbf     | item_item | 0.8 | 0.2 | 0.44 | 0.47 | 45  | 0.86 |
| mf      | item_item | 0.7 | 0.3 | 0.48 | 0.47 | 43  | 0.84 |
| item_item | persbias | 0.4 | 0.6 | 0.47 | 0.47 | 49  | 0.86 |
| mf      | user_user | 0.9 | 0.1 | 0.50 | 0.44 | 16  | 0.83 |
| cbf     | item_item | 0.9 | 0.1 | 0.44 | 0.44 | 43  | 0.89 |
| item_item | persbias | 0.3 | 0.7 | 0.48 | 0.43 | 40  | 0.88 |
| mf      | item_item | 0.8 | 0.2 | 0.50 | 0.42 | 29  | 0.87 |
| cbf     | mf | 0.9 | 0.1 | 0.46 | 0.40 | 39  | 0.90 |
| cbf     | persbias | 0.9 | 0.1 | 0.46 | 0.37 | 37  | 0.91 |
| cbf     | mf | 0.8 | 0.2 | 0.47 | 0.36 | 37  | 0.90 |
| cbf     | mf | 0.7 | 0.3 | 0.48 | 0.35 | 32  | 0.91 |
| cbf     | persbias | 0.8 | 0.2 | 0.47 | 0.35 | 35  | 0.91 |
| user_user | persbias | 0.1 | 0.9 | 0.51 | 0.34 | 11  | 0.87 |
| cbf     | mf | 0.6 | 0.4 | 0.48 | 0.33 | 27  | 0.90 |
| item_item | persbias | 0.2 | 0.8 | 0.50 | 0.33 | 25  | 0.91 |
| cbf     | persbias | 0.7 | 0.3 | 0.48 | 0.33 | 30  | 0.91 |
| mf      | item_item | 0.9 | 0.1 | 0.52 | 0.32 | 17  | 0.89 |
| cbf     | persbias | 0.6 | 0.4 | 0.48 | 0.32 | 26  | 0.91 |
| cbf     | mf | 0.5 | 0.5 | 0.49 | 0.31 | 22  | 0.91 |
| cbf     | mf | 0.4 | 0.6 | 0.50 | 0.31 | 19  | 0.91 |
| cbf     | mf | 0.3 | 0.7 | 0.51 | 0.29 | 15  | 0.91 |
| cbf     | persbias | 0.3 | 0.7 | 0.51 | 0.29 | 15  | 0.93 |
| mf      | persbias | 0.2 | 0.8 | 0.53 | 0.29 | 7   | 0.93 |
| cbf     | persbias | 0.5 | 0.5 | 0.49 | 0.29 | 21  | 0.92 |
| cbf     | persbias | 0.2 | 0.8 | 0.52 | 0.28 | 9   | 0.95 |
| cbf     | persbias | 0.4 | 0.6 | 0.50 | 0.28 | 19  | 0.92 |
| mf      | persbias | 0.9 | 0.1 | 0.53 | 0.28 | 9   | 0.92 |
| mf      | persbias | 0.8 | 0.2 | 0.53 | 0.28 | 9   | 0.93 |
| mf      | persbias | 0.6 | 0.4 | 0.53 | 0.28 | 9   | 0.94 |
| item_item | persbias | 0.1 | 0.9 | 0.52 | 0.28 | 18  | 0.94 |
| mf      | persbias | 0.3 | 0.7 | 0.53 | 0.28 | 8   | 0.90 |
| mf      | persbias | 0.1 | 0.9 | 0.53 | 0.28 | 6   | 0.96 |
| cbf     | persbias | 0.1 | 0.9 | 0.53 | 0.27 | 8   | 0.96 |
| mf      | persbias | 0.7 | 0.3 | 0.53 | 0.27 | 9   | 0.93 |
| mf      | persbias | 0.5 | 0.5 | 0.53 | 0.27 | 8   | 0.93 |
| mf      | persbias | 0.4 | 0.6 | 0.53 | 0.27 | 8   | 0.91 |
| cbf     | mf | 0.2 | 0.8 | 0.52 | 0.26 | 13  | 0.91 |
| cbf     | mf | 0.1 | 0.9 | 0.52 | 0.26 | 9   | 0.91 |


Pure **User-User Collaborative Filtering** had already achieved 96% accuracy, so we can see that by adding hybridization, the results can improve even more.

Finally, additional Python code was used to generate the recommendations at scale from the hybrid models. Table 3 illustrates the recommendations for 3 sample users, with **Pure User-User Collaborative Filtering**, **Pure CBF**, and the **Hybrid Model**. Since the **User-User** model is weighted higher, we can see that the final recommendations are heavily influenced by it, but the order has changed, as induced by CBF.


| user id | algorithm | Top 1 | Top 2 | Top 3 | Top 4 | Top 5 |
|---------|-----------|-------|-------|-------|-------|-------|
| 64      | user_user | 1518  | 1317  | 1951  | 1790  | 1240  |
| 64      | cbf       | 1874  | 1387  | 2257  | 1951  | 327   |
| 64      | hybrid    | 1518  | 2257  | 1032  | 1951  | 1317  |
| 65      | user_user | 45    | 1763  | 619   | 2068  | 1461  |
| 65      | cbf       | 2257  | 1032  | 1874  | 327   | 2247  |
| 65      | hybrid    | 45    | 1461  | 619   | 1763  | 1377  |
| 75      | user_user | 2080  | 1806  | 2258  | 327   | 2233  |
| 75      | cbf       | 2257  | 1874  | 1387  | 327   | 2247  |
| 75      | hybrid    | 2080  | 327   | 1806  | 2101  | 2169  |


## Part 4

### Introduction and Business Need

Nile-River.com aims to boost sales during its "Back to School" campaign. While physical stores often see 31% of their annual sales in August, our online platform currently sees only 23%. The goal is to develop a recommendation system that will increase both sales and customer satisfaction during this key period.

Because back-to-school shopping involves a variety of product categories and price points, our recommender needs to:
- **Increase Accuracy**: Ensure that the top recommendations are highly relevant, encouraging users to make purchases.
- **Promote Diversity**: Showcase a broad range of categories and varied price points so users can explore a diverse selection of products.
- **Add a Touch of Surprise**: Feature lesser-known or unexpected items (serendipity) to emphasize the uniqueness of our inventory.

In summary, the recommender system must help users find what they need quickly while also introducing them to fresh, interesting products they may not have initially considered.

### Evaluation Criteria (Non-Technical Explanation)

We evaluated our recommendation methods based on four main criteria:
- **Mean Absolute Error (MAE)**: Compares the actual user ratings with the predicted ratings. A lower MAE indicates that the system is better at estimating what a user will like or dislike.
- **Precision @ 5**: Measures how many of the top 5 items are relevant or interesting to the user. A higher precision means that the items we recommend are more likely to appeal to the user.
- **Diversity**: Measures how different the 5 recommended items are in terms of category and price. We aim to avoid recommending five items from the same category or five expensive items. Variety helps users discover new products.
- **Serendipity**: Measures how many of the recommended items are unexpected. If all the recommendations are popular, they may be relevant but not surprising. A good balance includes some hidden gems that can capture the user's attention.
