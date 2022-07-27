# in-place update instructions

These instruction should guide you through how to perform an in-place
update of a Collective Access instance from an older to newer version.

Please refer to mentioned links in case these instructions become deprecated.


see readme for details on inplace update https://github.com/collectiveaccess/providence/blob/master/README.md

```
$ cd /var/www/html/ca
$ git status
```

If dubious ownership, run: `git config --global --add safe.directory /var/www/html/ca`

```
$ git status . -- ':!vendor'
$ git describe --tags #(or git branch if branch named properly)
$ sudo git fetch --all --tags
$ git tag --sort=committerdate -l #(to see sorted order of tags)
$ mkdir pre-update
$ cp setup.php ./pre-update/
$ cp media ./pre-update/
$ cp app/conf/local/* ./pre-update/local
```
If there are any changes to `app/printTemplates/*/local` then copy these into `./pre-update` as well

 ```
$ sudo git -f checkout tags/X.Y.Z -b ca_X.Y.Z
$ sudo cp pre-update/setup.php ./setup.php
$ sudo cp -r pre-update/local ./app/conf/
$ sudo cp -r pre-update/media/ ./media/
$ sudo systemctl restart apache2.service
```