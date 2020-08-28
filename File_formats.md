Data file formats

## Cluster files

TSV data

| required? | column name | old name | data type | contents |
|-|-|-|-|-|
| R | cluster_id | n/a | integer | cluster number |
| R | node_ids   | n/a | string  | comma-separated array of node_ids |

Notes: the node IDs should be separated by commas, rather than using the same separator as the column data uses.

## Edge files

| required? | column name | old name | data type | contents |
|-|-|-|-|-|
| R | node1      | node1         | string | node ID |
| R | node2      | node1         | string | node ID |
| R | score      | edge          | number | edge weight/score |
| R | edge_type  | layer_descrip | string | must be one of the edge types listed in ...; |

Notes: data from the `edge_descrip` column is tied to the `edge_type`, so it can be captured elsewhere

## Node files

| required? | column name | old name | data type | contents |
|-|-|-|-|-|
| R | node_id | - | string | unique node ID |
| R | node_type | - | string | one of the node types listed in ... |
| | transcript | - | string | transcript ID |
| | gene_symbol | - | string |
| | gene_full_name | - | string |
| | gene_model_type | - | string |
| | tair_computational_description | TAIR_Computational_description | string |
| | TAIR_Curator_summary | - | string |
| | TAIR_short_description | - | string |
| | go_description | GO_descr | string | GO term name(s) -- to be revisited |
| | go_terms | - | string | GO term IDs, comma-separated |
| | MapMan_bin | - | string | MapMan ID |
| | MapMan_name | - | string | MapMan term name |
| | mapman_description | MapMan_descr | string | MapMan description |
| | pheno_AraGWAS_ID | - | string |  |
| | pheno_description | pheno_descrip1 | string |  |
| | pheno_pto_name | pheno_descrip2 | string |  |
| | pheno_pto_description | pheno_descrip3 | string |  |
| | pheno_reference | pheno_ref | string |  |
| | user_notes | - | string | freeform text |
