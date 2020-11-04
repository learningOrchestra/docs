# /api/learningOrchestra/v1/explore/histogram

The `histogram` resource makes a histogram from a stored file, storing the resulting histogram in a new dataset.

## Create a histogram from a dataset

`POST CLUSTER_IP/api/learningOrchestra/v1/explore/histogram`

The request is sent in the body, `input_filename` is the filename from used dataset and the `output_filename` is the name of the file in which the histogram result is saved. The `names` is an array with all the fields necessary to make the histogram.

```json
{
    "input_filename": "filename_from_dataset_to_use",
    "output_filename": "filename_to_histogram_dataset",
    "names" : ["list", "of", "fields"]
}
```

## List histogram dataset content

`GET CLUSTER_IP/api/learningOrchestra/v1/explore/histogram/<filename>?skip=number&limit=number&query={}`

Returns rows of the dataset requested, with pagination.

* `filename` - Name of file requests
* `skip` - Amount of lines to skip in the CSV file
* `limit` - Limit the query result, maximum limit set to 20 rows
* `query` - Query to find documents, if only pagination is requested, `query` should be empty curly brackets `query={}`

The first row in the query is always the metadata file.


## List all histogram datasets metadata

`GET CLUSTER_IP/api/learningOrchestra/v1/explore/histogram`

Returns an array of histogram datasets metadata, where each dataset contains a metadata file.

### Histogram datasets metadata

```json
{
    "fields": [
        "PassengerId",
        "Survived",
        "Pclass",
        "Name",
        "Sex"
    ],
    "filename": "titanic_training_histogram",
    "parent_filename": "titanic_training",
    "finished": true,
    "type": "histogram",
    "time_created": "2020-07-28T22:16:10-00:00",
    "url": "https://filebin.net/rpfdy8clm5984a4c/titanic_training.csv?t=gcnjz1yo"
}
```

* `fields` - Names of the columns in the file
* `parent_filename` - The `filename` used to make the histogram dataset, from which the current dataset is derived.
* `filename` - Name of the file
* `finished` - Flag used to indicate if asynchronous processing from file downloader is finished
* `type` - The type of file
* `time_created` - Time of creation
* `url` - URL used to download the file

## Delete a histogram dataset

`DELETE CLUSTER_IP/api/learningOrchestra/v1/explore/histogram/<filename>`

Request of type `DELETE`, passing the `filename` field of an histogram dataset in the request parameters, deleting the dataset.


