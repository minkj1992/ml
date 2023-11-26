# Made with ML
> https://madewithml.com/

## init

```bash
> git clone https://github.com/GokuMohandas/Made-With-ML.git .
> git remote set-url origin https://github.com/minkj1992/Made-With-ML.git
> git checkout -b dev

> pyenv virtualenv 3.10.11 madewithml
> pyenv local madewithml
> python -m pip install --upgrade pip 
> python3 -m pip install -r requirements.txt
```

## 1. Design

[madewithml](https://madewithml.com/courses/mlops/preparation/)에서 Desin파트를 읽었다. Product과 System Design을 통해서 문제정의와 constraint를 만들어내고, ray를 통해서 병렬 학습및 slicing, augment등을 가능하게 하며

System Design에서는 

1. Data
    1. training
        1. training.csv
        2. holdout.csv (testing)
    2. production
        1. batches
        2. real-time streams
2. Labeling
    1. multi label
    2. one label(one category)
3. Metrics
    1. f1 score
4. Evaluation
    1. offline evaluation
        1. gold standard holdout dataset for benchmark
        2. slice of data (by ray)
    2. online evaluation
        1. ensure our model perform well in production
        2. model
            1. Proxy model - internal canary rollout, (champion vs challenger performance)
            2. Rollout
            3. A/B rollout
5. Modeling
6. Inference
    1. (Offline) Batch inference
    2. Online Inference
7. Feedback
    1. human-in-loop checks
    2. users to report issues
   
## 2. Data

### 2.1. Data preparation
1. train, val, test
2. val는 train set에서 Kfold로 divide해서 train loop 마다 validation을 수행한다.
3. 하지만 기존의 kfold는 데이터 셋의 분포가 하나의 라벨에 몰려있거나 하는 등의 문제가 발생했다.
4. 이를 위해서 sklearn의 `StratifiedKFold`를 사용하며, 이는 fold별 라벨 분포를 맞춰준다.
5. 마지막으로 test를 (0.2)로 비율을 잡았다면, * 4(train_size / test_size)를 해줘서 테스트의 크기를 맞춰준다.

### 2.2. EDA (Exploratory Data Analysis)
> Understand signal and nuances of dataset.

### 2.3. Preprocessing

1. `Preparation`, organizing and cleaning the data
2. `Transformation`, transforming the data involves feature encoding
    1. Scaling
        1. [standardization](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn.preprocessing.StandardScaler): rescale values to mean 0, std 1
        2. [min-max](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.minmax_scale.html#sklearn.preprocessing.minmax_scale), rescale values between a min and max.
        3. [binning](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.KBinsDiscretizer.html), continuoust -> categorical
            1. `uniform`, `quantile`, `kmeans`
    2. Encoding
    3. Extraction
3. `Implementation`, 