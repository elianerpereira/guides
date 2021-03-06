# Basic concepts

osbuild-composer works with a concept of **blueprints**. A blueprint is a description of the final **image** and its **customizations**. A **customization** can be an additional RPM package, enabled service, custom kernel command line parameter, and many other (see Blueprint reference). An **image** is defined by its blueprint and **image type**, which is for example qcow2 (QEMU Copy On Write disk image) or AMI (Amazon Machine Image).

Finally, osbuild-composer also supports **upload targets** which are cloud providers where an image can be stored right after it is built. Currently supported providers are [AWS](https://aws.amazon.com/) and [Azure](https://azure.microsoft.com/).

## Example blueprint

```toml
name = "base-image-with-tmux"
description = "A base system with tmux"
version = "0.0.1"

[[packages]]
name = "tmux"
version = "*"
```

The blueprint is in [TOML format](https://toml.io/en/).

## Blueprints management using composer-cli

osbuild-composer provides a storage for blueprints. To store a blueprint file `blueprint.toml` run this command:

```
$ composer-cli blueprints push blueprint.toml
```

To verify that the blueprint is available, list all currently stored blueprints and display the one which you have just added:

```
$ composer-cli blueprints list
base-image-with-tmux
$ sudo composer-cli blueprints show base-image-with-tmux
name = "base-image-with-tmux"
description = "A base system with tmux"
version = "0.0.1"
modules = []
groups = []

[[packages]]
name = "tmux"
version = "*"
```

## Image types

osbuild-composer supports various types of output images. To see all supported types run:

```
$ composer-cli compose types
```
