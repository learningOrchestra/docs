# Usage

learningOrchestra can be used with the [REST API](#rest-api) or with the [python package](https://github.com/learningOrchestra/pythonClient).

## REST API

### Dataset

[Dataset](https://learningorchestra.github.io/docs/database-api/)- Download and handle datasets in a database.

### Transform

[Projection](https://learningorchestra.github.io/docs/projection-api/)- Make projections of stored datasets using Spark cluster.

[Data type](https://learningorchestra.github.io/docs/datatype-api/)- Change dataset fields type between number and text.

### Explore

[Histogram](https://learningorchestra.github.io/docs/histogram-api/)- Make histograms of stored datasets.

[t-SNE](https://learningorchestra.github.io/docs/t-sne-api/)- Make a t-SNE image plot of stored datasets.

[PCA](https://learningorchestra.github.io/docs/pca-api/)- Make a PCA image plot of stored datasets.

### Builder

[Builder](https://learningorchestra.github.io/docs/modelbuilder-api/)- Create a prediction model from pre-processed datasets using Spark cluster.

### Spark Microservices

The Projection, t-SNE, PCA and Builder uses the Spark microservice to work.

By default, this microservice has only one instance. In case your data processing requires computing power, you need scale this microservice.

To do this, with learningOrchestra already deployed, run the following in the manager machine of your Docker swarm cluster:

`docker service scale microservice_sparkworker=NUMBER_OF_INSTANCES`

*\** `NUMBER_OF_INSTANCES` *is the number of Spark microservice instances which you require. Choose it according to your cluster resources and your resource requirements.*

