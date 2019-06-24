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
process. The servers are controlled through Jenkins on https://henkins.homebox.space, but this can also be done from the
command line, with an API key, like the examples below.

This is a huge “work-in-progress”, but it is enough for now to drive the tests of two flavours of homebox
instances. There is no dynamic inventories yet, and the IP addresses are hardcoded in the Jenkins jobs.
Using dynamic inventories will require creating or updating the DNS GLUE records and the DNSSEC keys (ZSK and KSK) on
the DNS provider as well.

### To start the environment

This will create the servers if they don't exists.

```sh
export VULTR_API_KEY='RAR56637DKULV27RML4ACCFFKQSR2TOV6FR8'
ansible-playbook -i config/hosts.yml -vv vultr/playbooks/environment-start.yml
```

### To stop the environment

```sh
export VULTR_API_KEY='RAR56637DKULV27RML4ACCFFKQSR2TOV6FR8'
ansible-playbook -i config/hosts.yml -vv vultr/playbooks/environment-stop.yml
```

The servers are automatically turned off after the tests.

### To re-install the environment

This will be used for the weekly tests, when the stack is installed from scratch.

```sh
export VULTR_API_KEY='RAR56637DKULV27RML4ACCFFKQSR2TOV6FR8'
ansible-playbook -i config/hosts.yml -vv vultr/playbooks/environment-reinstall.yml
```
