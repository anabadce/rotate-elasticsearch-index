# rotate-elasticsearch-index
Bash script to delete indices in Elasticsearch older than a given number of days. 
Index names must contain date in YYYY.MM.DD format.
 
This script receives as parameters:

* the URL of the ElasticSearh server, example: https://search-something.somewhere.es.amazonaws.com
* the number of days to keep, example: 3
* the string to match the indexes you want to delete, example: cspdata

Script will then for example:

1. Get all the indexes names from https://search-something.somewhere.es.amazonaws.com/_cat/indices
2. Match the ones that containg the string entered
3. Find the date in the index name
4. Those that have a date older than the days to keep will be deleted
5. Any other index will be ignored and kept

### Usage example

* The ES index list looks like this: https://search-something.somewhere.es.amazonaws.com/_cat/indices

```
green open .kibana-4          1 1     14 4  78.9kb  39.5kb 
green open cspdata-2016.09.18 5 1 111672 0   103mb  51.5mb 
green open cspdata-2016.09.20 5 1 153572 0 155.1mb  77.5mb 
green open cspdata-2016.09.21 5 1  48591 0 107.4mb    51mb 
green open cspdata-2016.09.17 5 1  72536 0  71.3mb  35.7mb 
green open cspdata-2016.09.19 5 1 224702 0 199.1mb  99.6mb 
```

* You run this command:

```
./rotate-elasticsearch-index.sh cspdata https://search-something.somewhere.es.amazonaws.com 3
```

* You get this output:

```
=== Tue Sep 21 03:30:02 UTC 2016 ===
-------------------------------------------
INFO: Index: cspdata-2016.09.17
INFO: Found date: 2016-09-17
DELETE: cspdata-2016.09.17 is about to be deleted.
-------------------------------------------
INFO: Index: cspdata-2016.09.19
INFO: Found date: 2016-09-19
INFO: cspdata-2016.09.19 is less than 3 days old, doing nothing.
-------------------------------------------
INFO: Index: cspdata-2016.09.21
INFO: Found date: 2016-09-21
INFO: cspdata-2016.09.21 is less than 3 days old, doing nothing.
-------------------------------------------
INFO: Index: cspdata-2016.09.18
INFO: Found date: 2016-09-18
INFO: cspdata-2016.09.18 is less than 3 days old, doing nothing.
-------------------------------------------
INFO: Index: cspdata-2016.08.20
INFO: Found date: 2016-08-20
INFO: cspdata-2016.09.20 is less than 3 days old, doing nothing.
-------------------------------------------
```