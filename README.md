# Performance of Brazilian students at the ENEM 2018
Authors: Sophia Moyen & Emanuel Iwanow \
Date: 30.07.2024

The notebook provides a deep dive into the 2018 ENEM dataset as well as hypothesis testing analysis and a prediction model for the scores.

### Summary

* [1. Introduction](#1)
    * [1.1 Motivation and Objective](#11)
    * [1.2 About the Dataset](#12)
    * [1.3 Problem Statement](#13)
* [2. Preprocessing](#2)
    * [2.1 Choosing which columns to import](#21)
    * [2.3 Dealing with misssing values](#22)
    * [2.4 Checking for duplicates and outliers](#23)
* [3. Exploratory Data Analysis](#3)
* [4. Feature Extraction](#4)
* [5. Hypothesis Testing](#5)
* [6. Modelling and Prediction](#6)
* [7. Conclusion](#7)
* [References](#8)


# 1. Introduction <a class="anchor" id="1"></a>
## 1.1. Motivation and Objective <a class="anchor" id="11"></a>
-  ### What is the ENEM?
ENEM (Exame Nacional do Ensino Médio) is a non-mandatory, **standardized national exam** held in Brazil for **college admissions**. Entry to federal universities, considered the best in the country, takes place solely based on the results of this test. More and more non-federal public universities and private universities have also accepted the results of this test as a form of admission. On the matter of relevevance, in 2016, there were 8.6 million people signed up to take it, which makes it the second largest in the world after China's National Higher Education Entrance Examination.

- ###  How is the ENEM structured?
The ENEM is a two-day exam held annually in November, simultaneously across the entire country. Each day of exam lasts 5 hours. The exam consists of multiple 180 multiple choice questions and one essay. A special system of normalization is applied, called **TRI** (Teoria de Resposta ao Item). Following the TRI, each multiple choice question has a different value, calculated after normalization across the performance of all candidates to **avoid awarding students who "guessed" the right question**. A question that only a few candidates got right will have a higher value, but only if the candidate that got that question right also got "easier" questions right. This follows the logic that if a candidate is able to solve a complicated question, he/she should also have been able to solve an "easy" one, that many people got right. What in practice happens, e.g, it that two candidates may have scored exactly 144 questions out of the 180 and have had the same grade on the essay, but their final grades may be completely different because of the TRI. 

The TRI normalization also means that every year the maximum and minimum punctuation for each subject can only be known after the results, as shown above for the year 2018. For example,in a certain year the maximum grade in Maths could be 850 and the other year 950. But the maximum range is always around 800-1100. This is of course doesn't apply to the critical essay grading, that always ranges betweeen 0 and 1000. 
    
The **implementation of this mathematical "trick"** is an important part of the exam and gives the standardized test score **more credibility** to evaluate the candidate knowledge and, in our case, analyse relationships between the candidates' grades and other socioeconomical features.

- ### Why is analysing the ENEM relevant?
Beyond its role in selecting students for universities, the ENEM offers a rich dataset to investigate the interplay between **socioeconomic factors** and **academic performance**.

Understanding the socioeconomic determinants of ENEM performance is crucial for several reasons. First, it allows for a comprehensive assessment of the exam’s fairness and its ability to accurately measure students’ potential. Second, by identifying socioeconomic gaps in performance, **policymakers and educators** can develop targeted strategies to **mitigate inequalities and provide more educational opportunities** for disadvantaged students. Finally, exploring the predictive power of the ENEM in relation to a candidate's probability of success can inform the use of the exam as a selection tool for higher education.

## 1.2. About the Dataset <a class="anchor" id="12"></a>

The Brazilian Ministry of Education provides the microdata of the each year's event, [available in their website](https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/microdados/enem). We were specially interested in analysing the ENEM from the year 2018 because that was the year in which we wrote the exam ourselves.

<div class="alert alert-info">
<strong><div style="color: rgb(0, 0, 0);">📌  Important infos:</div></strong> <br>

<div style="color: rgb(0, 0, 0);">→ In 2018, there were 5.5 million people taking the exam. Each row of the dataset corresponds to a candidate, we have 5513733 rows;</div>
<div style="color: rgb(0, 0, 0);">→ The dataset has 78 columns of information about each candidate;</div>
<div style="color: rgb(0, 0, 0);">→ The data has been anonimized such that no candidate can be identified by the microdata. To make this possible, a few features were transformed or removed. The candidate's place of birth and residence were removed and the age variable was replaced by age range. Some specifics about special application needs were also removed.;</div>
<div style="color: rgb(0, 0, 0);">→ Aside from the candidate's information related to the exam, a 27-question-socioeconomical questionnaire was also collected at the time of enrollment. Including questions related to the candidate's residence, parents and financial situation;</div>
</div>


All data has been collected was stored either as **numerical or alphanumerical** variables. A dictionary was provided to identify the corrspondance of the numbers/letters. For example, the column `TP_COR_RACA` stores the candidate's ethnicity as numerals. The column `Q024`, that corresponds the 24th question of the socioeconomical questionnaire, categorizes the answers with letters from A to E:

<table>
<tr><th> TP_COR_RACA </th><th> Q024 </th></tr>
<tr><td>

| Ethnicity | Variables |
| --- | --- |
| Not informed | 0 |
| White | 1 |
| Black | 2 |
| Pardo | 3 |
| Yellow | 4 |
| Indiginous | 5 |

</td><td>

| Do you have a computer at home? | Variables |
| --- | --- |
| No | A |
| Yes, one | B |
| Yes, two | C |
| Yes, three | D |
| Yes, four or more | E |

</td></tr> </table>



## 1.3. Problem Statement <a class="anchor" id="13"></a>

The microdata from the ENEM 2018 provides us many features related to each candidate, which enables us to investigate many tendencies regarding the exam. Our main goal will be to understand what factors influence a good grade at the exam and how socioeconomical factors influence a candidate's results. We can bring our goals for this report down to the following points:

1. **Exploratory Data Analysis**: Who are the ENEM 2018 candidates? How did they perform in the exam?
2. **Feature Extraction**: What features are important to predict a candidate's grade?
3. **Statistical Hypothesis Testing**: Based on the feature extraction, does feature X really influence a candidate's grade or is it just chance?
4. **Modeling and Prediction**: Can we create a model that is able to predict a candidate's grade? How well does it perform?

# >>> For the complete analysis, check out our notebook! <<
...
# Here is our conclusion <a class="anchor" id="7"></a>

<div class="alert alert-info">
<strong><div style="color: rgb(0, 0, 0);">📌  Insights:</div></strong> <br>
<div style="color: rgb(0, 0, 0);">→ After the EDA and the hypothesis testing, it is left very clear how much a candidate's family income is determinant to their performance in the ENEM. The ENEM is the main college admission exam in the country and the strong correlation that we saw between income and grade translates into awarding a more wealthy candidate a spot in university and leaving a poorer one out, perpetuating the poverty cycle. A  measure that universities are currently using to overcome this trend and spot the outstanding candidates within their minority groups is through minority quotas for college admissions for public school students and for ethnical minorities. This is however not a long-term measure and we hope more investment in public schools and base education in Brazil can bring equal opportunities among all candidates so that we don't see any correlation between income and performance anymore!;</div>
<div style="color: rgb(0, 0, 0);">→ We could select the most imporant features for determining a candidate's average grade in the ENEM with a relatively low error (68 points from around 1000)</div>
</div>

<div class="alert alert-danger">
<strong><div style="color: rgb(0, 0, 0);">📌  Challenges and Future Work:</div></strong> <br>
<div style="color: rgb(0, 0, 0);">→ Because of lack of computational power and memory, we did the analysis on only a portion of the whole daatset (5.5mi). It is not ideal and may have led to worse results and misleading analysis.;</div>
<div style="color: rgb(0, 0, 0);">→ We didn't have time to test different regression methods and make a comparative study between them. That would have been very interesting.</div>
<div style="color: rgb(0, 0, 0);">→ We dropped many columns or rows to solve missing data issues. That is not the best way to do it in production, specially when the data is as meaningful as what we have here (already treated data). </div>
<div style="color: rgb(0, 0, 0);">→ The hypothesis testing was 1uite obvious, but still an rigorous statistical approach to avoid being biased towards an obvious direction. We could however test other less obvious things in our analysis, such as difference in performance based on age and what could that mean. </div>
<div style="color: rgb(0, 0, 0);">→ A very meaningul analysis would include crossing the data with the schools where the candidates are from (how many teachers, how much ressources, how many students in the school) and also the sity in which the school is located (HDI, population size, income per capta, sanitation data, public transportation, healthcare) </div>
</div>

# References <a class="anchor" id="8"></a>

The contents for the **Hypothesis Testing** were taken from the tutorials from the lecture:

> Debes, C. (2024, Summer). Data Science I - 18-zo-2110-vl. Lecture course. Technische Universität Darmstadt, Darmstadt, Germany.

We were inspired by the **visual features** used in SmartPay notebook from Felipe Gomes available in Kaggle:
> Gomes, F. (2022). Smartpay - Calculadora de Salário Rio (GAMLSS.nb). [Kaggle](https://www.kaggle.com/code/gomes555/smartpay-calculadora-de-sal-rio-gamlss-nb)

**Dataset**: The public data from the Brazilian Ministry of Education for the microdata for the ENEM 2018:
> Instituto Nacional de Estudos e Pesquisas Educacionais Anísio Teixeira. Microdados do Enem 2018. 
[online]. Brasília: Inep, 2018: <http://portal.inep.gov.br/web/guest/microdados>

**Python libraries**
> - **Seaborn**: Waskom, M. L., (2021). seaborn: statistical data visualization. Journal of Open Source Software, 6(60), 3021, https://doi.org/10.21105/joss.03021.
> - **Matplotlib**: J. D. Hunter, "Matplotlib: A 2D Graphics Environment", Computing in Science & Engineering, vol. 9, no. 3, pp. 90-95, 2007.
> - **Sklearn**>Scikit-learn: Machine Learning in Python, Pedregosa et al., JMLR 12, pp. 2825-2830, 2011.
> - **Numpy**: Harris, C.R., Millman, K.J., van der Walt, S.J. et al. Array programming with NumPy. Nature 585, 357–362 (2020). DOI: 10.1038/s41586-020-2649-2. (Publisher link).
> - **Pandas**: McKinney, W., & others. (2010). Data structures for statistical computing in python. In Proceedings of the 9th Python in Science Conference (Vol. 445, pp. 51–56).
