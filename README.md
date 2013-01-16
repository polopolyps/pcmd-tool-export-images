Export Images
==============

[fileserver]: https://github.com/polopolyps/fileserver "Fileserver"
[greenfield-online]: https://github.com/atex-polopoly/greenfield-online "Greenfield online"
[pcmd]: https://github.com/polopolyps/pcmd "pcmd"
[mysql documentation]:
http://dev.mysql.com/doc/refman/5.6/en/innodb-data-log-reconfiguration.html
"Mysql documentation"

Example [pcmd][] tool for exporting image file data into an external file
server. This tool will have to be modified in order to use with
something other than a vanilla [greenfield-online][] using
[fileserver][] as external image database.

**This tool is destructive without ability to revert changes, backup
   the database before attempting to use it.**

**This tool will remove old versions of the content to allow space to
be reclaimed, but since MYSQL never releases storage by default you
will have to manually prune the database after the extraction (eg
export the database, drop it, and then import it again, see the
[mysql documentation][] for more information).**

### Installation

    mvn clean install

Will produce a jar file in the target directory, just put the jar file
on the classpath of [pcmd][], and you should be good to go. Verify
installation by running:

     pcmd help export-images

### Example Usage

To extract ids for all images using "example.Image" as input-template run:

    pcmd search --inputtemplate=example.Image | pcmd extract-images

If you need to extract a large number of images it might be practical to
do this in batches.

Use this to create files with a suitable number of content ids in:

    pcmd search --inputtemplate=example.Image | split -l <number of ids in each file> -a 5 - images-

then you can process them one by one using:

    for file in `ls image*`; do bin/pcmd export-images
    --fileServerUrl=<fileserver url for uploading images> < $file && mv $file DONE-$file ; done

If something fails you should be able to resume the processing by just
issuing the previous command again.
