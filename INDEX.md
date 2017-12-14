# Indexes and Mappings

 1. Create an index:

        http --session es PUT :9200/country

 2. Retrieve the index:

        http --session es :9200/country

 3. Put a mapping for a type:

        http --verbose --session es PUT :9200/country/_mapping/country properties:=@properties.json

 4. Delete an index:

        http --session es DELETE :9200/country

 5. Create and index with a mapping at once:

        http --session es PUT :9200/country mappings:=@mappings.json