# Readme XG boost

## Training pipeline 

The XGboost training pipeline can be found in [training/pipeline.py](https://github.com/teamdatatonic/kfp-template-0/blob/839fe7e5ec7269d43ffd953e99d55d0d7bc456b7/pipelines/xgboost/training/pipeline.py) . In the training pipeline, the component [custom_train](https://github.com/teamdatatonic/kfp-template-0/blob/feature/ml-pipelines-documentation/pipelines/kfp_components/aiplatform/custom_train.py) creates a custom training job by specifying the setting that Vertex AI needs, `machine_params`, to run the python training script. 

The training phase is preceded by a preprocessing phase where different transformations are applied to the training and evaluation data using SKlearn preprocessing functions. The **Preprocessing** step and the **training** step define the two components of the SKlearn pipline as shown in the graph below.

![Training process](sklearn_pipline.png)

## Preprocessing with SKlearn
The 3 data transformation steps considered in the `train.py` script are :

- Centering and scaling numerical values using [StandardScaler()](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html) function. In the exemple the scaled features are denoted `num_feats` : 

   - `dayofweek`
   - `hourofday`
   - `trip_distance`
   - `trip_miles`
   - `trip_seconds`
   
                                               
         
- Encoding a chosen subset of categorical features as a one-hot numeric array using the [OneHotEncoder()](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html) function. In the exemple, the OneHot encoded features are denoted `cat_feats_onehot` :

   - `payment_type`


- Encoding a chosen subset of categorical features as an integer array using the [OrdinalEncoder()](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OrdinalEncoder.html) function. In the exemple, the ordinal encoded features are denoted `cat_feats_ordinal` :

   - `company`

More processing steps can be included to the pipline. For more details consult [SKlearn.preprocessing](https://scikit-learn.org/stable/modules/preprocessing.html) webpage.

## Training the model : About the XGBoost model

For the regression problem, In the exemple case predicting the total fare of a taxi trip in Chicago, XGBRegressor is used with a set of hyperparameteres defined in [training/pipline.py](https://github.com/teamdatatonic/kfp-template-0/blob/839fe7e5ec7269d43ffd953e99d55d0d7bc456b7/pipelines/xgboost/training/pipeline.py) under the variable  `model_params`.

### Model Hyperparameters
You can specify different hyperparameters through the `model_params` argument of `train_xgboost_model`, including:


  - `Booster` : The type of booster. `gbtree` is a tree based booster used as default
  - `max_depth` : the depth of each tree
  - `Objective` : Equivqlent to the loss function. Squared loss, `reg:squarederror`, is set as default.
  - `min_split_loss` : Minimum loss reduction required to make a further partition on a leaf node of the tree.

More hyperparameters can be used to customize your training. For more details consult the [XGBoost_documentation](https://xgboost.readthedocs.io/en/stable/parameter.html)

### Model artifacts

Two modeling artifacts are generated when we run the training Job : 

`Model.joblib` : The model is exported to GCS file as a [joblib](https://joblib.readthedocs.io/en/latest/why.html#benefits-of-pipelines) object.
`Eval_result` : The evaluation metrics are exported to GCS as JSON file.



