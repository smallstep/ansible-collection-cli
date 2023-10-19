# smallstep.cli.install

This role can be used can be used to install the [step CLI](https://github.com/smallstep/cli) binary onto your servers. It uses the [smallstep.sigstore](https://github.com/smallstep/ansible-collection-sigstore) collection to verify the [Sigstore](https://sigstore.dev/) signatures which Smallstep uses to sign our software artifacts.

This role currently supports:

* Fedora (Current Releases)
* Enterprise Linux (RHEL, CentOS Stream, Rocky Linux, Alma Linux, etc)
* Ubuntu (Current Stable and LTS releases)
* Debian (Current Releases)
* Arch Linux

We have some basic support for using this role on Windows but it is untested. Use it at your risk!

## Requirements

* `ansible-galaxy collection install smallstep.sigstore` on control node
* `ansible-galaxy collection install ansible.windows` # Only needed for Windows installs (Untested!!)
* Python 3.8 or greater on servers
* `pip` installed on servers
* `pip install sigstore` on servers

## Role Variables

```yaml
smallstep_cli_version: # (Optional) Format: v0.2.24.4. It is empty by default. If it is left empty, the role will query GitHub's API to find the latest release.
smallstep_cli_install_path: # (Optional) Default: /usr/local/bin.
smallstep_cli_download_url: # (Optional) Default: https://dl.smallstep.com/gh-release/cli/gh-release-header
smallstep_cli_verify_signature: # (Optional) Default: True
```

## Example Playbook

Here's an example playbook for Enterprise Linux based servers. (Fedora, RHEL, CentOS Stream, Rocky Linux, Alma Linux, etc):

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
        # smallstep_cli_version: v0.24.4
        # smallstep_cli_install_path: /usr/local/bin
        # smallstep_cli_download_url: https://dl.smallstep.com/gh-release/cli/gh-release-header
        # smallstep_cli_verify_signature: True
```

## Author Information

* Joe Doss @jdoss
* Smallstep Engineering

## License

[Apache License Version 2.0](http://www.apache.org/licenses/LICENSE-2.0)

Copyright 2023 Smallstep Labs Inc.
