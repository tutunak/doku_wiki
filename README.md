DokuWiki docker container
=========================


To run image:
-------------

	docker run -d -p 80:80 --name my_wiki mprasil/dokuwiki 

You can now visit the install page to configure your new DokuWiki wiki.

For example, if you are running container locally, you can acces the page 
in browser by going to http://127.0.0.1/install.php

### Run with bind mounts ###

The run command above will store your data in internal Docker volumes. If you prefer to bind mount directories on your file system, then you can use the `-v` parameter, for example:

    docker run -d -p 80:80 --name my_wiki \
        -v /data/docker/dokuwiki/data:/dokuwiki/data \
        -v /data/docker/dokuwiki/conf:/dokuwiki/conf \
        -v /data/docker/dokuwiki/lib/plugins:/dokuwiki/lib/plugins \
        -v /data/docker/dokuwiki/lib/tpl:/dokuwiki/lib/tpl \
        -v /data/docker/dokuwiki/logs:/var/log

### Run a specific version ###

When running the container you can specify version to download from docker registry by using couple provided tags like *stable* which contains current stable release (this is generaly the same as *latest*) or *oldstable*. You can also use specific version like *2016-06-26a*.


To upate the image:
-------------------

### With internal volumes ###

First stop your container

	docker stop my_wiki

Then run new container just to hold the volumes

	docker run --volumes-from my_wiki --name my_wiki_data busybox true

Now you can remove old container

	docker rm my_wiki

..and run a new one (you built, pulled before)

	docker run -d -p 80:80 --name my_wiki --volumes-from my_wiki_data mprasil/dokuwiki 

afterwards you can remove data container if you want

	docker rm my_wiki_data

(or keep it for next update, takes no space anyway..)

### With bind mounts ###

Just stop the old container and run the new one with the same `-v` mapping. Since the data is on your file system, you do not need to do anything special to preserve it between containers.

Optimizing your wiki
--------------------

Lighttpd configuration also includes rewrites, so you can enable 
nice URLs in settings (Advanced -> Nice URLs, set to ".htaccess")

For better performance enable xsendfile in settings.
Set to proprietary lighttpd header (for lighttpd < 1.5)

Build your own
--------------

	docker build -t my_dokuwiki .

