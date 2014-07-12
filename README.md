coreosx
=======

# Installation

CoreOS on OS X in three steps:

1. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) and [Vagrant](http://www.vagrantup.com/downloads.html).

2. Put the `coreosx` script somewhere on your path:

        curl https://raw.githubusercontent.com/cap10morgan/coreosx/1.0.0/coreosx > /usr/local/bin/coreosx
        chmod +x /usr/local/bin/coreosx

3. Run:

        coreosx shell
        docker version (or etcdctl ls or fleetctl list-machines)


This script acts as both an installer and as Virtual machine manager. On first run, it installs and starts a CoreOS virtual machine. It forwards ports 4001 and 7001 from the host's localhost interface to the VM for etcd clients to connect to. It also (optionally) adds an entry to /etc/hosts called `localcoreos` for accessing exposed ports from Docker containers, for example.

The virtual machine that Docker runs on is given the hostname `localcoreos`. For example, if you run `docker run -p 8000:8000 ...`, then that will be available at `localcoreos:8000` from OS X.

## Additional commands

`coreosx` provide additional commands as shortcuts for controlling the Vagrant VM:

### coreosx start

Start the local Virtual Machine

### coreosx ssh

Open a console on the Vagrant virtual machine.

### coreosx destroy

Destroy the local Virtual Machine

### coreosx halt

Stop the Vagrant VM. You'll probably want to do this after you've finished working with CoreOS to save RAM.

### coreosx shell

Start the virtual machine and open a shell with DOCKER_HOST environment variable configured.


## Override defaults

The coreosx script has several options that can be overridden by adding a
new file `$HOME/.coreosx/defaults`. When coreosx starts the VM, it will
source this file.

When modifying the defaults for coreosx, currently, it is best to destroy
any already-created VM and configure a new one with the changes.

An example `defaults` file follows:

```bash
# $HOME/.coreosx/defaults
COREOS_IP=192.168.228.10
```

### COREOS_IP

The IP that the docker host will be mapped to on your machine.

Default: `172.16.23.75`

### COREOS_DOMAIN

The domain name added to `/etc/hosts`, pointing at the `COREOS_IP`.

Default: `localcoreos`

### DOCKER_PORT

The port that docker will be listening on.

Default: `2375`

### COREOS_CHANNEL

The initial version of CoreOS will be installed from this release and updates will come from it afterwards.

Default: `alpha`

### VAGRANT_BOX_URL

The URL used to download the vagrant box.

Default: `http://${COREOS_CHANNEL}.release.core-os.net/amd64-usr/current/coreos_production_vagrant.json`

## Contributors

* [Wes Morgan](https://github.com/cap10morgan) - CoreOSX author
* [Julien Duponchelle](https://github.com/noplay/) – Original author of docker-osx, which this is adapted from
* [Ben Firshman](https://github.com/bfirsh) – Faster and simpler installation of docker-osx with Vagrant image and pre-built binary

## Licence

Copyright 2014 Wes Morgan

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
