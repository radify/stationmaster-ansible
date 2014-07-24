StationMaster-ansible
=====================

Ansible playbook to support [https://github.com/radify/stationmaster](radify/stationmaster). Takes care of updating StationMaster's CSV

## What does it do?

StationMaster-ansible writes to stationmaster.csv - it can add and remove sites from StationMaster. You can include it into your playbook to automatically keep StationMaster up to date!

## Requirements

* Ansible 1.3 or above.
* A dev domain that all your sites are deployed to in the format `{branch}.{domain}`, e.g. `foo.dev.company.com`
* Servers that have some way of git cloning (to check out the [https://github.com/radify/stationmaster](radify/stationmaster repo))

## Role variables

* `base` - the base path you want to deploy this index to. e.g. '/opt/company/company-ui'
* `devdomain` - the domain that your development workspaces are on.
* `branch` - the branch that you are telling Stationmaster about

## Dependencies

None

## Boundaries

All StationMaster-ansible does is:

1. Check out StationMaster to `base`
1. Add and remove lines from StationMaster's data CSV.

So, this means that it's your responsibility to do things like:

* Set up the nginx or apache files to serve StationMaster
* Restart your web server
* Manage your domains, etc

## Instructions

### Installing StationMaster through Ansible Galaxy

```bash
ansible-galaxy install radify.stationmaster
```

### Example Playbook

Then, from within one of your playbooks, you could do something like creating a playbook `stationmaster.yaml` and using the `add` action:

```yaml
---
- hosts: all
  roles:
  - { role: radify.stationmaster, base: "/opt/stationmaster", devdomain: dev.company.com, branch: branch, protocol: 'https', action: 'add' }
```

Then you can do something like:

```bash
ansible-playbook -i path/to/custom-inventory stationmaster.yaml
```

Removing is symmetrical using the `remove` action:

```yaml
---
- hosts: all
  sudo: yes
  roles:
    - { role: stationmaster, action: 'remove', branch: branch }
```

## Without using Ansible Galaxy

You can also check out stationmaster-ansible to a directory and include it. If you don't want to use Ansible Galaxy for some reason, you can clone this repository and use an include statement from within a task. See the notes on [CONTRIBUTING.md](CONTRIBUTING.md) for details.


### Adding via StationMaster

Here's an example of checking out StationMaster to /opt/stationmaster and adding a branch to it:

```yaml
- include: /private/var/www/stationmaster-ansible/tasks/main.yml base=/opt/stationmaster devdomain=dev.company.com branch={{branch}} protocol='https' action=add
```

### Removing via StationMaster

This is symmetrical to the adding:

```yaml
- include: /private/var/www/stationmaster-ansible/tasks/remove.yml base=/opt/stationmaster devdomain=dev.company.com branch={{branch}} protocol='https' action=remove
```
