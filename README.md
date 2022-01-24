# dfbridge

## A Schematized dataframe formatter.

We often have need to reformat a base dataframe to create a dataframe following a schema, applying a combination of renaming some columns, applying functions to others, and doing groupby/transform operations.
These steps introduce a lot of boilerplate, but here we can assign it as a dictionary schema.
The original dataframe is unchanged, and all of the operations take place only on the original dataframe.

Let's say we want the output dataframe to have columns `final_name1`, `final_name2`, and `final_name3`, with one of them a simple rename from an input dataframe, one the result of some fucntion applied to the input dataframe, and one some groupby transform operation.
We can even remap values to other values in the process.
Setting `fill_missing` to True lets one add the column and set it as full of pandas NA values.

The schema to do this looks like:

```python
schema = {
    "final_name1`": {
        "type": "rename",
        "from": "original_name",
        "fill_missing": True,
        "column_type": None,
        'remap_dict': {'orig_val': 'new_val'}, # Remaps elements with original val to new val. Set to None or ignore to not use.
        'strict_remap': True, # If True, values not in the remap_dict are made pd.NA, else are passed through intact.
    },
    "final_name2": {
        "type": "apply",
        "func": function, # Expects the whole row of the original dataframe, so use row['col] style arguments.
        "fill_missing": True,
        "column_type": None,
        'remap_dict': None, # Remaps elements with original val to new val. Set to None or ignore to not use.
    },
    "final_name3": {
        "type": "transform",
        "groupby": "groupby_column",
        "column": "return_column",
        "action`": "mean", # (or anything that works in df.groupby().transform())
        "fill_missing": True,
        "column_type": None,
    },
}
```
