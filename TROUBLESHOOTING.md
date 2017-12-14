# Troubleshooting

 1. Profiling (shows that we're using a TermQuery):

        http --session es :9200/country/_search query:='{"match": {"name": {"query": "cyprus"}}}' profile=true

 2. Explain (shows how a hit was calculated):

        http --session es :9200/country/_search query:='{"match": {"name": {"query": "Cyprus"}}}' explain=true

 3. See what the analyzer is doing:

        http --session es :9200/country/_analyze analyzer=standard text="United States" | jq .tokens[].token
