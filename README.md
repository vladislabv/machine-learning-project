Predicting Communication Success on LinkedIn using Emerging Communication Trends (Concept)
==============================
## Introduction
Corporate communication on social media plays a vital role in shaping an organization's reputation, building relationships, and driving business growth. So, according to "Forbes" [1], approximately 77% of businesses utilize social media to connect with customers and drive revenue, while 44% of them focus on building brand awareness through these platforms.

Among other popular social networks, LinkedIn stands out not only as a powerhouse for professional networking, but also as a robust communication channel for brands and corporations with over 400 million potential audience [2].
As marketing interests keep growing, the companies seek an opportunity to evaluate their communication performance. Therefore, more and more studies focus on the importance of engagement rate, as qualitative measure of success on social media. So, the study conducted in 2021 concludes the social media engagement rate as a main contributor to value and consumer engagement purchasing behavior [3]. It also converges with previous studies focused on the importance of user engagement [4] [5]. The advantages of high engagement rate also usually expand to visibility boost and reflection of active receptive audience.

The study will seek for an optimal predictive model, which can best forecast communication success of yet unpublished posts. During the Feature Engineering, the research will identify key words led/leading to communication excellence on LinkedIn. In combination, businesses can fine-tune their approach and achieve a better level of engagement.

## Data Description 
To gather valuable insights on the impact of corporate posting, the LinkedIn application programming interface (API) was used. By accessing this API, businesses can extract quantitative data, such as engagement metrics and impression counts, which help in evaluating their communication efforts.

We have access to 30 LinkedIn business accounts of 16 international companies, acting in industrial, chemical and finance branches, which publication history will be used for the research. Since the study may show analysis on sensible corporate data, the companies will be anonymized by a specific id.

The framework of interest only includes 4875 “classical” posts with text description since it will be used for communication pattern identification. To exclude an impact of boosting or advertising, measurements obtained by applying monetary assets were ignored, thus only naturally retrieved data will be further used. Due to API Limitations on historical data and possible huge effects of pandemic, the data is collected from January 2023 until July 2023, making the time span of 7 months.

The dataset provides measurements on daily frequency, including 70276 daily measurements over the specified timespan.

The framework contains following columns:

 - 	“client_id” [Int64] – An id of the underlying LinkedIn Page
 - 	“post_id” [Str] – An id of the underlying LinkedIn Post
 - 	“Message Text” [Str] – Original Text of a Post
 - 	“Date” [Str] – Date string in format “%Y-%m-%d” when the performance metrics were obtained.
 - 	“Impressions” [Int64] – Number of times a post appears on LinkedIn feed.
 - 	“Likes” [Int64] – Number of times a post was liked by users on LinkedIn.
 - 	“Shares” [Int64] – Number of times a post was shared by users on LinkedIn.
 - 	“Comments” [Int64] – Number of user comments assigned to a post on LinkedIn.
 - 	“Published Date” – Date string in format “%Y-%m-%d”, when a post was officially published to a client’s feed.
 - 	“Engagements” [Int64] – Sum of likes, comments, and shares
 - 	“Engagement Rate” [Float] – Number of Engagements divided by the number of Impressions. (<= 1.0)

## Methodology

### Feature Engineering
As working with large message text volumes will affect model performance and sometimes its prediction quality, we will try achieve text summarization and discovering the hidden themes in the text collection. For this purpose, topic models will be used, which assign words to topics based on their co-occurrence in documents. The output of those models are probabilities, how likely a text entity may belong to a particular topic. Some techniques are purely based on probabilities computations, other use machine learning models as helpers to provide more accurate results but are more likely to run in an overfit. One of the models, which comes in handy in the professional word is Latent Dirichlet allocation (LDA), which is one of the examples of a Bayesian topic models. In terms of this model, a message text can belong to multiple topics and is ordered to a particular topic based on its probability score in the text corpus.[6]

One of the main tasks to achieve meaningful topics, is choosing how many categories best describing our data. This can't be calculated and needs to be explored by iterating over a predefined numerical range. To be sure that the allocation is better or worse than previous, we will compare coherent scores of the each iteration. As a plateau is reached, so we can assume that we reached an optimal number of topic that best describe the data. [7]

As the optimal number of topics is defined, we will need to restrict which observations will count as representative for each topic. So, it is manageable to put labels only considering 10 or 20 words instead of hundreds. To be precise, let us consider our dataset. Without any deep dive into the data, it is likely to assume that company names or specific product brands will hugely affect shapes of the topics and, therefore, should not be considered by LDA processing.

Once, we found the hidden themes among the post texts, we can tag all the collected posts, based on the word's occurence in a document. So, if a post A has 10 words belonging to a topic "Health" and 15 to the "Government", a post will be tagged as "Government". Further we omit texts and work only with the defined tags, where each tag corresponds to an output topic provided by the LDA.

Based on the tags, we will see how successful are the posts within each group by calculating a median of engagement rates of underlying documents. This will help us to determine better or worse topics to post on the LinkedIn.

Since the acquired dataset is rather low-dimensional, the feature selection won’t be explicitly used in the project. However, some predictive algorithms use embedded feature selection, like Decision Trees. In the final part of the project, it will then be compared if this affected the prediction rate.

### Data Processing
Due to many algorithms’ sensitivity to outlier data points, we will use density-based spatial clustering of applications with noise (DBSCAN) algorithm to detect and handle this type of data. The algorithm is mainly to find separate clusters that are closely positioned and mark outliers in datasets [8].

Once the data is collected, it needs to be preprocessed to ensure compatibility with the LDA model. We will begin with text cleaning, which is removing any unnecessary characters, special symbols, and formatting inconsistencies from the collected data. After this, the text will be tokenized into individual words. Further we will eliminate common stop words, based on a position tag, so prepositions or pronouns can be omitted. The rest of the tokens will be again reduced by applying lemmatization and stemming approaches to filter out inflected words for more accurate topic extraction.

### Model Selection & Evaluation
Based on the framework structure, to predict how successful a post can be, only prediction models can handle both categorical and numerical variables at the same time as features makes sense.

Therefore, we will start with Decision Tree Algorithm, which also has feature selection embedded. It involves creating a tree-like structure where each internal node represents a decision based on a feature, each branch represents the result of that decision, and each leaf node represents a final classification or regression value.
At the end, a CatBoost gradient boosting model will be used, which fights prediction shift occurred by adding categorical variables for prediction. This model operates with decision trees and processes categorical variables by applying ordered boosting [9].

For interim evaluation of the applied models, Shapley Additive explanations (SHAP) will be used. It is mainly used to explain the predicted output got from Machine Learning Models by making it more understandable for humans. SHAP works by breaking down the model's output into the individual impacts of each feature. It calculates a number to show how much each feature affects the model's outcome. These numbers will help understand which features are important and explain the model's results [10].

## Conclusion
The aforementioned concept aims to develop an optimal predictive model to forecast the communication success of unpublished posts. Through Feature Engineering, the study will identify key words that contribute to communication excellence on LinkedIn. By leveraging this model and knowledge of impactful keywords, businesses can refine their approach and enhance engagement levels.

# References
1. Blei, DM, AY Ng & MI Jordan (2003). Latent dirichlet allocation. Journal of machine Learning research 3(Jan), 993–1022.
2. Cabiddu, F, MD Carlo & G Piccoli (2014). Social media affordances: Enabling customer engagement. Annals of Tourism Research 48, 175–192. https://www.sciencedirect.com/science/article/pii/S0160738314000796.
3. Hasan, M, A Rahman, MR Karim, MSI Khan & MJ Islam (2021). Normalized approach to find optimal number of topics in Latent Dirichlet Allocation (LDA). In: Proceedings of International Conference on Trends in Computational and Cognitive Engineering: Proceedings of TCCE 2020. Springer, pp.341–354.
4. Khan, K, SU Rehman, K Aziz, S Fong & S Sarasvady (2014). DBSCAN: Past, present and future. In: The fifth international conference on the applications of digital information and web technologies (ICADIWT 2014). IEEE, pp.232–238.
5. Leading countries based on LinkedIn audience size as of January 2023 (in millions) (n.d.). https://www.statista.com/statistics/272783/linkedins-membership-worldwide-by-country. Accessed: 2023-07-22.
6. Prokhorenkova, L, G Gusev, A Vorobev, AV Dorogush & A Gulin (2018). CatBoost: unbiased boosting with categorical features. Advances in neural information processing systems 31.
7. Top Social Media Statistics And Trends Of 2023 (n.d.). https://www.forbes.com/advisor/business/social-media-statistics. Accessed: 2023-07-22.
8. Van den Broeck, G, A Lykov, M Schleich & D Suciu (2022). On the tractability of SHAP explanations. Journal of Artificial Intelligence Research 74, 851–886.
9. Yost, E, T Zhang & R Qi (2021). The power of engagement: Understanding active social media engagement and the impact on sales in the hospitality industry. Journal of Hospitality and Tourism Management 46, 83–95.
10. Zhang, M, L Guo,MHu &WLiu (2017). Influence of customer engagement with company social networks on stickiness: Mediating effect of customer value creation. International Journal of Information Management 37(3), 229–240. https://www.sciencedirect.com/science/article/pii/S0268401216302195.
------------
Project Organization
------------

    ├── LICENSE
    ├── Makefile           <- Makefile with commands like `make data` or `make train`
    ├── README.md          <- The top-level README for developers using this project.
    ├── data
    │   ├── external       <- Data from third party sources.
    │   ├── interim        <- Intermediate data that has been transformed.
    │   ├── processed      <- The final, canonical data sets for modeling.
    │   └── raw            <- The original, immutable data dump.
    │
    ├── docs               <- A default Sphinx project; see sphinx-doc.org for details
    │
    ├── models             <- Trained and serialized models, model predictions, or model summaries
    │
    ├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
    │                         the creator's initials, and a short `-` delimited description, e.g.
    │                         `1.0-jqp-initial-data-exploration`.
    │
    ├── references         <- Data dictionaries, manuals, and all other explanatory materials.
    │
    ├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
    │   └── figures        <- Generated graphics and figures to be used in reporting
    │
    ├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
    │                         generated with `pip freeze > requirements.txt`
    │
    ├── setup.py           <- makes project pip installable (pip install -e .) so src can be imported
    ├── src                <- Source code for use in this project.
    │   ├── __init__.py    <- Makes src a Python module
    │   │
    │   ├── data           <- Scripts to download or generate data
    │   │   └── make_dataset.py
    │   │
    │   ├── features       <- Scripts to turn raw data into features for modeling
    │   │   └── build_features.py
    │   │
    │   ├── models         <- Scripts to train models and then use trained models to make
    │   │   │                 predictions
    │   │   ├── predict_model.py
    │   │   └── train_model.py
    │   │
    │   └── visualization  <- Scripts to create exploratory and results oriented visualizations
    │       └── visualize.py
    │
    └── tox.ini            <- tox file with settings for running tox; see tox.readthedocs.io


--------

<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>
