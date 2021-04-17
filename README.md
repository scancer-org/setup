# Setting up Scancer

This repository is a collection of configuration scripts that we use
to maintain the [scancer.org](https://scancer.org)
infrastructure. They setup all the necessary pieces including:

- Application servers for the primary Django webapp
- TorchServe API servers that handle ML inference
- A Postgresql database server to store application data
- Celery workers for distributed processing of tasks

The toolchain is designed to setup these services on varying
configurations of underlying hardware, from a single low-resource
server to a large cluster. When setup, it brings up the following
services:

1. The user-facing webapp at [scancer.org](https://scancer.org/)
2. The ML inference API at [api.scancer.org](https://api.scancer.org/)
3. The ML model store at [models.scancer.org](https://models.scancer.org/)
4. The ML monitoring interface at [monitoring.scancer.org](https://monitoring.cancer.org/)

Services (2)–(4) are only accessible to the team working on and
managing Scancer.

We have shared this toolchain with you so you can understand how to
setup your own copy of the service on your own servers.

## How to use these scripts

### Locally

You can install the entire stack locally on a single Ubuntu virtual
machine. For this, you need to first have the following tools:

1. [VirtualBox](https://www.virtualbox.org)
2. [Vagrant](https://www.vagrantup.com)
3. [Ansible](https://www.ansible.com)

When you've installed these tools, navigate to the folder you have
cloned this repository to and run

````
vagrant up
````

If you add the following entry to your local DNS (which is
`/etc/hosts` on macOS and Linux),

````
192.168.33.16   www.scancer-local.org
192.168.33.16   scancer-local.org
192.168.33.16   api.scancer-local.org
192.168.33.16   models.scancer-local.org
````

you should be able to navigate to `scancer-local.org` and see the site
functioning.

### In production

This is a bit more involved. You first need to decide the servers you
want to have to deal with the amount of traffic you're
expecting. Typically, having:

- A good server for the database
- Two servers for the webapp
- Two servers for the API
- Two servers as workers

should suffice.

Provision these handful of servers (running `Ubuntu 20.04`) in your
cloud provider of choice and note down the IP addresses of these
machines.

You can then edit `servers/prodduction.yml` to point to these
different serves. When done, run:

````
ansible-playbook site.yml -i servers/production
````

While you are waiting for this to finish, go to your DNS provider and
point your domain to the primary application server. When the Ansible
run finishes, you should be able to access the site.

## Copyright and license

Copyright (c) 2021 [Harish Narayanan](https://harishnarayanan.org) and
Daniel Hen.

This code is licenced under the MIT Licence. See
[LICENSE](https://github.com/scancer-org/setup/blob/main/LICENSE) for
the full text of this licence.
