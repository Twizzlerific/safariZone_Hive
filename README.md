# Safari Zone - Apache Hive 

### Building the Data Lake
External tables on Hive are read-only sets of data built by files read from HDFS. They can be useful in some use cases but most frequently as a way of staging data. 

The idea is that we make Hive aware of the data through the external tables in an effort to make it easier to then act on that data. For example, we may stage data temporarily in an external table to allow for another job to parse, validate, and enrich the data before adding to a Hive table.  

 __Creating HDFS Directory:__


```
$ hdfs dfs -mkdir -p /safariZone/hive/
```

__Adding files for External Tables:__


```
$ hdfs dfs -put pkdb_ext /safariZone/hive/
```


__Creating the External Tables in Hive:__

Using Beeline, we will run the hql files to create the tables. 

```
$ beeline -f hql/Database_Creation.hql
$ beeline -f hql/pokedex/pkdb_1_EXTERNAL.hql
$ beeline -f hql/trainers/trainers_EXTERNAL.hql
```


__Building the Managed Tables__

From the External tables we can then build MANAGED tables which are essentially tables fully controlled by Hive. In most cases, this could be ACID with ORC format but we kept this stage since default types can be configured. 

__Create the Managed Tables:__

```
beeline -f hql/pokedex/pkdb_2_MANAGED.hql
beeline -f hql/trainers/trainers_MANAGED.hql
```

__Additional Formats:__

Apache Hive offers support for a bunch of different table storage formats. Run the HQL files included for a few of most popular formats: 

- [AVRO](https://avro.apache.org/)
- [ORC](https://orc.apache.org/)
- [PARQUET](https://parquet.apache.org/)
- [RCFILE](https://cwiki.apache.org/confluence/display/Hive/RCFile)
- [SEQUENCE](https://cwiki.apache.org/confluence/display/HADOOP2/SequenceFile)
- TEXT

# All in One:

```
hdfs dfs -mkdir -p /safariZone/hive/
hdfs dfs -put pkdb_ext /safariZone/hive/
beeline -f hql/Database_Creation.hql
beeline -f hql/pokedex/pkdb_1_EXTERNAL.hql
beeline -f hql/trainers/trainers_EXTERNAL.hql
beeline -f hql/pokedex/pkdb_2_MANAGED.hql
beeline -f hql/pokedex/pkdb_AVRO.hql
beeline -f hql/pokedex/pkdb_ORC.hql
beeline -f hql/pokedex/pkdb_PARQUET.hql
beeline -f hql/pokedex/pkdb_RCFILE.hql
beeline -f hql/pokedex/pkdb_SEQUENCE.hql
beeline -f hql/pokedex/pkdb_TEXTFILE.hql
beeline -f hql/trainers/trainers_MANAGED.hql
beeline -f hql/trainers/trainers_AVRO.hql
beeline -f hql/trainers/trainers_ORC_PARTITIONED.hql
beeline -f hql/trainers/trainers_ORC.hql
beeline -f hql/trainers/trainers_PARQUET.hql
beeline -f hql/trainers/trainers_RCFILE.hql
beeline -f hql/trainers/trainers_SEQUENCE.hql
beeline -f hql/trainers/trainers_TEXTFILE.hql
```

## Notes:

__Expectations:__
These commands are expected to run on a edge node with the relevant client libraries. The commands can be altered to fit your environment (i.e. passing in a beeline jdbc connection url).

__Security:__
You may need to provide permission and/or authorization privileges if security is enabled.

__DISCLAIMER__: Pokemon is the intellectual property of Nintendo and GAME FREAK. The author assumes no right to this data and the use is purely for education.  


# Credits
- [Veekun's Pokedex](https://github.com/veekun/pokedex)
- [Nintendo](https://www.nintendo.com/)

---
_DiBtP ("Done is Better than Perfect!")_

