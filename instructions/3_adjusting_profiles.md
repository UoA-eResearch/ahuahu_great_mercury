# Adjusting profiles

If you want to experiment with different profiles you may wish to set the `__CA_ALLOW_INSTALLER_TO_OVERWRITE_EXISTING_INSTALLS__` option in setup.php. By default the installer will refuse to install over an existing installation. With `__CA_ALLOW_INSTALLER_TO_OVERWRITE_EXISTING_INSTALLS__` set the installer will include an option to overwrite existing data. In a real system this is extremely dangerous – any one with access to the installer can delete the entire system – but is very handy for testing and evaluation.

## Insstructions

ADD INSTRUCTIONS TO FLESH THIS OUT AND DETAIL HOW TO DO THIS.

Any changes to the profile used by Collective Access require a rebuild of the database
tables.

Make any necessary changes to the `.xml` profile file, and reboot the server.


Navigate to https://ca.uoaarchdb.cloud.edu.au/install/ to re-run the installer, and
make sure to select the new profile.