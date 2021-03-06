# Entity Ranking with Logistic Regression

This project is for Baidu's entity search competition:

* Rules are [here](http://nlpcc.baidu.com/rules.html)
* Data are in data/ directory.

We are the Chitu team! -- [Link](http://fifigithub.github.io/Baidu-entity-search-competition) to project page.

## Features

1. Awesome error analysis interface ([demo](http://www.xuehuichao.com/error_analysis.html)). 
   * Viewing queries the system performed the worst
   * Summarize performance on held-out as well as cross-validation data
2. Easy to implement new feature extractors
   * Write a function, and decorate it with @Extractor

      ```python
      @Extractor
      def HasCommonCharacter((q_type, query), entity):
        return [("HAS_COMMON_CHAR", int(len(set(query).intersection(set(entity)) != 0)))]
      ```
3. Scripts to download data
   * Multi-thread downloading Baidu Baike data (in small_scripts/)

## Usage
### 10-fold cross validation experiment
The following command runs 10-fold cross validation, and tests on hold-out set.
```sh
python run_experiment.py --extractors=e1,e2...
```
Output:
```
Experiment ID: 101. Detailed report at reports/101.html. Model at models/101.model

Cross validation:
| task1      | task2     | ...
| 0.70+-0.01 | 0.80+-0.02| ...

Hold-out set:
| task1    | task2    | ...
| 0.70     | 0.80     | ...

```

### Listing Past Experiment Results
Listing past experiment result overview. For each feature set, we only list the last run.

```sh
python experiments_summary.py
```
Output:
```
  ID  features                                                 MAP    Celebrity     Movie    Restaurant    tvShow  time                                                                                      
  ----  --------------------------------------------------  --------  -----------  --------  ------------  --------  --------------------------                                                                
     7  nchar,char,nsumchar,sumchar,cont_bigram,cont_match  0.460184     0.822061  0.379818      0.277135  0.361721  2016-04-06 17:14:28.113651                                                                
     6  nchar                                               0.276946     0.360595  0.331004      0.259918  0.156267  2016-04-06 16:34:30.566979
```

### Export Submission Output
```sh
python export_result.py --model_id=101 --test_dir=data/DEV\ SET --output_dir=output
```
