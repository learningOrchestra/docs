# /api/learningOrchestra/v1/builder

The `builder` resource join several steps of machine learning workflow (transform, tune, train and evaluate) acoupling in a unique resouce, `builder` creates several model predictions using your own modeling code using a defined set of classifiers. 

## Create a builder

`POST CLUSTER_IP/api/learningOrchestra/v1/builder`

```json
{
    "train_filename": "train_filename",
    "test_filename": "test_filename",
    "modeling_code": "Python3 code to preprocessing, using Pyspark library",
    "classifiers_list": "String list of classifiers to be used"
}
```

### List of Classifiers

* `lr`: LogisticRegression
* `dt`: DecisionTreeClassifier
* `rf`: RandomForestClassifier
* `gb`: Gradient-boosted tree classifier
* `nb`: NaiveBayes

To send a request with LogisticRegression and NaiveBayes Classifiers:

```json
{
    "train_filename": "train_filename",
    "test_filename": "test_filename",
    "modeling_code": "Python3 code to preprocessing, using Pyspark library",
    "classifiers_list": ["lr", "nb"]
}
```

### modeling_code environment

The python 3 modeling code must use the environment instances in bellow:

* `training_df` (Instantiated): Spark Dataframe instance training filename
* `testing_df`  (Instantiated): Spark Dataframe instance testing filename

The modeling code must instantiate the variables in below, all instances must be transformed by pyspark VectorAssembler:

* `features_training` (Not Instantiated): Spark Dataframe instance for train the model
* `features_evaluation` (Not Instantiated): Spark Dataframe instance for evaluating trained model accuracy
* `features_testing` (Not Instantiated): Spark Dataframe instance for testing the model

In case you don't want to evaluate the model, set `features_evaluation` as `None`.

#### Handy methods

```python
self.fields_from_dataframe(self, dataframe, is_string)
```
This method returns string or number fields as a string list from a DataFrame.

* `dataframe`: DataFrame instance
* `is_string`: Boolean parameter, if `True`, the method returns the string DataFrame fields, otherwise, returns the numbers DataFrame fields.

#### modeling_code Example

This example uses the [titanic challengue datasets](https://www.kaggle.com/c/titanic/overview).

```python
from pyspark.ml import Pipeline
from pyspark.sql.functions import (
    mean, col, split,
    regexp_extract, when, lit)

from pyspark.ml.feature import (
    VectorAssembler,
    StringIndexer
)

TRAINING_DF_INDEX = 0
TESTING_DF_INDEX = 1

training_df = training_df.withColumnRenamed('Survived', 'label')
testing_df = testing_df.withColumn('label', lit(0))
datasets_list = [training_df, testing_df]

for index, dataset in enumerate(datasets_list):
    dataset = dataset.withColumn(
        "Initial",
        regexp_extract(col("Name"), "([A-Za-z]+)\.", 1))
    datasets_list[index] = dataset

misspelled_initials = [
    'Mlle', 'Mme', 'Ms', 'Dr',
    'Major', 'Lady', 'Countess',
    'Jonkheer', 'Col', 'Rev',
    'Capt', 'Sir', 'Don'
]
correct_initials = [
    'Miss', 'Miss', 'Miss', 'Mr',
    'Mr', 'Mrs', 'Mrs',
    'Other', 'Other', 'Other',
    'Mr', 'Mr', 'Mr'
]
for index, dataset in enumerate(datasets_list):
    dataset = dataset.replace(misspelled_initials, correct_initials)
    datasets_list[index] = dataset


initials_age = {"Miss": 22,
                "Other": 46,
                "Master": 5,
                "Mr": 33,
                "Mrs": 36}
for index, dataset in enumerate(datasets_list):
    for initial, initial_age in initials_age.items():
        dataset = dataset.withColumn(
            "Age",
            when((dataset["Initial"] == initial) &
                 (dataset["Age"].isNull()), initial_age).otherwise(
                    dataset["Age"]))
        datasets_list[index] = dataset


for index, dataset in enumerate(datasets_list):
    dataset = dataset.na.fill({"Embarked": 'S'})
    datasets_list[index] = dataset


for index, dataset in enumerate(datasets_list):
    dataset = dataset.withColumn("Family_Size", col('SibSp')+col('Parch'))
    dataset = dataset.withColumn('Alone', lit(0))
    dataset = dataset.withColumn(
        "Alone",
        when(dataset["Family_Size"] == 0, 1).otherwise(dataset["Alone"]))
    datasets_list[index] = dataset


text_fields = ["Sex", "Embarked", "Initial"]
for column in text_fields:
    for index, dataset in enumerate(datasets_list):
        dataset = StringIndexer(
            inputCol=column, outputCol=column+"_index").\
                fit(dataset).\
                transform(dataset)
        datasets_list[index] = dataset


non_required_columns = ["Name", "Embarked", "Sex", "Initial"]
for index, dataset in enumerate(datasets_list):
    dataset = dataset.drop(*non_required_columns)
    datasets_list[index] = dataset


training_df = datasets_list[TRAINING_DF_INDEX]
testing_df = datasets_list[TESTING_DF_INDEX]

assembler = VectorAssembler(
    inputCols=training_df.columns[:],
    outputCol="features")
assembler.setHandleInvalid('skip')

features_training = assembler.transform(training_df)
(features_training, features_evaluation) =\
    features_training.randomSplit([0.8, 0.2], seed=33)
features_testing = assembler.transform(testing_df)
```

## List builder dataset content

`GET CLUSTER_IP/api/learningOrchestra/v1/builder/<filename>?skip=number&limit=number&query={}`

Returns rows of the dataset requested, with pagination.

* `filename` - Name of file requests
* `skip` - Amount of lines to skip in the CSV file
* `limit` - Limit the query result, maximum limit set to 20 rows
* `query` - Query to find documents, if only pagination is requested, `query` should be empty curly brackets `query={}`

The first row in the query is always the metadata file.


## List all builder datasets metadata

`GET CLUSTER_IP/api/learningOrchestra/v1/builder`

Returns an array of builder datasets metadata, where each dataset contains a metadata file.

### Builder datasets metadata

```json
{
    "classifier": "nb",
    "filename": "titanic_test_nb",
    "finished": true,
    "parent_filename": ["titanic_train", "titanic_test"],
    "time_created": "2020-11-04T10:51:43-00:00",
    "type": "builder"
}
```

* `classifier` - The classifier used to make the predictions
* `parent_filename` - The train and test `filenames` used to make the builder dataset, from which the current dataset is derived.
* `filename` - Name of the file
* `finished` - Flag used to indicate if asynchronous processing from file downloader is finished
* `type` - The type of file
* `time_created` - Time of creation

## Delete a builder dataset

`DELETE CLUSTER_IP/api/learningOrchestra/v1/builder/<filename>`

Request of type `DELETE`, passing the `filename` field of a builder dataset in the request parameters, deleting the dataset.
