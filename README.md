Export Images
==============

Example PCMD tool for exporting image file data into an external file
server.

Run:

    pcmd help export-images

to see the available parameters.

Example Usage:

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
