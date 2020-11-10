## exascale_data

Data repo for the Exascale collaboration between KBase and the Jacobson lab at ORNL.

File format guide: [File formats.md](https://github.com/kbase/exascale_data/blob/main/File_formats.md)

Parser: https://github.com/kbase/relation_engine/tree/develop/importers/djornl

### JSONSchema specs for the dataset

All in the [KBase Relation Engine](https://github.com/kbase/relation_engine) repo

- Node and edge database schemas: [spec/collections/djornl](https://github.com/kbase/relation_engine/tree/develop/spec/collections/djornl)

- Stored queries: [spec/stored_queries/djornl](https://github.com/kbase/relation_engine/tree/develop/spec/stored_queries/djornl)

- Dataset definitions: [spec/datasets/djornl](https://github.com/kbase/relation_engine/tree/develop/spec/datasets/djornl)



## Setting up the relation engine locally

Clone the relation_engine repo

```
git clone https://github.com/kbase/relation_engine
```
### Bring up the Docker container

Build the docker container
```sh
docker-compose -f relation_engine/docker-compose.yaml -f docker-compose.override.yaml build
```

Run the container, opening a shell in the python image:
```
docker-compose -f relation_engine/docker-compose.yaml -f docker-compose.override.yaml run re_api sh
```

From the python shell, start up the db server in the background:

```sh
sh scripts/start_server.sh &
```
### Using the DJORNL Exascale data parser

Use the parser to load some data; if there are no errors in the data, you will get output detailing the number of nodes and edges added to the database. The shell environment variable `RES_ROOT_DATA_PATH` contains the path to the data directory containing `manifest.yaml` and the data files.

By default, `RES_ROOT_DATA_PATH` is set to point to the exascale data; that dataset can be loaded into the db with the command:
```sh
python -m importers.djornl.parser
```

In this example, we load a small test dataset:

```sh
RES_ROOT_DATA_PATH=/app/spec/test/djornl/test_data python -m importers.djornl.parser
```

Successful output:

```
Parsing edge file /app/spec/test/djornl/test_data/edges.tsv
Parsing edge file /app/spec/test/djornl/test_data/hithruput-edges.csv
Parsing node file /app/spec/test/djornl/test_data/nodes.csv
Parsing node file /app/spec/test/djornl/test_data/pheno_nodes.csv
Parsing node file /app/spec/test/djornl/test_data/extra_node.tsv
Parsing cluster file /app/spec/test/djornl/test_data/I2_named.tsv
Parsing cluster file /app/spec/test/djornl/test_data/I4_named.tsv
Parsing cluster file /app/spec/test/djornl/test_data/I6_named.tsv
PUT /api/v1/documents -> 200 OK
Saved docs to collection djornl_node!
{
  "created": 14,
  "empty": 0,
  "error": false,
  "errors": 0,
  "ignored": 0,
  "updated": 0
}

================================================================================
PUT /api/v1/documents -> 200 OK
Saved docs to collection djornl_edge!
{
  "created": 10,
  "empty": 0,
  "error": false,
  "errors": 0,
  "ignored": 0,
  "updated": 0
}

================================================================================
```

If there are errors in the dataset, the parser will list them and will not perform the data load. This example dataset has duplicate node and edge data:

```sh
RES_ROOT_DATA_PATH=/app/spec/test/djornl/duplicate_data python -m importers.djornl.parser
```

Output:
```
Parsing edge file /app/spec/test/djornl/duplicate_data/edges.tsv
Parsing edge file /app/spec/test/djornl/duplicate_data/hithruput-edges.csv
Parsing node file /app/spec/test/djornl/duplicate_data/nodes.csv
Parsing node file /app/spec/test/djornl/duplicate_data/pheno_nodes.csv
Parsing node file /app/spec/test/djornl/duplicate_data/extra_node.tsv
Parsing cluster file /app/spec/test/djornl/duplicate_data/I2_named.tsv
Parsing cluster file /app/spec/test/djornl/duplicate_data/I4_named.tsv
Parsing cluster file /app/spec/test/djornl/duplicate_data/I6_named.tsv
Parsing cluster file /app/spec/test/djornl/duplicate_data/I6_copy.csv
hithruput-edges.csv line 5: duplicate data for edge AT1G01010__AT1G01030__protein-protein-interaction_high-throughput_AraNet_v2
hithruput-edges.csv line 9: duplicate data for edge AT1G01030__AT1G01050__pairwise-gene-coexpression_AraNet_v2
extra_node.tsv line 5: duplicate data for node AT1G01080
```


#### Docker Desktop client

If you use Docker Desktop, you can open the ArangoDB image in a browser and view the data you have loaded.


### Shut down

Close the python shell, and then stop the docker container and all the images therein:
```
docker-compose -f relation_engine/docker-compose.yaml -f docker-compose.override.yaml down --remove-orphans
```

### Note about running the parser without docker

If docker is unavailable, it may still be possible to run the parser, eg:

```sh
cd exascale_data
git clone https://github.com/kbase/relation_engine.git
cd relation_engine
RES_ROOT_DATA_PATH=../prerelease/ python -m importers.djornl.parser
```

The shell environment variable `RES_ROOT_DATA_PATH` contains the path to the data directory containing `manifest.yaml` and the data files.


### Adding new data

* If adding new edge types, make a pull request to the relation_engine repo with the proposed new edge types in `spec/datasets/djornl/edge_type.yaml`.

* If adding new node types, make a pull request to the relation_engine repo with the proposed new node types in `spec/datasets/djornl/node_type.yaml`.

* Update the `manifest.yaml` file to include the new files.

* Run the parser on the new dataset to ensure that the data does not contain any errors.

* Make a pull request to the exascale_data repo.
