Next Generation HHVM Binary Packaging
=====================================

All linux builds will be in Docker containers, on AWS.

Configuration
=============

Docker containers will have the following directories bind-mounted:

 - `/var/out`: read-write; build artifacts (e.g. packages) go here.
 - `/opt/hhvm-packaging`: read-only; this directory.
 - `/opt/hhvm-distro-packaging`: read-only; the subdirectory for your
   distribution. This should contain a `make-package` script or symlink

For example, when building for Debian Jessie, the `debian-8-jessie/`
subdirectory is mounted to /opt/hhvm-distro-packaging.

The package building process will execute
`/opt/hhvm-distro-packaging/make-package` in the container, and will
expect that to create packages in `/var/out`. `make-package` should install
all required build dependencies.

Debian-like distributions
-------------------------

You probably want `make-package` to be a symlink to `/opt/hhvm-packaging/bin/make-debianish-package`; this expects:

 - a `DISTRIBUTION` file containing the string name for the distribution - e.g. 'jessie', 'trusty'
 - a `PKGVER` file containing the *package* version - e.g. `1~jessie`
 - a `debian/` subdirectory, containing `control`, `rules`, etc.

Packages will be build with `debbuild`

Local usage
===========

1. [Install Docker](https://www.docker.com/get-docker)
2. run `bin/interactive-container`; you now have a shell in the container
3. within the container, install git (e.g. `apt-get update -y; apt-get install -y git`)
4. run `/opt/hhvm-packaging/bin/make-source-tarball`
5. run `/opt/hhvm-distro-packaging/make-package`