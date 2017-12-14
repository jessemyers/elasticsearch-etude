# Searching Data

We're going to use the [Search APIs](https://www.elastic.co/guide/en/elasticsearch/reference/5.6/search.html)

----

## Pagination

 1. First ten records:

        http --session es :9200/country/_search | jq .hits.hits[]._id

 2. First twenty records:

        http --session es :9200/country/_search size==20 | jq .hits.hits[]._id

 3. Second twenty records:

        http --session es :9200/country/_search size==20 from==20 | jq .hits.hits[]._id


## Q (URI Search)

Simple search:

 1. Match country name:

        http --session es :9200/country/_search q==Cyprus
        http --session es :9200/country/_search q==name:Cyprus

 2. Match capital too:

        http --session es :9200/country/_search q==Nicosia
        http --session es :9200/country/_search q==capital:Nicosia

 3. This misses:

        http --session es :9200/country/_search q==capital:Cyprus

 4. **NOT** fuzzy, but wildcards work:

        http --session es :9200/country/_search q==nicosi
        http --session es :9200/country/_search q=='nicos*'

        http --session es :9200/country/_search q==cprus
        http --session es :9200/country/_search q=='c*prus'

## TERM

Term-oriented search:

 1. These miss (keyword properties and terms are not analyzed):

        http --session es :9200/country/_search query:='{"term": {"name": "cyprus"}}'
        http --session es :9200/country/_search query:='{"term": {"capital": "Nicosia"}}'

 2. These hit (indexes are analyzed; wildcards are fuzzy):

        http --session es :9200/country/_search query:='{"term": {"capital": "nicosia"}}'
        http --session es :9200/country/_search query:='{"term": {"name": "Cyprus"}}'
        http --session es :9200/country/_search query:='{"wildcard": {"name": "*yprus"}}'


## MATCH

More fine-grained matching:

 1. These work (text properties and matches are both analyzed):

        http --session es :9200/country/_search query:='{"match": {"capital": {"query": "nicosia"}}}'
        http --session es :9200/country/_search query:='{"match": {"capital": {"query": "Nicosia"}}}'

 2. This still misses (keyword properties are not analyzed):

        http --session es :9200/country/_search query:='{"match": {"name": {"query": "cyprus"}}}'

 3. And also not this (matches are not fuzzy by default):

        http --session es :9200/country/_search query:='{"match": {"name": {"query": "Cprus"}}}'

 4. But we can make them fuzzy (Levenshtein edit distance):

        http --session es :9200/country/_search query:='{"match": {"name": {"query": "Cprus", "fuzziness": 2}}}'
        http --session es :9200/country/_search query:='{"match": {"name": {"query": "cyprus", "fuzziness": 2}}}'


## SUGGEST

Auto-completion:

 1. First letter:

        http --session es :9200/country/_search suggest:='{"name": {"prefix": "u", "completion": {"field": "suggest"}}}' | \
            jq .suggest.name[].options[].text

 2. More letters

        http --session es :9200/country/_search suggest:='{"name": {"prefix": "unite", "completion": {"field": "suggest"}}}' | \
            jq .suggest.name[].options[].text

----

[Next: Troubleshooting](./TROUBLESHOOTING.md)
