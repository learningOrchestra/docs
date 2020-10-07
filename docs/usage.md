# Usage

learningOrchestra can be used with the [Microservices REST API](#microservices-rest-apis) or with the `learning-orchestra-client` [Python package](https://pypi.org/project/learning-orchestra-client/).

## Microservices REST APIs

[Database API](https://learningorchestra.github.io/docs/database-api/)- Download and handle datasets in a database.

[Projection API](https://learningorchestra.github.io/docs/projection-api/)- Make projections of stored datasets using Spark cluster.

[Data type API](https://learningorchestra.github.io/docs/datatype-api/)- Change dataset fields type between number and text.

[Histogram API](https://learningorchestra.github.io/docs/histogram-api/)- Make histograms of stored datasets.

[t-SNE API](https://learningorchestra.github.io/docs/t-sne-api/)- Make a t-SNE image plot of stored datasets.

[PCA API](https://learningorchestra.github.io/docs/pca-api/)- Make a PCA image plot of stored datasets.

[Model builder API](https://learningorchestra.github.io/docs/modelbuilder-api/)- Create a prediction model from pre-processed datasets using Spark cluster.

### Spark Microservices

The Projection, t-SNE, PCA and Model builder microservices uses the Spark microservice to work.

By default, this microservice has only one instance. In case your data processing requires more computing power, you can scale this microservice.

To do this, with learningOrchestra already deployed, run the following in the manager machine of your Docker swarm cluster:

`docker service scale microservice_sparkworker=NUMBER_OF_INSTANCES`

*\** `NUMBER_OF_INSTANCES` *is the number of Spark microservice instances which you require. Choose it according to your cluster resources and your resource requirements.*

## Database GUI

NoSQLBooster- MongoDB GUI performs several database tasks such as file visualization, queries, projections and file extraction to CSV and JSON formats. 
It can be util to accomplish some these tasks with your processed dataset or get your prediction results.

Read the [Database API docs](https://learningorchestra.github.io/docs/database-api/) for more info on configuring this tool.
