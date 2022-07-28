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


## Instructions

Navigate to https://ca.uoaarchdb.cloud.edu.au/install/ to (re-)run the installer.

On this page enter an email address (relevant when email server configured) and select a profile
from the dropdown menu.

You can upload a new profile xml file by dragging and dropping the file into the browser. This should
then create or update the profile and select it in the "Installation Profile" dropdown.

Profiles are located in the `/var/www/html/ca/install/profile/xml` directory on the server.

Select the "overwrite existing installation?" box and then click "begin installation". This process
will take a few minutes to complete. When completed you will be given the username/password of the
administrator account - note these down for use!