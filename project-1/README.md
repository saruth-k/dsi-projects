# Project 1: Standardized Test Analysis (ACT or SAT)

## Contents

- [Problem Statement](#Problem-Statement)
- [Datasets](#Datasets)
- [Data Dictionary](#Data-Dictionary)
- [Analysis & Interpretation](#Analysis-&-Interpretation)
- [Conclusions & Recommendations](#Conclusions-&-Recommendations)

## Problem Statement

Pursuing higher education is probably one of the toughest moments in life, especially when the made decision can greatly dictate the student's future. While many high school students have a clear aim of their aspired colleges, some struggle even to choose between the ACT or SAT for their applications. This data-centric study is carried out to explore students' performances by state on each standardized test, and tries to provide an elementary guideline whether a typical average student should focus on the ACT or SAT.

## Datasets

Out of the 10 datasets included in the [`data`](../data/) folder, 6 are used in this project along with 2 additional datasets from external sources. 

### Included Sources

- [`act-scores-2017.csv`](../data/act-scores-2017.csv) : 2017 ACT scores by state
- [`act-scores-2018.csv`](../data/act-scores-2018.csv) : 2018 ACT scores by state
- [`act-scores-2019.csv`](../data/act-scores-2019.csv) : 2019 ACT scores by state


- [`sat-scores-2017.csv`](../data/sat-scores-2017.csv) : 2017 SAT scores by state
- [`sat-scores-2018.csv`](../data/sat-scores-2018.csv) : 2018 SAT scores by state
- [`sat-scores-2019.csv`](../data/sat-scores-2019.csv) : 2019 SAT scores by state

### External Sources

- [`act-percentiles.csv`](../data/act-percentiles.csv)
  
  ACT score percentiles based on ACT-tested high school graduates from 2017, 2018, and 2019 and reported on score reports during 2019 - 2020  
  
  - retrieved from : https://www.act.org/content/act/en/products-and-services/the-act/scores/national-ranks.html
  - original source : https://www.act.org/content/dam/act/unsecured/documents/MultipleChoiceStemComposite.pdf


- [`sat-percentiles.csv`](../data/sat-percentiles.csv)
  
  SAT User Percentiles, which are based on only actual SAT scores of students in the classes of 2016 - 2020 who took the new SAT  
  
  - retrieved from : https://blog.prepscholar.com/historical-percentiles-new-sat
  - original sources
  
    - https://collegereadiness.collegeboard.org/pdf/understanding-sat-scores.pdf
    - https://web.archive.org/web/20191211232552/https://collegereadiness.collegeboard.org/pdf/understanding-sat-scores.pdf
    - https://web.archive.org/web/20190412042651/https://collegereadiness.collegeboard.org/pdf/understanding-sat-scores.pdf
    - https://web.archive.org/web/20180827023524/https://collegereadiness.collegeboard.org/pdf/understanding-sat-scores.pdf
    - https://web.archive.org/web/20161128224433/https://collegereadiness.collegeboard.org/pdf/understanding-sat-scores-2016.pdf

## Data Dictionary
---

### `{act|sat}_scores_{2017|2018|2019}` DataFrames

| Feature | Type | Dataset | Description | Remarks |
| --- | --- | --- | --- | --- |
| state | string | {ACT,SAT} scores {2017,2018,2019} | A state in the US | Puerto Rico and Virgin Islands are intentionally excluded, as they only appear in the SAT 2019. |
| particpation | float | {ACT,SAT} scores {2017,2018,2019} | A participation rate of eligible students in a given test. | Units in percent, i.e. 98.1 means 98.1% |
| score | float | {ACT,SAT} scores {2017,2018,2019} | An average test score for a given state | Composite score (1 - 36) for the ACT.<br>Total score (400 - 1,600) for the SAT |
| percentile | integer | {ACT,SAT} score percentiles | The corresponding percentile to the average test score for a given state | The SAT percentiles are exclusively referred to SAT User Percentiles, which are based on only actual SAT scores of students in the classes of 2016 - 2020 who took the new SAT. |
| estimate | float | - | An estimated score percentile at 100% participation rates | This value is obtained from the calculation of lines of best fit |

### `differences` DataFrame

| Feature | Type | Dataset | Description | Remarks |
| --- | --- | --- | --- | --- |
| code | string | - | A two-letter US state abbreviation | This is primarily needed for choropleth map plotting. |
| state | string | {ACT,SAT} scores {2017,2018,2019} | A state in the US | Puerto Rico and Virgin Islands are intentionally excluded, as they only appear in the SAT 2019. |
| 2017 | float | - | How much the ACT estimated score percentile is greater then the SAT in 2017 | ACT 2017 estimated score percentile - SAT 2017 estimated score percentile |
| 2018 | float | - | How much the ACT estimated score percentile is greater then the SAT in 2018 | ACT 2018 estimated score percentile - SAT 2018 estimated score percentile |
| 2019 | float | - | How much the ACT estimated score percentile is greater then the SAT in 2019 | ACT 2019 estimated score percentile - SAT 2019 estimated score percentile |

## Analysis & Interpretation
---

1. The ACT and SAT scores are based on different scales and hence, cannot be directly compared. We need to transform them into percentiles, which have the common range of 0 to 100. Below are the official score to percentile transformation tables from the ACT and SAT respectively

<img src="./images/act-percentiles.png" width="640">

source : <a href="https://www.act.org/content/dam/act/unsecured/documents/MultipleChoiceStemComposite.pdf">https://www.act.org/content/dam/act/unsecured/documents/MultipleChoiceStemComposite.pdf</a>


<img src="./images/sat-percentiles.png" width="640">

source : <a href="https://blog.prepscholar.com/historical-percentiles-new-sat">https://blog.prepscholar.com/historical-percentiles-new-sat</a>

2. Even though the ACT remains the more popular choice of standardized tests, it is gradually losing the popularity to the SAT.

<img src="./images/act-participations.png" height="360">

<img src="./images/sat-participations.png" height="360">

<img src="./images/participations-90+.png" height="360">

3. It is found that the participation rates and the score percentiles are negatively correlated across all tests and years.

<img src="./images/correlation-participations-percentiles.png" height="360">

4. Using lines of best fit, estimated score percentiles at the same 100% participation rates are calculated.

<h4 style="font-weight: bold";>Calculation:</h4>

Each line of best fit contains the intercept $b_0$ and the slope $b_1$ respectively. We estimate the slope value to be the score percentile decrease for each 1% increase in participation rates.<br>
Given the real participation rate and score percentile $(x, y)$, the estimated score percentile at 100% participation rate is therefore calculated by:

$$ \large{\hat{y} = y + b_1 (100 - x)} $$

5. Based on the latest data available in 2019, students in almost all US states are advised to take the ACT, as it is expected to yield higher score percentiles.

<img src="./images/recommended-tests-2019.png" height="360">

## Conclusions & Recommendations
---

After the estimation of score percentiles at perfect participations, the study has found that students in almost all states perform best in the ACT, whereas only a select few may favor the SAT. In fact, the ACT yields higher estimated score percentiles in every state in 2017. The trend, however, slowly changes as students in Alaska, Arizona, and Mississippi for instance, produce the higher estimates from the SAT in 2018 and 2019. Nevertheless, the differences are still minimal at best.

To summarize, the ACT remains the preferred standardized test of choice for college admission across most US states, as it apparently yields the higher score percentiles in comparison with its competitor, the SAT. This generally means students can outrank more other candidates and therefore, have a higher chance of successful admission when taking the ACT. Together with the subsidized test fees in some states, there are no concrete reasons to switch to the SAT in such states at this moment. Nonetheless, the result of this study should never be considered definite, for it is simply estimation from very limited data. Each student should consult their education advisors, consider the availability of learning resources, and thoroughly study the differences between the two tests before making a decision. This study result should be used as the last resort after all those contemplations.
