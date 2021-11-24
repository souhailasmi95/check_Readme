# Readme XG boost

## Training pipeline 

The XGboost training pipeline can be found in [training/pipeline.py] . In the training pipeline, the component custom train creates a custom training job by specifying the setting that Vertex AI needs to run the python training script. 

The training phase is preceded by a preprocessing phase where different transformations are applied to the training and evaluation data using SKlearn preprocessing functions.

## Preprocessing with SKlearn
The 3 transformations considered in the [train.py] script are. The features are divided into 3 sets subject to the transformation:

* Centering and scaling numerical values using [StandardScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html) function. In the exemple the scaled features are denoted num_feats: 

                     -  "dayofweek",
                     -  "hourofday",
                     -  "trip_distance",
                     -  "trip_miles",
                     -  "trip_seconds",
                                 
                                               
         
* Encoding a chosen subset of categorical features as a one-hot numeric array using the [OneHotEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html) function. In the exemple, the OneHot encoded features are denoted cat_feats_onehot:
                     - "payment_type"


* Encoding a chosen subset of categorical features as an integer array array using the [OrdinalEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OrdinalEncoder.html) function. In the exemple, the OneHot encoded features are denoted cat_feats_ordinal:
                     - "company"



## About the XGBoost model

Vertex AI settings 
The settings needed to create the Custom Training Job :
The python Script : train.py


