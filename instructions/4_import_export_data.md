Now that we've finally created our CA schema to specify all our lists/metadata/UIs/relationships it's time to get data into the CA instance!

# adding data in bulk using Importers

The UI is nice and intuitive to add new entries one-by-one, but to add them in bulk takes a bit more effort.
Fortunately the "import" mechanism in CA is quite comprehensive (compared to many other curation tools!) but unfortunately
it's documentation is not as clear as it ought to be.

I'll detail the rough process with links below, but one key suggestion is to take snapshots of the database while experimenting/developing data imports. I didn't do this, but I had an empty database to play with that I could quickly rebuild. Now that I've uploaded quite a few entries (media, bone artefacts, places) that take ~1-2days to ingest I would ceretainly snapshot the database.

If you have the Nectar resources (increase the allocation until you have) then I would suggest snapshotting the volume and VM as well so that things are even easier to recover!

CA works by creating an `Import Mapping` file that describes how to interpret a data file and how to translate that into
the corresponding CA model schema. Information on this concept can be found here: https://manual.collectiveaccess.org/providence/user/import/introduction.html

In practice, I have created `import mappings` in excel, and used them to import excel files containing the Fayum data. Several examples exist in the `~\mappings` folder.

Here are some instructions on how to write an import mapping excel file: https://manual.collectiveaccess.org/providence/user/import/c_import_tutorial.html. I found that they were not always sufficiently documented, so please look at the examples I have created as they should cover most of the nuances and examples required. If need, ask me how to try something!

### Caveats / important notes

* We have both media and media metadata, so the best way to upload this is to move the media files to the VM manually (in the `/gm_data/import/` directory) and then ingest a spreadsheet of metadata information which references the server location. Other ways will make it a nightmare to match up the metadata and file records!
* Uploading media this way will create images/thumbnails of each specified file (increasing the data size to ~2-2.5x total). If an error occurs the "new" DB entry is correctly deleted but the new thumbnail files still remain around. This will bloat the disk eventually so I suggest using Volume snapshots before running media imports. It is a nightmare to remove these files manualy as CA obfuscates how it stores it's new media.
* Hierarchies, such as the place data, are an absolute nightmare to specify! The "API" that controls it (or at least `Places`) only allows you to match parents with children using `parent_id` (the hidden internal CA DB ID) instead of `labels`. I created an example exporter that will output an internal_ID and label for POIs - this can be used as a basis for other times.

### Important note about the artefact mappings

I wasn't sure how to handle the source_id column so I implemented it as a keyword rather than auto-creating Sources with only IDs as names... For the uplaoded examples this only resulted in N<20 keywords that related to Sources/other "relations" and so not much manual work to fix for the ingested data. However this may create lots of work for the remaining data and so you might wish to change how the data mapping works (change from keywords to a Source/collection relation - see camera-media relation for example).

# Running an import

The actual steps of running an import can be found here: https://manual.collectiveaccess.org/providence/user/import/running.html

Note that this can take time especially for media. The upload of the ~30,000 Bone entries took at least 8hrs...


# Exporting data

This is conceptually the same as importing in that we use an `export mapping` to specify how we extract CA data into a file (CSV, XML, etc). Instructions can be found here: https://manual.collectiveaccess.org/providence/user/export/mappings.html

The actual workflow for exporting data is to:

1) upload an exporter mapping
2) run a search on the desired entries (i.e. on POIs, or on Bone artefacts, etc)
3) click on the download/export button above the search results
4) choose `export with mapping` and select your mapping
5) once done, click on `set up export download` and then `download` to download your file

Caveat: for some utterly insane confusing reason my exported CSVs ended up semi-colon delimited instead of comma delimited...!