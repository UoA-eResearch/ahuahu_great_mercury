# Ahuahu / Great Mercury Island Collective Access Database

This is a quick repo to house instructions related to initiating a Collective Access
instance from scratch for the Ahuahu / Great Mercury Island project, as well as
various instructions and files relating to it's ongoing operation.

The current instance of Collective Access can be found at https://ca.uoaarchdb.cloud.edu.au

## Configuration_files

This directory houses configuration files that govern how CA is set up and operates.

## Instructions

This directory houses instructions on how to:

* install and set up a working Collective Access instance
* perform an in-place update to Collective Access
* update the profile used by Collective Access (via XML)

## Mappings

This directory houses directories and `xlsx` files that are used to import metadata 
into CA. 

Some of these mappings are used to initially import established objects,
such as the `Faiyum` Project and Egypt+NZ Countries, to speed up any database rebuilds + "data ingestion".
These files begin with `#_` where `#` is replaced by a number. This number is the order in which
to import these data files.

The photo_mappings folder contains a mapping xlsx and several xlsx files corresponding to the 2009-2014 
photo datasets. These xlsx files will import the ~8000+ media object (metadata+images) into CA.

The shapefile_mappings folder contains mapping files for the FY shapefile data.
The folder is further divided into `lines`, `polygons`, and `points` folders to house the xlsx files
for said shapefile data. `shapefiles_mappings` contains an example data+mapping to map a polygon to an
example AOI.


## Profiles

This directory houses any pre-made and custom profiles:

* `profile.xsd` provides the Schema to validate all templates against
* `default.xml` provides a copy of the Collective Access default template
* `test_profile.xml` is a test xml currently under development (depreciated)
* `YYYY_MM_DD_profile.xml` are snapshots of profiles while under development