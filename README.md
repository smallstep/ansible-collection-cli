# Ansible Collection - smallstep.cli

This collection provides the `smallstep.cli.install` role which can be used to install the [step CLI](https://github.com/smallstep/cli) binary onto your servers. It uses the [smallstep.sigstore](https://github.com/smallstep/ansible-collection-sigstore) collection to verify the [Sigstore](https://sigstore.dev/) signatures which Smallstep uses to sign our software artifacts.

This collection currently supports:

* Fedora (Current Releases)
* Enterprise Linux (RHEL, CentOS Stream, Rocky Linux, Alma Linux, etc)
* Ubuntu (Current Stable and LTS releases)
* Debian (Current Releases)
* Arch Linux

We have some basic support for using this collection on Windows but it is untested. Use it at your risk!

## Requirements

* `ansible-galaxy collection install smallstep.sigstore` on control node
* `ansible-galaxy collection install ansible.windows` # Only needed for Windows installs (Untested!!)
* Python 3.8 or greater on servers
* `pip` installed on servers
* `pip install sigstore` on servers

## Role: smallstep.cli.install

### Role variables

```yaml
step_cli_version: # (Optional) Format: v0.2.24.4. Default: latest version
step_cli_install_path: # (Optional) Default: /usr/local/bin.
step_cli_download_url: # (Optional) Default: https://dl.smallstep.com/gh-release/cli/gh-release-header
step_cli_verify_signature: # (Optional) Default: True
```

### Example Playbook

```yaml
---
- hosts: all
  become: True

  collections:
    - smallstep.sigstore
    - smallstep.cli

  pre_tasks:
    - name: Make sure the current version of pip is installed.
      dnf:
        name: python3-pip
        state: latest

  roles:
    - role: smallstep.cli.install
      vars:
        # step_cli_version: v0.24.4
        # step_cli_install_path: /usr/local/bin
        # step_cli_download_url: https://dl.smallstep.com/gh-release/cli/gh-release-header
        # step_cli_verify_signature: True
```

## Playbook: smallstep.cli.install_step_cli

Assuming you have the following requirements: Python 3.8 or greater, `pip` and `pip install sigstore` installed on your on servers, you may easily run the collection playbook `smallstep.cli.install_step_cli` to install the most recent version of step CLI.

### Install the most recent version of step CLI

```bash
ansible-playbook smallstep.cli.install_step_cli -i ansible_inventory
```

### Install a specific version of step CLI to the path of your choice

```bash
ansible-playbook smallstep.cli.install_step_cli -i ansible_inventory -e "step_cli_version=v0.24.4" -e "step_cli_install_path=/usr/bin"
```

## Local development

### Setup Ansible Collections workspace

In your source code directory do the following:

```bash
mkdir ansible_collections
cd ansible_collections
git git@github.com:smallstep/ansible-collection-cli.git smallstep/cli
git clone git@github.com:smallstep/ansible-collection-sigstore.git smallstep/sigstore
git clone git@github.com:ansible-collections/ansible.windows.git ansible/windows
cd smallstep/cli
```

Then make your changes and then run the `ansible-test` commands in the Testing section.

## Testing

### ansible-test sanity

```bash
ansible-test sanity --docker --skip-test validate-modules
```

### ansible-test integration

```bash
ansible-test integration --docker
```

## Local install

### Install the collection dependencies

```bash
ansible-galaxy collection install git+https://github.com/smallstep/ansible-collection-sigstore.git
ansible-galaxy collection install ansible.windows # Only needed for Windows installs (Untested!!)
```

### Install the smallstep.cli collection

```bash
ansible-galaxy collection build --output-path /tmp --force
ansible-galaxy collection install /tmp/smallstep-cli-0.0.1.tar.gz --force
```

## Local uninstall

```bash
rm -rf ~/.ansible/collections/ansible_collections/smallstep/cli/
```

## License

[Apache License Version 2.0](http://www.apache.org/licenses/LICENSE-2.0)

Copyright 2023 Smallstep Labs Inc.
