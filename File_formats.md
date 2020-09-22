## Data file formats

This file describes the format of the files that can be parsed and loaded into ArangoDB by the DJORNL parser, https://github.com/kbase/relation_engine/blob/develop/importers/djornl/parser.py.

Given a file in TSV or CSV format, the parser can load data from the columns specified in the descriptions below.

Column names are case-insensitive; the parser converts them all to lower case.

### Manifest file

This is used to describe the list of files in each release, and is used by the parser to know which files are to be parsed and what format they are in. The format of the manifest file is validated using JSON Schema against the schema file, https://github.com/kbase/relation_engine/blob/develop/spec/datasets/djornl/manifest.schema.json.

### Cluster files

TSV data

| required? | column name | old name | data type | contents |
|-|-|-|-|-|
| R | cluster_id | n/a | integer | cluster number |
| R | node_ids   | n/a | string  | comma-separated array of node_ids |

Notes: the node IDs should be separated by commas, rather than using the same separator as the column data uses.

### Edge files

| required? | column name | old name | data type | contents |
|-|-|-|-|-|
| R | node1      | node1         | string | node ID |
| R | node2      | node1         | string | node ID |
| R | score      | edge          | number | edge weight/score |
| R | edge_type  | layer_descrip | string | must be one of the edge types listed in the [edge schema file](https://github.com/kbase/relation_engine/blob/develop/spec/datasets/djornl/edge_type.yaml) |

Notes: data from the `edge_descrip` column is tied to the `edge_type`, so it can be captured elsewhere. The parser will ignore this column.

### Node files

| required? | column name | old name | data type | contents |
|-|-|-|-|-|
| R | node_id | - | string | unique node ID |
| R | node_type | - | string | one of the node types listed in the [node schema file](https://github.com/kbase/relation_engine/blob/develop/spec/datasets/djornl/node_type.yaml) |
| | transcript | - | string | transcript ID |
| | gene_symbol | - | string | gene symbol |
| | gene_full_name | - | string | full name |
| | gene_model_type | - | string | gene model name |
| | tair_computational_description | - | string | |
| | tair_curator_summary | - | string | |
| | tair_short_description | - | string | |
| | go_description | GO_descr | string | GO term name(s) -- see note below |
| | go_terms | - | string | GO term IDs, comma-separated |
| | mapman_bin | - | string | MapMan ID |
| | mapman_name | - | string | MapMan term name |
| | mapman_description | MapMan_descr | string | MapMan description |
| | pheno_aragwas_id | - | string |  |
| | pheno_description | pheno_descrip1 | string |  |
| | pheno_pto_name | pheno_descrip2 | string | Plant Trait Ontology (PTO) term name |
| | pheno_pto_description | pheno_descrip3 | string | PTO term description |
| | pheno_reference | pheno_ref | string | DOI of publication reference for phenotype  |
| | user_notes | - | string | freeform text |

Note: GO descriptions cannot be parsed into single term names at present as the data in the field is comma-separated, but does not take into account term names with commas in them.
