# /api/learningOrchestra/v1/explore/tsne

The T-distributed Stochastic Neighbor Embedding (t-SNE) is a machine learning algorithm for visualization. 

It is a nonlinear dimensionality reduction technique well-suited for embedding high-dimensional data for visualization in a low-dimensional space of two or three dimensions. 

Specifically, it models each high-dimensional object by a two- or three-dimensional point in such a way that similar objects are modeled by nearby points and dissimilar objects are modeled by distant points with high probability, more information about this algorithm in its [Wiki page]( https://en.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding).

The `tsne` resource uses the t-SNE method on a dataset, and create a plot image from result.

## Create an image plot

`POST CLUSTER_IP/api/learningOrchestra/v1/explore/tsne`

The body contains the json fields:

```json
{
    "inputDatasetName": "used dataset name",
    "outputPlotName": "result image plot name",
    "label": "dataset label column"
}
```

The `label` is the label name of the column for machine learning datasets which has labeled tuples. In the case that the dataset used doesn't contain labeled tuples, define the value as null type in json:

```json
{
    "inputDatasetName": "used dataset name",
    "outputPlotName": "result image plot name",
    "label": null
}
```

## Read the names of the created plots

`GET CLUSTER_IP/api/learningOrchestra/v1/explore/tsne`

Returns a list with all created images plot names.
 
## Read an image plot

`GET CLUSTER_IP/api/learningOrchestra/v1/explore/tsne/<plotName>`

Returns the image plot of the specified plotName.

## Delete an image plot

`DELETE CLUSTER_IP/api/learningOrchestra/v1/explore/tsne/<plotName>`

Deletes an image plot by specifying its plotName.

### Image plot examples

These examples use the [titanic challengue datasets](https://www.kaggle.com/c/titanic/overview).

#### Titanic Train dataset

![](./tsne_titanic_train.png)

#### Titanic Test dataset

![](./tsne_titanic_test.png)
