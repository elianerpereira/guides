# Building OSTree image

This section contains a guide for building OSTree commits. As opposed to the "traditional" image types, these commits are not self-sufficient and it is necessary to download Fedora installation ISO.



OSTree is a technology for creating immutable operating system images and it is a base for Fedora IoT and RHEL Edge. For more information on OSTree, see [their website]().



## Overview of the intended result

As mentioned above, osbuild-composer does not produce self-sufficient images for OSTree. Instead it produces an "OSTree commit" which is a tarball containing filesystem image and a metadata file (TODO: verify).

Additionally you will need:

* Fedora installation ISO - such as netinst

* HTTP server to serve the content of the tarball to the Fedora VM booted from the ISO

* Kickstart file that instructs Anaconda to use the OSTree commit from the HTTP server

In this guide, a container running httpd will be used as the HTTP server.

The result will look like this:

```
 _________________          ____________________
|                 |        |                    |
|                 |------> | Fedora VM from ISO |
|                 |        |  - Anaconda        |
|  Fedora Host OS |        |____________________|
|                 |                |
|                 |         _______|________________________
|                 |        |                                |
|                 |------->| Fedora container running httpd |
|_________________|        |  serving tarball and kickstart |
                           |________________________________|
```

## Building an OSTree commit

Start by creating a blueprint for your commit. Using your favorite text editor, vi, create a file named `fishy.toml` with this content:

```toml
name = "fishy-commit"
description = "Fishy OSTree commit"
version = "0.0.1"

[[packages]]
name = "fish"
version = "*"
```

Now push the blueprint to osbuild-composer using composer-cli:

```
$ composer-cli blueprints push fishy.toml
```

And start a build:

```
$ composer-cli compose start fishy-commit fedora-iot-commit
Compose 8e8014f8-4d15-441a-a26d-9ed7fc89e23a added to the queue
```

Monitor the build status using:

```
$ composer-cli compose status
```

And finally when the compose is complete, download the result:

```
$ composer-cli compose image 8e8014f8-4d15-441a-a26d-9ed7fc89e23a
8e8014f8-4d15-441a-a26d-9ed7fc89e23a-commit.tar: 587.60 MB
```



## Setting up an HTTP server

## Running a VM and applying the OSTree commit

