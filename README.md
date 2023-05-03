# Ahuahu / Great Mercury Island Collective Access Database

This repository houses instructions related to creating a Collective Access
instance from scratch for the Ahuahu / Great Mercury Island project, as well as
various instructions and files relating to it's ongoing operation.

The current instance of Collective Access can be found at https://ca.uoaarchdb.cloud.edu.au

## Instructions

This directory houses instructions that should walk you through creating a new CA instance on Nectar,
configuring CA to behave as desired, and uploading data into CA. THe following topics are covered:

1) install and set up a working Collective Access instance
2) perform an in-place update to Collective Access
3) update the main schema/tables used by Collective Access
4) adding metadata fields to the various tables
5) tweaking the UIs shown in Collective Access
6) importing and exporting data

## Configuration_files

This directory houses configuration files that govern how CA is set up and operates. These files contain
configuration changes needed to run the ArchDB CA instance. These files need to be placed in the CA directory: 
`~/ca/app/conf/local/`.

## Mappings

This directory houses directories and `xlsx` files that are used to import metadata 
into CA. 

Some of these mappings are used to initially import established objects to speed up any database rebuilds 
+ "data ingestion" (such as the `Faiyum` Project and Egypt+NZ Countries) .
These files begin with `#_` where `#` is replaced by a number. This number is the order in which
to import these data files.

The photo_mappings folder contains a mapping xlsx and several xlsx files corresponding to the 2009-2014 
photo datasets. These xlsx files will import the ~8000+ media object (metadata+images) into CA.

The `shapefile_mappings` folder contains mapping files for the FY shapefile data, and shapefile feature data 
exported to CSV files. The folder is further divided into `lines`, `polygons`, and `points` folders to house 
the csv files for said shapefile data shapes. `shapefiles_mappings` also contains an example data+mapping to map a 
geospatial polygon to an example AOI as part of the Georeference metadata. This example can be used to ingest
the geospatial aspect of the data into CA if desired.

In addition to the csv of shapefile features, the `points` folder contains some `xlsx` representations of the 200K
points split up into different types of data. `point_data` object types are described in the `points_data-object_types.txt` file,
with some exceptions noted at the end of the file. `xlsx` files of data and mappings have been made as an example for files up to the `core` type. The files in the `done` directory have already been uploaded/ingested into the current instance of CA.
`ZZZ_` files are left aside until it is figured out how to represent these data in CA.


## Profiles

This directory houses any pre-made and custom profiles:

* `profile.xsd` provides the Schema to validate all templates against
* `default.xml` provides a copy of the Collective Access default template
* `test_profile.xml` is a test xml currently under development (depreciated)
* `YYYY_MM_DD_profile.xml` are snapshots of profiles while under development

In general the latest profile should be the most up-to-date and recommended to use.


# Summary of project work

* Created the https://ca.uoaarchdb.cloud.edu.au Nectar VM instance
* Configured the VM according to `1_installation.md` instructions using the `configuration_files` folder and profiles in `profiles` folder
* Went through and documented the process of updating CA in place; see `2_updating_ca.md`
* Created CA models(schemas) for the broad tables (POI/AOI/Project/Sources/etc) of the ArchDB schema; implemented in the `profiles` folder
* Created relations for most tables and key metadata such as UNIDs and keywords.
* Configured UIs for some tables such as projects/media/objects
* Documented the process of creating new CA tables/metadata/relations/UIs in `2.5_adjusting_profiles.md` and `3_models_metadata_UIs.md`
* Examples in the existing database (and profiles the `profiles` folders) of how to implement various things / extend to other models
* Documented the upload/download process for ArchDB data into CA; see `4_import_export_data.md`
* Created examples of how to upload media data, geospatial metadata, and object metadata; see `mappings` folder
* Created example project in CA and uploaded the Fayum POIs,AOIs, media+metadata, and arch_objects (via the shapefile data) for ~30K artefacts (Beads through to Coins, around 15% of the "artefact" data.)

### Key things/nuances to remember

* export the profile when you make changes
* make snapshots at the DB level (and volume and VM level for even more ease!) when experimenting with models/metadata/uploads
* The ingested data likely needs some curation (object keywords for instance)
* Be careful whyen uploading data as it is easy to choose the wrong mapping and create lots of bad entries!
* Your volume needs to be at least ~2.5x the media data you wish to ingest (*due to thumbnails/etc)
* Access control can be assigned on every UI to a specific role, meaning you can limit who can curate what, and even who can see what objects (or even specific object metadata) exists.
