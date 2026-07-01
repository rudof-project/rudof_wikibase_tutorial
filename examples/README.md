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

rudof is based on configuration files which define values for default properties like names of endpoints and namespace aliases. The default config file [is this one](https://github.com/rudof-project/rudof/blob/master/rudof_lib/src/default_config.toml).

If you want to use your own endpoint and namespace aliases you can create your own config.toml. For example, this [rhizome_config.toml]() contains definitions for the rhizome wikibase instance. You can specify the config in the command line as:

```
rudof
```

## Querying Wikidata from rudof


## Information about a node in a different Wikibase instance

## Validating with an entity schema


