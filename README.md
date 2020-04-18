# Market Capitalization Classifier (Unicorn Classifier)

Unicorn, a privately held startup company valued at over \$1 billion. According to TechCrunch, there were 279 unicorns as of March 2018.

Wikipedia contributors. (2020, April 16). Unicorn (finance). In *Wikipedia, The Free Encyclopedia*. Retrieved
02:16, April 18, 2020, from https://en.wikipedia.org/w/index.php?title=Unicorn_(finance)&oldid=951237395

---

## Context

### How Valuable Is a Unicorn? Maybe Not as Much as It Claims to Be

Ilya A. Strebulaev and another professor working with him, Will Gornall of the University of British Columbia, have come to a startling conclusion: The average unicorn is worth half the headline price tag that is put out after each new valuation.

And if the special side deals that most unicorn companies offer to certain investors — more on this sleight of hand in a moment — are taken into account, almost half of the companies would fall below the $1 billion threshold.

- Gornall and Strebulaev (2018). "Squaring Venture Capital Valuations with Reality". Stanford University Working Paper. SSRN [2955455](https://ssrn.com/abstract=2955455).
- Sorkin, Andrew (2017). ["How Valuable Is a Unicorn? Maybe Not as Much as It Claims to Be"](https://www.nytimes.com/2017/10/16/business/how-valuable-is-a-unicorn-maybe-not-as-much-as-it-claims-to-be.html). *New York Times*. Retrieved 11 March 2018. 

### Market Capitalization

Market capitalization, commonly called market cap, is the market value of a publicly traded company's outstanding shares.

Market capitalization is equal to the share price multiplied by the number of shares outstanding. Since outstanding stock is bought and sold in public markets, capitalization could be used as an indicator of public opinion of a company's net worth and is a determining factor in some forms of stock valuation.

Wikipedia contributors. (2020, March 26). Market capitalization. In *Wikipedia, The Free Encyclopedia*. Retrieved
04:19, April 18, 2020, from https://en.wikipedia.org/w/index.php?title=Market_capitalization&oldid=947485274

---

## Goal

Given the valuation of unicorns may not be as much as it claims to be, it is difficult for potential investors to determine their market values as if the unicorns were to be traded in the public market. Since market capitalization could represent market values, can we determine the market capitalization of these unicorns?

Determining the exact market cap is a challenge because deriving the share price for calculating market cap is difficult, where share price can be affected by many different variables. Instead, based on the unicorns' financial performance, we will model a classifier that will divide the unicorns into large-cap, mid-cap and small-cap according to S&P's selection criteria.

It should be noted, however, that potential investors should still continue to perform due diligence. This tool was created to help make it easier for potential investors to sift through many unicorns, and find their ideal investments.

---

## Approach

1. Scrape 10 years of annual report (2009 - 2019) from https://www.sec.gov/edgar.shtml, for
    - 500 large-cap companies from S&P 500
    - 400 mid-cap companies from S&P 400
    - 600 small-cap companies from S&P 600
2. Perform exploratory data analysis (EDA) and cleaning, including analysing correlations between features and target (large-cap, mid-cap, small-cap)
3. Set baseline accuracy
4. Convert categorical variables into numbers (for modelling)
5. Split data into training (0.75) and test (0.25) at random, in a stratified fashion
6. Standardize all features with mean of 0 and standard deviation of 1
7. Assemble different pipelines with Step 5 and different models
8. Train different pipelines with hyper parameters tuning.
9. Repeat Step 5 to 7, while using Principal Component Analysis (PCA) to project down the dimensions of features
10. Select model with highest training accuracy
11. Investigate the important features from the best model

---

## Table of Contents

1. Scrape the data
    - [Scrape list of S&P 500 companies](code/01_wiki_sp500_scraping.ipynb)
    - [Scrape list of S&P 1000 companies (mid-caps and small-caps)](code/02_wiki_sp1000_scraping.ipynb)
    - [Scrape list of S&P 400 companies (to separate mid-caps from S&P 1000)](code/03_wiki_sp400_scraping.ipynb)
    - [From the list of S&P 500 companies, scrape data files from 10-K reports](code/04_edgar_sp500_scraping.ipynb)
    - [From the list of S&P 1000 companies, scrape data files from 10-K reports](code/05_edgar_sp1000_scraping.ipynb)
2. EDA and cleaning
    - [S&P 500](code/06_eda_and_cleaning_sp500.ipynb)
    - [S&P 1000](code/07_eda_and_cleaning_sp1000.ipynb)
    - [S&P 500 and S&P 1000 merged](code/08_eda_and_cleaning_sp1500.ipynb)
3. [Pre-processing](code/09_preprocessing.ipynb)
4. [Modelling](code/10_modelling.ipynb)
5. [Production Model and Insights](code/11_production_model_and_insights.ipynb)
6. [Conclusion](#Conclusion)

---

## Conclusion

In this project, we have attempted to model a market cap classifier and obtained features which are important in classifying companies to large-cap, mid-cap or small-cap. Using just the top few features, we can use scatter plots to visualize the relationships between the important features and market cap categories. Instead of having to manually search for a unicorn, potential investors can use this classifier to run through financial performance of all the unicorns, and with the new market cap category will aid potential investors to make informed decisions. 

The modelling of the data was done through XGBoost classifier on a pre-processed, standardized financials. Importance provides a score that indicates how useful or valuable each feature was in the construction of the boosted decision trees within the model. The more an attribute is used to make key decisions with decision trees, the higher its relative importance. The important features are found through supervised learning models and can be used in revealing patterns that human may not have even thought about.

The manual inspection of the documents was quite limited, as I might not be qualified enough to assess the meaning of the financial performances. Even so, it was apparant that the quantum in certain features could be related to a specific market cap. By organizing the unicorns, qualified people can quickly find a suitable investment. This project can further be improved by abstracting the underlying data analysis techniques as described in this notebook to develop a user interface/tool that presents the financial performance in a user-friendly manner.

---

## Future Thoughts To Consider

- Including quarterly reports for better comparability between companies with different financial year end
- Expand the modelling to include text documented within the annual report
- Expand data collection before 2009 and after 2020 (different document formats)
- Expand to companies outside of S&P 1500 boundaries
- To increase storage space to at least twice the size of current datasets, current datasets total up to 50GB where reading the file will take up the same amount of space as temporary storage
- Alternative storage solutions for scraped datasets which are too large to be pushed onto GitHub
- Current hyper parameter tuning `RandomizedSearchCV` is based on a sample from specified distributions, hence results can vary
- To implement `GridSearchCV`, more intensive hyper parameter tuning but demands more computing power
- Explore more hyper parameters to tune
- Explore time series modelling, recurrent neural networks (RNN), long short-term memory (LSTM)
- Long run time to read file and train models on large datasets
- Include actual financial performances of unicorns before and after IPO for evaluation of classifier
- Identify potential clusters using unsupervised models
- Identifying companies with creative accounting (but extremely rare known case such as Enron) 