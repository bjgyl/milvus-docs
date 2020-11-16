---
id: create_drop_index.md
---

# Create and Drop an Index

This article provides Python sample codes for creating or dropping indexes.

## Create an index

Milvus supports one index for each field in a collection. Switching index type causes the original index to be deleted. A collection uses FLAT as its default index before an index type is specified. 

<div class="alert note">
<code>create_index()</code> specifies the index type of a collection and synchronously creates indexes for the previously inserted data. When the row count of the inserted data reaches <code>segment_row_limit</code>, Milvus automatically creates indexes in the background. For streaming data, it is recommended to create indexes before inserting the vector so that the system can automatically build indexes for the next data. For static data, it is recommended to import all the data at first and then create indexes. See <a href="https://github.com/milvus-io/pymilvus/tree/{{var.milvus_python_sdk_version}}/examples/indexes">index sample program</a> for more information about using index.
</div>

1. Prepare the parameters required for creating an IVF_FLAT vector index. The index parameters are stored in a JSON string, which is represented by a dictionary in The Python SDK stores the index parameters as a JSON string in the form of dictionary. 

   ```python
   # Prepare index param.
   >>> ivf_param = {"index_type": "IVF_FLAT", "metric_type": "L2", "params": {"nlist": 4096}}
   ```

   <div class="alert note">
   Different index types requires different indexing parameters. They <b>must</b> all have a value.
   </div>

2. Create index for the collection:

   ```python
   # Create an index.
   >>> client.create_index('test01', "Vec", ivf_param)
   ```

## Drop an index

After deleting the index, the vector field uses the default index type FLAT again.

```python
>>> client.drop_index('test01', "Vec")
```

## FAQ

<details>
<summary><font color="#4fc4f9">How to set the value of <code>nlist</code> when I build indexes?</font></summary>
{{fragments/faq_set_nlist.md}}
</details>
<details>
<summary><font color="#4fc4f9">Can Milvus create different types of index for different partitions in the same collection?</font></summary>
{{fragments/faq_collection_different_index.md}}
</details>
<details>
<summary><font color="#4fc4f9">Does Milvus create new indexes after vectors are inserted?</font></summary>
{{fragments/faq_create_index_after_insertion.md}}
</details>
