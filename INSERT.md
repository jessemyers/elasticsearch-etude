# Inserting Data

We're going to use the [Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/5.6/docs-bulk.html)
and the [Get API](https://www.elastic.co/guide/en/elasticsearch/reference/5.6/docs-get.html).

----

 1. Bulk insert data (`countries.csv` not included):

        # start with the second line
        tail -n +2 countries.csv | \

            # extract first, second, and eight columns (uri, name, capital) as JSON array
            python3 -c 'import csv, sys; c = csv.reader(sys.stdin); print("\n".join(f"[\"{r[0]}\",\"{r[1]}\",\"{r[7]}\"]" for r in c))' | \

            # export in the correct bulk format
            jq -c '{index: {_id: .[1]}}, {uri: .[0], name: .[1], capital: .[2], suggest: .[1]}' | \

            # upload
            http --session es :9200/country/country/_bulk

 2. Query by identifier:

        http --session es :9200/country/country/Cyprus
