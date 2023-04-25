# Fedora IoT

This is an example repository to demonstrate how Ansible, Podman and Fedora IoT
can be used for a "pull-only" IoT/Edge device.

## Motivation

This repository was mostly created to support some IoT articles over at the
[while-true-do.io blog](https://blog.while-true-do.io). It is meant to hold
some IoT related code, which is used in the articles, but might see another use
in the future.

## Usage

The usage of the repository is explained in
[IoT/Edge - Fedora + Ansible + Podman](https://blog.while-true-do.io/iot-fedora-ansible-podman/).

### Requirements

Each host, that should consume this repository, needs Ansible installed. On
AlmaLinux, Rocky, CentOS and Fedora this can be done via:

```bash
dnf install ansible-core
```

### Initialize

On the desired host, the following command should be executed, once.

```bash
/usr/bin/ansible-pull \
  --url https://github.com/dschier-wtd/fedora-iot.git \
  --inventory inventory/hosts.yml \
  --checkout main \
  ansible/playbooks/install_requirements.yml \
  ansible/playbooks/configure_server.yml
```

You can check, if the services are enabled manually:

```bash
# check for ansible-pull services
$ systemctl status ansible-pull.service
$ systemctl status ansible-pull.timer

# Check for the started containers
$ systemctl status container-web.service
$ podman ps
```

### Later on

All further deployments will be coming from the playbooks, as provided in the
desired service units. Meaning, you only need to update the playbook and wait
until `ansible-pull` takes care.
