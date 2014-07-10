# Detexify data

This repository contains instructions on how to obtain and use the training data for http://detexify.kirelabs.org. It does not contain the data.

## Obtaining the data

Detexify's training data is stored in a [CouchDB](http://couchdb.apache.org/) database hosted on [Cloudant](https://cloudant.com).

The database lives at: https://kirelabs.cloudant.com/detexify

### Replication

The best way to obtain the data is to set up your own CouchDB and [replicate](http://guide.couchdb.org/draft/replication.html) the database to yours. This can be done easily via CouchDB's Admin Interface. Assuming you have a local CouchDB running visit

http://127.0.0.1:5984/_utils

If you are familiar with CochDB's HTTP interface you can instead use the following request to start replication:

    POST /_replicate HTTP/1.1
    Content-Type: application/json
    {"source":"https://kirelabs.cloudant.com/detexify","target":"detexify"}
    
## Querying data & data format

You can query data via the [view](http://guide.couchdb.org/editions/1/en/views.html) `by_id`. Have a look at https://kirelabs.cloudant.com/detexify/_design/tools/_view/by_id?keys=%5B%22amssymb-OT1-_bigstar%22%5D&descending=false&include_docs=true&reduce=false&limit=5 for an example.

A prettyprinted example document can be viewed in [example.json](example.json). It was obtained via the request https://kirelabs.cloudant.com/detexify/_design/tools/_view/by_id?skip=4242&limit=1&reduce=false&include_docs=true.

The json objects contain two relevant keys. One is `key` and it identifies the LaTeX command this sample is for (see https://github.com/kirel/detexify/blob/master/lib/latex/symbol.rb#L22 for details). The other one is `data` which contains an array of ink strokes. Ink strokes are represented as arrays of objects `{ x: x-coordinate, y: y-coordinate, t: timestamp }`. This data is _not_ preprocessed in any way.

## License

The database is licensed under the [ODbL](odbl-10.txt). This license is also used by [OpenStreetMap](http://wiki.openstreetmap.org/wiki/Open_Database_License). A human-readable form of the license can be found at http://opendatacommons.org/licenses/odbl/summary/. I am no expert in database licensing but this looks reasonable to me. Feel free to contact [me](kirelabs.org) if you have questions or concerns.

## Note

Please be kind. I pay for database traffic. Replicate the data once and then use your own database.
