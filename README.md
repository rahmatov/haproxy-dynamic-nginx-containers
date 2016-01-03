# HAProxy front end with scaleable nginx docker container backends.

This repository demonstrates how you can dynamically add an nginx container into
an [HAProxy](http://haproxy.com) load balanced server group.

The repository contains a Vagrantfile to create an haproxy server which will load
balance to another server running dockerized nginx conainers.

Ansible is used to provision the ubuntu hosts, and then to add new nginx containers
to the haproxy roundrobin.

## Requirements

This repository uses [Vagrant](https://docs.vagrantup.com/v2/installation/) for creating
the two virtual servers and [Ansible](http://docs.ansible.com/intro_installation.html) for
provisioning them. Refer to their documentation for installation details.

I used [VirtualBox](https://www.virtualbox.org/wiki/Downloads) for my virtualization provider.

## Setup

1. Clone this repository
2. Run `vagrant up`

At the end of the setup, you should have two ubuntu guests, one for haproxy at 192.168.111.224, and one
for the nginx containers. There will be two, serving a page from [http://192.168.111.225:5001/](http://192.168.111.225:5001/)
and one from [http://192.168.111.225:5002/](http://192.168.111.225:5002/).

You can curl the setup by doing:

curl http://192.168.111.224/ and the nginx server will return information about the conainter like so:

`This is me on nginx container nginx2 port 5002`

Each time you run the curl command, you should get content from the other container.

## Adding antoher nginx container

To add another nginx container to the mix, run the ansible play:

`ansible-playbook -i ansible/inventory ansible/add_another_nginx.yml`

This will spin up another nginx container on the nginx guest, add that nginx container to haproxy.cfg,
and reload haproxy.

Curling again several times with the command above will show that the container is now in the round robin.

## HAPROXY Stats

The HAProxy stats are served from [http://192.168.111.224:1936/](http://192.168.111.224:1936/)

## NOTES

* ansible uses the private keys generated from Vagrant for VirtualBox, if you use another virtualization application, you'll have to make changes.
* you may get caching if you test the streaming content from a web browser. I recommend using curl or wget to test the web content returned from the HAProxy front end.
