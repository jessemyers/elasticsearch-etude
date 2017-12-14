# Basics

We're going to use the [CAT API](https://www.elastic.co/guide/en/elasticsearch/reference/5.6/cat.html).

----

 1. Health check:

        http -a elastic:changeme :9200/_cat/health

 2. Using a session:

        http --session es -a elastic:changeme :9200/_cat/health

 3. Prettier, more verbose:

        http --session es :9200/_cat/health v== format==json

 4. Other available info:

        http --session es :9200/_cat

 5. Example: indexes:

        http --session es :9200/_cat/indices v== format==json

----

[Next: Indexes and Mappings](./INDEX.md)
