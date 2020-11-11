# /api/learningOrchestra/v1/transform/dataType

The `dataType` resource changes the fields data type from stored datasets between `number` and `string`.

## Change field types of a dataset

`PATCH CLUSTER_IP/api/learningOrchestra/v1/transform/dataType`

The request uses `inputDatasetName` as the dataset name to change the field types, and the `types` uses a `number` or `string` descriptor in each `Key:Value` to describe the new value of altered field in dataset.

```json
{
    "inputDatasetName": "SomeDatasetName",
    "types": {
        "field1": "number",
        "field2": "string"
    }
}
```
