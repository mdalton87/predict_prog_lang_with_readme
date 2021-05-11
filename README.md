# <a name="top"></a>Predicting a Repositories Primary Coding Language using only the README.md File.

***
[[Project Description](#project_description)]
[[Project Goals](#goals)]
[[Project Planning](#planning)]
[[Key Findings](#findings)]
[[Data Dictionary](#dictionary)]
[[Data Acquire and Prep](#wrangle)]
[[Data Exploration](#explore)]
[[Statistical Analysis](#stats)]
[[Modeling](#model)]
[[Conclusion](#conclusion)]
___


<a name="project_description"></a>
   <img src="https://docs.google.com/drawings/d/e/2PACX-1vQNNiHHSR9vTJbzeTI5SoP4Jqm_FstTKjPqf_56sMJ2RgOO8JrI7DP_qL_uugm45I69acUUjmqhFlSH/pub?w=927&amp;h=274">
[[Back to top](#top)]

***

### This project is designed to familiarize myself with Natural Language Processing and web scraping techniques. I build a dataframe by collecting readme files and primary coding language from repos, and run classification models to predict the language being used.

***

<a name="goals"></a>
<img src="https://docs.google.com/drawings/d/e/2PACX-1vT0vWlUMODQ19OWW2VWW1F2uZEheo5QQawe-Re8n_Q6VUuHmq6ESvpIJrnWSu7NdaWzsb8KerGCj5Gl/pub?w=929&amp;h=274">
[[Back to top](#top)]

### The goal of this project is to create a model that out performs the baseline prediction.

***

<a name="planning"></a>
<img src="https://docs.google.com/drawings/d/e/2PACX-1vQ_thjFgc_szZ9fZCPeZ3nDUuvXxOSGlI82UYj6SB32PlQHiMQQpwq8cxqKl-X2kRJ8FsLpylIGLjpN/pub?w=927&amp;h=274">
[[Back to top](#top)]

***

### Begin by choosing a topic/organization to collect readme files:
- I decide to go with Microsoft's GitHub because of the large quantity of repos to choose from.
- Need to use BeautifulSoup and create a loop to collect the repo names to make a list.
- Once I have the list I clean the list of special characters/whitespaces/Archived repos.
- Once the list is cleaned I applly it to the scrape_github_data() function which collects as a json file.
- json file is converted into a dataframe and prepared for exploration.
- Models are applied and analyzed.

***

<a name="findings"></a>
<img src="https://docs.google.com/drawings/d/e/2PACX-1vSqMYDmxT3JzgfeJlQifIKITgmFOFXTHzM8NOtRLx7G8AzVXRNDTI14AkUxESF3zTyZD2-QpOGied8Y/pub?w=927&amp;h=275">
[[Back to top](#top)]

***

- Top 10 most common coding languages in Microsoft's GitHub:
    - C#
    - TypeScript
    - Python
    - C++
    - JavaScript
    - PowerShell
    - Juptyer Notebook
    - C
    - Java
    - HTML
- Baseline is the 'mode' C#, at 28.3% accuracy
- SGD Classifier is the best model performing 57.2% accuracy

***

<a name="dictionary"></a>
<img src="https://docs.google.com/drawings/d/e/2PACX-1vQO0DWXI17i4jNcIiV_EzldyjQVH0Tmpa9a2LG6GMiQhS2cTKcnULDK4FUy0xvmpd_ss3oq47tzBxJa/pub?w=928&amp;h=272">
[[Back to top](#top)]

---
| Attribute | Definition | Data Type |
| ----- | ----- | ----- |
| **repo** | Unique identifier for of the repo location | object/str |
| **language** |  Most common coding language in repo | object/str |
| **readme_contents** | Original README text | orbject/str |
| **lemmed_readme** | Cleaned, tokenized, and lemmatized README text without STOPWORDS | object/str |

***

<a name="wrangle"></a>
<img src="https://docs.google.com/drawings/d/e/2PACX-1vR19fsVfxHvzjrp0kSMlzHlmyU0oeTTAcnTUT9dNe4wAEXv_2WJNViUa9qzjkvcpvkFeUCyatccINde/pub?w=1389&amp;h=410">
[[Back to top](#top)]


## Acquire
All functions use to acquire data is in the acquire.py file within this repo.

## Prepare
- Normalized README
- Tokenized README
- Lemmatized README
- Removed Stopwords from README
- Removed a couple addition words that provided no additional insight

***

<a name="explore"></a>
<img src="https://docs.google.com/drawings/d/e/2PACX-1vQUW9iv6p6EhuRHQwbhiXPrwzj5ED81TvY-UaXH-AApx4xeGz_KKMnv1EtBVW2RH6SXfLM4OTOoCY7D/pub?w=929&amp;h=274">
[[Back to top](#top)]

***

- Created bigrams and trigrams for C#, Python, and Java containing the top 10 most common ngrams
- C reated word clouds of the same languages

***

<a name="model"></a>
<img src="https://docs.google.com/drawings/d/e/2PACX-1vTEBbtXZ7ssMV7E2bGBgX0dYoMU4HMOIWMItsuYSUgg9mm9wSWK6s6lhkKII6MjIaKwW1GWECJNsEB9/pub?w=928&amp;h=274">
[[Back to top](#top)]

***

- Used GridSearchCV to optimize the parameters for:
    - Logisitic Regression
    - K Nearest Neighbors
    - Decision Tree
    - Ridge Classifier
    - SGD Classifier
    
    
### GridSearchCV Results:

| Model | Parameters | Accuracy |
| ----- | ----- | ----- |
| **Logisitic Regression** | {'C': 10, 'penalty': 'l2'} | 0.559 |
| **KNN** | {'algorithm': 'auto', 'n_neighbors': 20, 'p': 2, 'weights': 'distance'} | 0.487 |
| **Decision Tree** | {'criterion': 'gini', 'max_depth': 5, 'min_samples_split': 2, 'splitter': 'best'} | 0.468 |
| **Ridge** | {'alpha': 1.0, 'class_weight': None, 'copy_X': True, 'fit_intercept': False, 'normalize': True} | 0.579 |
| **SGD** | {'alpha': 0.001, 'fit_intercept': False, 'loss': 'modified_huber', 'max_iter': 10, 'penalty': 'l2', 'shuffle': True, 'verbose': 1} | 0.584 |

### Baseline:
- C# is the most common language
     - create a column holding only C# and compared it to the actual
     - Baseline is 28.3%
     
### Logistic Regression Model: 

| Model | Parameters | Train | Validate | Test |
| ----- | ----- | -----: | -----: | -----: |
| **Logisitic Regression** | {'C': 10, 'penalty': 'l2'} | 0.99 | 0.57 | 0.58 |
| **KNN** | {'algorithm': 'auto', 'n_neighbors': 20, 'p': 2, 'weights': 'distance'} | 0.99 | 0.53 | 0.48 |
| **Decision Tree** | {'criterion': 'gini', 'max_depth': 5, 'min_samples_split': 2, 'splitter': 'best'} | 0.47 | 0.51 | 0.42 |
| **Ridge** | {'alpha': 1.0, 'class_weight': None, 'copy_X': True, 'fit_intercept': False, 'normalize': True} | 0.98 | 0.64 | 0.55 |
| **SGD** | {'alpha': 0.001, 'fit_intercept': False, 'loss': 'modified_huber', 'max_iter': 10, 'penalty': 'l2', 'shuffle': True, 'verbose': 1} | 0.99 | 0.62 | 0.58 |


***

<a name="conclusion"></a>
<img src="https://docs.google.com/drawings/d/e/2PACX-1vSI-wVz8FiywYD0jmZKNmvImS15p4hrgyjLLQhfRt47MC3Lcoen50Iv_LTG2SE6SyRrRSmYkuq9JfMM/pub?w=927&amp;h=272">
[[Back to top](#top)]

***

Goal was accomplished, I more than doubled the baseline of 28% by using the SGD Model increasing the prediction rate to 58%.