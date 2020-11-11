# /api/learningOrchestra/v1/explore/pca

PCA is used to decompose a multivariate dataset in a set of successive orthogonal components that explain a maximum amount of the variance. 

In `scikit-learn` (used in this microservice), PCA is implemented as a transformer object that learns components in its fit method, and can be used on new data to project it on these components.

This is often useful if the models down-stream make strong assumptions on the isotropy of the signal: this is for example the case for Support Vector Machines with the RBF kernel and the K-Means clustering algorithm, more information about this algorithm in [scikit-learn PCA docs](https://scikit-learn.org/stable/modules/decomposition.html#pca).

The `pca` resource uses the PCA method on a dataset, and create a plot image from result.

## Create a PCA image plot

`POST CLUSTER_IP/api/learningOrchestra/v1/explore/pca`

The body contains the json fields:

```json
{
    "inputDatasetName": "used dataset name",
    "outputPlotName": "result image plot name",
    "label": "dataset label column"
}
```

The `label` is the label name column for machine learning datasets which has labeled tuples. In the case that the dataset used doesn't contain labeled tuples, define the value as null type in JSON:

```json
{
    "inputDatasetName": "used dataset name",
    "outputPlotName": "result image plot name",
    "label": null
}
```

## Read the plot names of the created images

`GET CLUSTER_IP/api/learningOrchestra/v1/explore/pca`

Returns a list with all created image plot names.
 
## Read an image plot

`GET CLUSTER_IP/api/learningOrchestra/v1/explore/pca/<plotName>`

Returns the image plot of `plotName` specified.

## Delete an image plot

`DELETE CLUSTER_IP/api/learningOrchestra/v1/explore/pca/<plotName>`

Deletes an image plot from the database.

### Images plot examples

This examples use the [titanic challengue datasets](https://www.kaggle.com/c/titanic/overview).

#### Titanic Train dataset

![](./pca_titanic_train.png)

#### Titanic Test dataset

![](./pca_titanic_test.png)
