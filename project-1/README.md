# Project 1: Standardized Test Analysis (ACT or SAT)

<br>

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

### `scores` DataFrame

  | Feature | Type | Dataset | Description | Remarks |
  | --- | --- | --- | --- | --- |
  | state | string | {ACT,SAT} scores {2017,2018,2019} | A state in the US | Puerto Rico and Virgin Islands are intentionally excluded, as they only appear in SAT 2019. |
  | state_code | string | {ACT,SAT} scores {2017,2018,2019} | A two-letter US state abbreviation | This is primarily needed for choropleth map plotting. |
  | test | string | {ACT,SAT} scores {2017,2018,2019} | A test type | Either "ACT" or "SAT" |
  | year | integer | {ACT,SAT} scores {2017,2018,2019} | A test year | Either 2017, 2018, or 2019 |
  | part | float | {ACT,SAT} scores {2017,2018,2019} | A participation rate of eligible students in a given test. | Units in percent, i.e. 98.1 means 98.1% |
  | score | float | {ACT,SAT} scores {2017,2018,2019} | An average test score for a given state | Composite for ACT, total for SAT |
  | pctl | integer | {ACT,SAT} scores {2017,2018,2019} | The corresponding percentile to the average test score for a given state | The SAT percentiles are exclusively referred to SAT User Percentiles, which are based on only actual SAT scores of students in the classes of 2016 - 2020 who took the new SAT. |
  | metric | float | {ACT,SAT} scores {2017,2018,2019} | A score metric used as a normalized score percentile | The calculation formula for this value is:<br>     `participation rate` * `score percentile` / 100 |


## Analysis & Interpretation

1. The ACT and SAT scores are based on different scales and hence, cannot be directly compared. We need to transform them into percentiles, which have the common range of 0 to 100. Below are the official score to percentile transformation tables from the ACT and SAT respectively

<img src="./images/act-percentiles.png" width="640">

source : <a href="https://www.act.org/content/dam/act/unsecured/documents/MultipleChoiceStemComposite.pdf">https://www.act.org/content/dam/act/unsecured/documents/MultipleChoiceStemComposite.pdf</a>


<img src="./images/sat-percentiles.png" width="640">

source : <a href="https://blog.prepscholar.com/historical-percentiles-new-sat">https://blog.prepscholar.com/historical-percentiles-new-sat</a>

2. The ACT and SAT score percentiles distinctly differ across different geographical areas.

<img src="./images/percentiles-by-state.png" height="360">

3. It is found that participation rates and score percentiles are negatively correlated.

<img src="./images/participation-percentile-correlations.png" height="360">

4. To normalize score percentiles, a score metric is calculated from

```
participation rate * score percentile / 100
```

5. After normalization, it can be shown that the mainland states perform higher overall on the ACT, whereas the coastal regions, especially on the east, perform better on the SAT.

<img src="./images/metrics-by-state.png" height="360">

6. Based on the latest data available in 2019, students in the mainland states are advised to take the ACT while students in the coastal regions should take the SAT.

<img src="./images/recommended-tests.png" height="360">

## Conclusions & Recommendations

After the normalization of score percentiles, the study has found that students in the mainland states perform the best overall in the ACT, whereas the coastal states have the higher overall scores on the SAT. Unless students are required by their states to take one of the two tests, they are advised to take the test in which students in their states perform best overall. Apart from a higher probability to score a higher percentile, it could be suggested that learning environments and resources in that state such as schools, tuition institutions, and mock test materials are more suitable, accessible, and ready to assist students to achieve a higher score on that test. As a general rule of thumb, students in the mainland states are recommended to take the ACT and students in the coastal regions are recommended to take the SAT.
