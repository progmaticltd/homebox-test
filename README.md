This repository contains testing material for HomeBox.

The files in this repository are used by the [HomeBox](https://github.com/progmaticltd/homebox) continuous integration
server at [jenkins.homebox.space](https://jenkins.homebox.space).

## Generic configurations

The folder configs/generic contains _generic configurations_ that can be used on any hardware or cloud provider, witout
any specificity.

## Support

For any question or suggestion, you can write an email to [support@homebox.space](mailto:support@homebox.space).


## Provisioning

This folder contains some of the code to control the testing environment on Vultr, and will be completed as an ongoing
process. The servers are controlled through Jenkins on https://jenkins.homebox.space, but this can also be done from the
command line, with an API key, like the examples below.

This is a huge “work-in-progress”, but it is enough for now to drive the tests of two flavours of homebox
instances. The playbooks is called by Jenkins, to run the continuous integration tests.

For the tests to run correctly, a docker-compose is used that provides a staging Acme server, a DNS server and a package
proxy cache. There is a repository to build the Jenkins continuous integration stack with a specific domain, hosted on
[GitHub](https://github.com/progmaticltd/homebox-jenkins).

This local repository does the following:

1. Start (optionally after reinstall) the virtual machine(s) needed for the tests
2. Activate the private IP address of the virtual machine
3. Attach the private IP address to the docker network of the continuous integration server

The Jenkins CI server is responsible of starting the installation and the integration tests.

### To start the environment

The following command example will create the _Buster_ servers if they don't exists, and start them:

```sh
export VULTR_API_KEY='RAR56637DKULV27RML4ACCFFKQSR2TOV6FR8'
ansible-playbook -vv -i ../config/hosts-local.yml -e env=vultr -e testing=buster playbooks/environment-start.yml
```

### To stop the environment

```sh
export VULTR_API_KEY='RAR56637DKULV27RML4ACCFFKQSR2TOV6FR8'
ansible-playbook -vv -i ../config/hosts-local.yml -e env=vultr -e testing=buster playbooks/environment-stop.yml
```

The servers are automatically turned off after the tests.

### To re-install the environment

This will be used for the weekly tests, when the stack is installed from scratch.

```sh
export VULTR_API_KEY='RAR56637DKULV27RML4ACCFFKQSR2TOV6FR8'
ansible-playbook -vv -i ../config/hosts-local.yml -e env=vultr -e testing=buster playbooks/environment-reisntall.yml
```

**Note:**

The repository is organised in a way that makes possible to create new cloud providers easily. Copy the _vultr_ folder,
with another provider name, create the code to start, stop and reinstall the virtual machines, and _voila_. The only
requirement is the virtual machines should able to reach the docker IP address of the continuous integration server.
