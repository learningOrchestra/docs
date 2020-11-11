# /api/learningOrchestra/v1/dataset

The **dataset** resource is used to manages datasets, datasets are downloaded in CSV format and parsed into JSON format where the primary key for each document is the datasetName field contained in the JSON file.

## Downloads dataset from URL

`POST CLUSTER_IP/api/learningOrchestra/v1/dataset`

Insert a CSV into the database using the POST method, JSON must be contained in the body of the HTTP request.
The following fields are required: 

```json
{
    "datasetName": "key to dataset identification",
    "url": "http://sitetojson.file/path/to/csv"
}
```

## List dataset content

`GET CLUSTER_IP/api/learningOrchestra/v1/dataset/<datasetName>?skip=number&limit=number&query={}`

Returns rows of the dataset requested, with pagination.

* `datasetName` - Name of file requests
* `skip` - Amount of lines to skip in the CSV file
* `limit` - Limit the query result, maximum limit set to 20 rows
* `query` - Query to find documents, if only pagination is requested, `query` should be empty curly brackets `query={}`

The first row in the query is always the metadata file.


## List all datasets metadata

`GET CLUSTER_IP/api/learningOrchestra/v1/dataset`

Returns an array of datasets metadata, where each dataset contains a metadata file.

### Datasets metadata

```json
{
    "fields": [
        "PassengerId",
        "Survived",
        "Pclass",
        "Name",
        "Sex",
        "Age",
        "SibSp",
        "Parch",
        "Ticket",
        "Fare",
        "Cabin",
        "Embarked"
    ],
    "datasetName": "titanicTraining",
    "finished": true,
    "type": "dataset",
    "timeCreated": "2020-07-28T22:16:10-00:00",
    "url": "https://filebin.net/rpfdy8clm5984a4c/titanic_training.csv?t=gcnjz1yo"
}
```

* `fields` - Names of the columns in the file
* `datasetName` - Name of the file
* `finished` - Flag used to indicate if asynchronous processing from file downloader is finished
* `type` - The type of file
* `timeCreated` - Time of creation
* `url` - URL used to download the file

## Delete a dataset

`DELETE CLUSTER_IP/api/learningOrchestra/v1/dataset/<datasetName>`

Request of type `DELETE`, passing the `datasetName` field of an existing dataset in the request parameters, deleting the dataset in the database.
