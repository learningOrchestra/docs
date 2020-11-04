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
    "input_filename": "used_dataset_filename",
    "output_filename": "result_image_plot_filename",
    "label": "dataset_label_column"
}
```

The `label` is the label name of the column for machine learning datasets which has labeled tuples. In the case that the dataset used doesn't contain labeled tuples, define the value as null type in json:

```json
{
    "input_filename": "used_dataset_filename",
    "output_filename": "result_image_plot_filename",
    "label": null
}
```

## Read the filenames of the created images

`GET CLUSTER_IP/api/learningOrchestra/v1/explore/tsne`

Returns a list with all created images plot filenames.
 
## Read an image plot

`GET CLUSTER_IP/api/learningOrchestra/v1/explore/tsne/<filename>`

Returns the image plot of the specified filename.

## Delete an image plot

`DELETE CLUSTER_IP/api/learningOrchestra/v1/explore/tsne/<filename>`

Deletes an image plot by specifying its filename.

### Image plot examples

These examples use the [titanic challengue datasets](https://www.kaggle.com/c/titanic/overview).

#### Titanic Train dataset

![](./tsne_titanic_train.png)

#### Titanic Test dataset

![](./tsne_titanic_test.png)
