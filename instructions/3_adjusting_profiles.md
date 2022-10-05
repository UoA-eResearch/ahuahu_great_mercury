# Adjusting profiles

Any changes to the profile xml file used by Collective Access requires a rebuild of the database
tables (meaning all data will be deleted.)

You can also configure tables manually using the Collective Access administrator UI, though
remember to make these changes to the appropriate XML file at some point to ensure the 
profiles can be rebuilt easily in the future.

If you want to experiment with different profiles you may wish to set the `__CA_ALLOW_INSTALLER_TO_OVERWRITE_EXISTING_INSTALLS__` option in setup.php. By default the installer will refuse to install over an existing installation. With `__CA_ALLOW_INSTALLER_TO_OVERWRITE_EXISTING_INSTALLS__` set the installer will include an option to overwrite existing data. In a real system this is extremely dangerous – any one with access to the installer can delete the entire system – but is very handy for testing and evaluation.

In addition the `__CA_ALLOW_DRAG_AND_DROP_PROFILE_UPLOAD_IN_INSTALLER__` option can be defined in
the setup.php file, allowing you to drag and drop an XML file onto the browser in order to upload and
use the file to install the database.


## Profile installation instructions

Navigate to https://ca.uoaarchdb.cloud.edu.au/install/ to (re-)run the installer.

On this page enter an email address (relevant when email server configured) and select a profile
from the dropdown menu.

You can upload a new profile xml file by dragging and dropping the file into the browser. This should
then create or update the profile and select it in the "Installation Profile" dropdown.

Profiles are located in the `/var/www/html/ca/install/profile/xml` directory on the server.

Select the "overwrite existing installation?" box and then click "begin installation". This process
will take a few minutes to complete. When completed you will be given the username/password of the
administrator account - note these down for use!


## What the heck is this xml profile?

See https://manual.collectiveaccess.org/providence/user/dataModelling/introductionData.html to delve into the verbose CA documentation on Data structure and XML profiles. Read on for Mike's abridged version of the XML files.

The xml profile is used to "map/translate" your own database structure/schema (i.e. the Archeology DB schema) into a format that CA can understand how to use. This is done by mapping your own DB tables into 1 of the base-CA table types (objects, collections, places, entities, etc). You can specify as many "sub-types" as you desire and control almost every aspect of those sub-types. i.e. under the "object" table type you could create an "artifact" type, "stone" type, "fruit" type and so forth.

The XML profile is broken up into 5 main sections: Locales, Lists, Elementsets, UserInterfaces, and RelationshipTypes.

### Locales

This is the smallest section of the XML profile and is used to specify the languages to be used throughout the Collective Access instance. e.g.

```
<locales>
  <locale lang="en" country="US">English</locale>
  <locale lang="mi" country="NZ">Māori</locale>
</locales>
```

While this is the smallest section of the profile it is responsible for a large portion of the XML file in the form of Label tags associated with almost every user-facing aspect of the XML file. All tables, metadata elements, relationships, and user inferaces require you to specify a label for the item in question, and each label needs to be tied to one of the specified locales. Currently only English is implemented in the `test-profile.xml`.

### Lists

This section is where you define the ArchDB tables that you wish to populate Collective Access with. The ArchDB tables need to be "mapped" onto the base CA tables so that CA understands "what" the ArchDB tables are and what to do with them. The mapping is done in this section of the xml. 

The `lists` section is composed of a number of `<list> </list>` entries. These entries take the form of `<list> code="XXX_YYY"</list>` where `XXX` is typically a base CA table such as `object` or `entity`, and where `YYY` is typically a suffix such as `type`, or `sources` and `statuses`. 

Each of these lists also takes the `hierarchical`, `system`, and `vocabulary` boolean flags that detewrmine whether the tables are heirarchical in nature, whether they belong to the CA system, and whether they are vocabulary tables.

```
<lists>
  <list code="object_sources" hierarchical="1" system="0" vocabulary="0">
    ...
  </list>
  <list code="object_types" hierarchical="1" system="0" vocabulary="0">
    ...
  </list>
  ...
</lists>
```

**It is unlikely that you will need to add any new top level list entries, and CA will not work properly if some of the entries are omitted.** Instead you will need to customise the contents of the list entries.

The `<list> code="XXX_type"</list>` entries can be used to create one or more custom table types of a base CA table. The example below shows how a `Feature` table can be created using the CA `object` base table by adding it as an item entry:

```
<list code="object_types" hierarchical="1" system="0" vocabulary="0">
  <labels>
    <label locale="en_US">
      <name>Object types</name>
    </label>
  </labels>
  <items>
    <item idno="feature" enabled="1" default="1">
      <labels>
        <label locale="en_US" preferred="1">
          <name_singular>Feature</name_singular>
          <name_plural>Features</name_plural>
        </label>
      </labels>
    </item> 
  </items>
</list>
```

Similarly, sub-types of a ArchDB object can be made by nesting an `<items> </items>` field in an existing ArchDB item:

```
<list code="object_types" hierarchical="1" system="0" vocabulary="0">
  <labels>
    <label locale="en_US">
      <name>Object types</name>
    </label>
  </labels>
  <items>
    <item idno="feature" enabled="1" default="1">
      <labels>
        <label locale="en_US" preferred="1">
          <name_singular>Feature</name_singular>
          <name_plural>Features</name_plural>
        </label>
      </labels>
      <items>
        <item idno="feature_subtype" enabled="1" default="1">
          <labels>
            <label locale="en_US" preferred="1">
              <name_singular>Feature subtype</name_singular>
              <name_plural>Feature subtypes</name_plural>
            </label>
          </labels>
        </item>
      </item> 
  </items>
</list
```

The `<list> code="XXX_sources"</list>` and `<list> code="XXX_statuses"</list>` entries are used to specify "types" that correspond to intrinsic CA table metadata entries `source` and `status` that apply to various tables. They are intrinsic to CA so will break CA if omitted, however they may not be useful for the ArchDB. I have left them in for functionality but feel free to edit their entries as appropriate if you find a use!

### Element sets

- To expand


### User Interfaces

- To expand


### Relationship Types

- To expand


### Extras

A useful cookbook/FAQ for installation profiles can be found at https://manual.collectiveaccess.org/providence/user/Cookbook/cookbook_install_profiles.html.
