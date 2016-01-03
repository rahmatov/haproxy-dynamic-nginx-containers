# Ha Proxy and dynamically adding docker web server containers.

This repo demonstrates how you can dynamically add an nginx container into
an ha server load balancer group.

The repository contains a Vagrantfile to create an haproxy server which will load
balance to another server running dockerized nginx conainers.

Ansible is used to provision the ubuntu hosts, and then to add new nginx containers
to the haproxy roundrobin.

## Setup

Run `vagrant up`

At the end of the setup, you should have two ubuntu guests, one for haproxy at 192.168.111.224, and one
for the nginx containers. There will be two, serving a page from [http://192.168.111.225:5001/](http://192.168.111.225:5001/)
and one from [http://192.168.111.225:5002/](http://192.168.111.225:5002/).

You can curl the setup by doing:

curl http://192.168.111.224/ and the nginx server will return information about the conainter like so:

`This is me on nginx container nginx2 port 5002`

Each time you run the curl command, you should get content from the other container.

To add another nginx container to the mix, run:

`ansible-playbook -i ansible/inventory ansible/add_another_nginx.yml`

This will spin up another nginx container on the nginx guest, add that nginx container to haproxy.cfg,
and reload haproxy.

Curling again several times with the command above will show that the container is now in the round robin.

## HAPROXY Stats

The HAProxy stats are served from [http://192.168.111.224:1936/](http://192.168.111.224:1936/)

docker run --rm -v /usr/local/etc/haproxy/haproxy.cfg:/usr/local/etc/haproxy/haproxy.cfg -p "80:80" haproxy:1.5 haproxy -f /usr/local/etc/haproxy/haproxy.cfg

## NOTES

* ansible uses the private keys generated from Vagrant for VirtualBox, if you use another virtualization application, you'll have to make changes.
