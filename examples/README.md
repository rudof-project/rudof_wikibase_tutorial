# Examples for rudof tutorial

## Information about a node in Wikidata

Get the neighbourhood of node `wd:Q80` from Wikidata:

```sh
$ rudof node --node wd:Q80 --endpoint wikidata
Outgoing arcs
wd:Q80
├─── schema:dateModified ─► "2026-06-17T05:29:20Z"^^xsd:dateTime
├─── schema:description ─► "britský inžinier, počítačový výskumník a profesor na MIT"@sk
├─── schema:description ─► "英国计算机科学家"@zh-my
├─── schema:description ─► "informáticu británicu, inventor de la World Wide Web"@ast
├─── schema:description ─► "cientista da computação inglês (nascido em 1955)"@pt
. . .
├─── wdt:P97 ─► wd:Q209690
├─── wdt:P973 ─► <http://www.w3.org/People/Berners-Lee/>
├─── wdt:P973 ─► <https://www.obalkyknih.cz/view_auth?auth_id=xx0000870>
├─── wdt:P973 ─► <http://www.bbc.co.uk/things/2166d5db-3cd1-4d8a-a066-bddb220ef216>
├─── wdt:P9871 ─► "13570"
├─── wdt:P9885 ─► "0ea3b05d-bd7d-6075-1707-3602747891c3"
├─── wdt:P990 ─► <http://commons.wikimedia.org/wiki/Special:FilePath/Tim%20Berners-Lee%20-%20The%20New%20Elizabethans%20-%2029%20August%202012.flac>
└─── wdt:P9984 ─► "981058515612706706"
```

It can be simplified to:

```sh
$ rudof node -n wd:Q80 -e wikidata
. . .
```

If you want to get information about some specific properties, you can restrict it with:

```sh
$ rudof node -n wd:Q80 -e wikidata -p wdt:P19 -p wdt:P31
Outgoing arcs
wd:Q80
├─── wdt:P19 ─► wd:Q84
└─── wdt:P31 ─► wd:Q5
```

rudof contains some pre-defined names for SPARQL endpoints, but it is also possible to identify a generic SPARQL endpoint using its URI, so the following command also works:

```
$ rudof node -n wd:Q80 -e https://query.wikidata.org/sparql
```


## Information about a node in a different Wikibase instance

rudof is based on configuration files which define values for default properties like names of endpoints and namespace aliases. The default config file [is this one](https://github.com/rudof-project/rudof/blob/master/rudof_lib/src/default_config.toml).

If you want to use your own endpoint and namespace aliases you can create your own config.toml. For example, this [rhizome_config.toml](https://github.com/rudof-project/rudof_wikibase_tutorial/blob/main/examples/rhizome_config.toml) contains some definitions for the rhizome wikibase instance. 
You can specify the config in the command line as:

```sh
$ rudof node -n r:Q1 -c rhizome_config.toml -e rhizome
```

It is possible to use several wikibase instance declarations in the same config  file. For example, the [following file](https://github.com/rudof-project/rudof_wikibase_tutorial/blob/main/examples/wikibases_config.toml) contains several wikibase instances:

```sh
$ rudof node -n r:Q1 -c wikibases_config.toml -e rhizome
```

and now it is also possible to obtain information about a node in MARDI as:

```sh
$ rudof node -n wd:Q2958445 -c wikibases_config.toml -e mardi
```

## Using rudof to run SPARQL queries

rudof has a command called `query` that can be used to run SPARQL queries

### Querying Wikidata

```sh
rudof query -q _query.sparql -e wikidata
```


### Querying a different wikibase instance
```sh
rudof query -q mardi_query.sparql -c wikibases_config.toml -e mardi 
```


## Validating with an entity schema

Wikidata (and Wikibase) provides Entity Schemas based on the [ShEx](https://shex.io/) language which can be used to validate items. 

A directory of existing Wikidata entity schemas can be found [here](https://www.wikidata.org/wiki/Wikidata:Database_reports/EntitySchema_directory). 

rudof contains a ShEx engine that can be used to validate items against an entity schema. 

The command is `shex-validate` and it usually requires a shapemap which indicates which items to validate with which shapes in the entit schema. 

For example, the following command validates `wd:Q42` as a human (entity schema [`E10`](https://www.wikidata.org/wiki/EntitySchema:E10)) using a SPARQL query that returns the item to be validated:

```sh
rudof shex-validate -s https://www.wikidata.org/wiki/Special:EntitySchemaText/E10 -m wikidata_shapemap1.sm -e wikidata
```


### Note about Entity schemas

Currently, there 2 URIs for entity schemas in Wikibase: the HTML version, which is something like: 

```
https://portal.mardi4nfdi.de/wiki/EntitySchema:E5
```

and the raw version which is:  

```
https://portal.mardi4nfdi.de/wiki/Special:EntitySchemaText/E5
```

This is important because rudof needs the raw version of the entity schemas.

### Example from Wikidata

Wikidata 

### Example from a Wikibase.cloud instance (MARDI)

```sh
rudof shex-validate -s https://portal.mardi4nfdi.de/wiki/Special:EntitySchemaText/E5 -m mardi_shapemap2.sm -c mardi_config.toml -e mardi
```

- The parameter `-s` (`--schema`) specifices the location of the raw entity schema 
- The parameter `-m` (`--shapemap`) specifies the location of the shapemap. In this case, the shape map is a SPARQL query:
- The parameter `-c` (`--config`) specifies the config file where the mardi endpoint URI is declared
- The parameter `-e` (`--endpoint`) specifies the endpoint name

```sparql
prefix wd: <https://portal.mardi4nfdi.de/entity/>
prefix wdt: <https://portal.mardi4nfdi.de/prop/direct/>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT * WHERE {
  wd:Q2958445 wdt:P31 ?value .
  ?value rdfs:label ?label 
  FILTER (lang(?label) = 'en')
}
limit 10
```


