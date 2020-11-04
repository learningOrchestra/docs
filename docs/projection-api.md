# /api/learningOrchestra/v1/transform/projection

The `projection` resource makes a projection from dataset inserted in dataset resource, generating a new dataset, and also manages this generated dataset.

## Create a projection dataset from an inserted dataset

`POST CLUSTER_IP:/api/learningOrchestra/v1/transform/projection`

```json
{
    "input_filename": "filename_from_dataset_to_use",
    "output_filename": "filename_to_projection_dataset",
    "names" : ["list", "of", "fields"]
}
```

## List projection dataset content

`GET CLUSTER_IP/api/learningOrchestra/v1/transform/projection/<filename>?skip=number&limit=number&query={}`

Returns rows of the dataset requested, with pagination.

* `filename` - Name of file requests
* `skip` - Amount of lines to skip in the CSV file
* `limit` - Limit the query result, maximum limit set to 20 rows
* `query` - Query to find documents, if only pagination is requested, `query` should be empty curly brackets `query={}`

The first row in the query is always the metadata file.


## List all projection datasets metadata

`GET CLUSTER_IP/api/learningOrchestra/v1/transform/projection`

Returns an array of projection datasets metadata, where each dataset contains a metadata file.

### Projection datasets metadata

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
    "filename": "titanic_training_projection",
    "parent_filename": "titanic_training",
    "finished": true,
    "type": "projection",
    "time_created": "2020-07-28T22:16:10-00:00",
    "url": "https://filebin.net/rpfdy8clm5984a4c/titanic_training.csv?t=gcnjz1yo"
}
```

* `fields` - Names of the columns in the file
* `parent_filename` - The `filename` used to make the projection dataset, from which the current dataset is derived.
* `filename` - Name of the file
* `finished` - Flag used to indicate if asynchronous processing from file downloader is finished
* `type` - The type of file
* `time_created` - Time of creation
* `url` - URL used to download the file

## Delete a projection dataset

`DELETE CLUSTER_IP/api/learningOrchestra/v1/transform/projection/<filename>`

Request of type `DELETE`, passing the `filename` field of an projection dataset in the request parameters, deleting the dataset.


