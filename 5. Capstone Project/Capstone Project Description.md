# Capstone Assignment

## Capstone Project
### The Challenge: Design, Measure, Mix, Propose, and Justify

This capstone project will bring together the skills you’ve learned across the four prior courses of the Recommender Systems specialization in a single project. You will be given a data set and a specific scenario, and will be expected to research, propose, and justify a recommender specifically designed to match that data set and scenario. You will carry out this project individually in four parts, all of which will be submitted together as a final project report:

### **1. Design**

Your first challenge is to understand your data set and scenario and produce a research design. This design will identify a set of metrics and evaluation techniques you will use to evaluate possible algorithms for your recommender, and will outline your plans for exploring both individual and hybrid recommender algorithms. Pay particular attention to how you plan to separate training and test data to ensure that your tests are valid. The plan can be brief (2-3 pages), but must explain how the metrics you choose relate to the business goals in the scenario.

### **2. Measure**

Next, you will work with a set of at least three different base algorithms (drawn from among those you’ve studied) to understand how they perform on the provided data set, using your selected metrics. As part of this step you may need to tune some of the base algorithms to get reasonable performance.

### **3. Mix**

With complex objectives, it is likely that no single algorithm will produce a set of recommendations that meet all of the goals of the scenario. Thus, you need to explore hybrid algorithms to provide the best result set. We expect you to explore at least two, and possibly several different hybrids to find the best results.

### **4. Proposal and Reflection**

Finally, you should present your recommendation for the algorithm (including possibly a hybrid algorithm) that should be used to fulfill the scenario. You should justify the result and the means used to achieve it, and should address a set of questions about your exploration.


## The Data Set

You will be using a data set derived from Amazon.com with product metadata and ratings data on office products. The data set is provided thanks to Julian McAuley at UCSD, and involves actual data from the period May 1996-July 2014. To make your computation more tractable, we’ve used a dense subset of the data (called the 5-core subset) that only includes items and users with at least five ratings. 

**Note**: The original datasets are available at [http://jmcauley.ucsd.edu/data/amazon/](http://jmcauley.ucsd.edu/data/amazon/), though these should not be used for this capstone.  
There are separate data set extracts for those of you completing the programming (honors) track and the non-programming track. The non-programming track dataset is smaller to make it more feasible for use in spreadsheet computation.

For each item, your meta-data includes:
- An item number
- Amazon’s ITEM number (“asin”)
- The item’s brand name
- The item title
- The item category (both leaf category and full path)
- A price in dollars
- An availability score between 0 and 1 that reflects how widespread the product is in retail stores; higher scores reflect broad availability; lower scores indicate products not found in most big box stores. Note that the availability score is synthetic (we created it), but for purposes of this capstone, treat it as if it were real data.

You are also provided with a ratings matrix with a row for each item and columns representing each user (ratings are on a 1-5 star scale). Your ratings matrix includes all the ratings data you will receive (we have not separated out test and training data -- that’s your responsibility).

## Capstone Scenario (Project Objective)

Your project should assess and recommend a recommender solution for the following scenario:

You work for a large online retailer (we’ll call it Nile-River.com) as a recommender systems expert on a team focused on direct sales to consumers in the US.

Your market research team has identified "back to school" as a critical time period for office product sales to consumers in the US. They note that the six weeks including and surrounding the month of August are responsible for 31% of yearly office product sales.

They also report that the surge in office product sales is not limited to traditional school products (such as notebooks, pencils, and erasers). Rather, it appears that once people are buying school products, they also buy other office products (indeed, more document shredders are sold during these six weeks than at any other time of the year, even tax preparation season).

Indeed, most large-dollar office-product purchases include a mix of inexpensive and more expensive products in the same transaction. This data suggests that it may be important to have inexpensive products as an entry point, but more expensive ones that build the transaction size.

They suspect that the surge in office product sales is due to in-person sales and promotion at retail outlets, with two particularly important prompts:
- Visits to office products superstores (chain stores such as Staples and Office Depot) peak during this time of year.
- Special displays are set up at "Big-Box" stores such as Wal-mart and Target with both school supplies and other office products.

### Problem statement

Unfortunately, your site (Nile-River.com) does not experience as large a surge in office product sales during the back-to-school period. You do experience a surge (about double typical sales, or about 23% of annual consumer sales), but it is far below that of your offline competitors.

These figures include the results of existing promotions such as back-to-school banners and a free next-day shipping promotion for products sold during the two weeks when schools most commonly start classes (late August and early September).

Your challenge, therefore, is to develop a recommender system to increase sales of office products during this important time period. To maximize business value, you also have a set of key goals and constraints.

Given that your site already has a very effective product-association recommender system, you’ve been asked to focus on recommending products based on customer’s overall profiles, not their current browsing or basket.

Your product recommendations will be displayed in two places on the site:
1. Five products displayed on the “office products” landing page where customers will land if they click on banner ads (back to school shopping!) or select the office products category (from various menus or navigation aids).
2. Five products displayed as part of “other suggestions” that will be displayed as part of the shopping cart display and near the bottom of product pages (primarily will be placed on product pages from the same category, but also related products such as textbooks, school bags, and backpacks).

## Part I: Designing a Measurement and Evaluation Plan (Joint)

Your first task is to develop a written capstone project plan. Specifically, this plan should detail a set of objective measures that will be used to evaluate candidate recommender systems as well as a clear plan for the multi-step evaluation process. Among the elements of this plan are:
- Translation of business goals and constraints into metrics and measurable criteria.
- A plan for evaluating a set of base algorithms.
- A plan for constructing and evaluating hybrid algorithms.


## Part II: Measurement (Separate)
### Standard

For the standard version of this assignment, you have been provided with the predicted rating outputs from five different algorithms:
- A TFIDF content-based recommender.
- An item-item collaborative filtering recommender.
- A matrix factorization (gradient descent) recommender.
- A user-user collaborative filtering recommender.
- A baseline recommender that uses product and customer ratings distributions to provide personally-scaled average predictions (PersBias).

You must calculate a set of metrics for at least three of these algorithms (it may be useful to do all five). The metrics are those that you identified above as relevant to the business goals of your algorithm, though in some cases you may need to make compromises to find close-enough metrics that you are able to compute.

## Part III: Mixing the Algorithms (Separate)
### Standard

Next, you will explore at least four combinations of algorithms to produce the best possible hybrid algorithm for this dataset and objective (based on your defined criteria). Your goal is to produce top-5 lists that provide the best performance against your combined set of criteria.

## Part IV: Proposal and Reflection (Joint)

The final part of your project is a brief “business proposal” and a reflection on what you’ve learned in the project.

**Deliverable 1**: A two-page “business proposal” for the recommender choice you’ve determined is best.

**Deliverable 2**: A reflection document on the process, approximately two pages, but no fixed limit, addressing at least the following five questions.
1. Reflect on the process of translating business requirements to metrics.
2. Reflect on the process of evaluating individual algorithms and creating hybrids.
3. Reflect on data management, including the process of separating training and test data.
4. Reflect on the tools you used for this capstone.
5. Finally, reflect on the capstone experience as a whole.
