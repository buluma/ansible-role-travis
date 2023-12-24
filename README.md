# [Ansible role travis](#travis)

Installs travis on your system.

|GitHub|Version|Issues|Pull Requests|
|------|-------|------|-------------|
|[![github](https://github.com/buluma/ansible-role-travis/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-travis/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-travis.svg)](https://github.com/buluma/ansible-role-travis/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-travis.svg)](https://github.com/buluma/ansible-role-travis/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-travis.svg)](https://github.com/buluma/ansible-role-travis/pulls/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-travis/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: buluma.travis
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-travis/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  gather_facts: yes
  become: yes
  vars:
    ruby_download_url: http://cache.ruby-lang.org/pub/ruby/3.0/ruby-3.0.0.tar.gz
    ruby_version: 3.0.0
    ruby_install_gems_user: root
    ruby_install_gems:
      - json
    ruby_gems_bin_path: /root/.gem/ruby/bin

  pre_tasks:
    - name: add rubygems bin dir to system-wide $PATH.
      ansible.builtin.copy:
        dest: /etc/profile.d/ruby.sh
        content: 'PATH=$PATH:{{ ruby_gems_bin_path }}'
        mode: 0644

    - name: don't install Bundler on CentOS 7 because of old Ruby version.
      ansible.builtin.set_fact:
        ruby_install_bundler: false
      when:
        - ansible_os_family == 'RedHat'
        - ansible_distribution_major_version == '7'

  roles:
    - role: buluma.bootstrap
    - role: buluma.buildtools
    # - role: buluma.ruby
    - role: buluma.ruby_gems
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.


## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-travis/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.buildtools](https://galaxy.ansible.com/buluma/buildtools)|[![Ansible Molecule](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-buildtools.svg)](https://github.com/shadowwalker/ansible-role-buildtools)|
|[buluma.ruby](https://galaxy.ansible.com/buluma/ruby)|[![Ansible Molecule](https://github.com/buluma/ansible-role-ruby/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-ruby/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-ruby.svg)](https://github.com/shadowwalker/ansible-role-ruby)|
|[buluma.ruby_gems](https://galaxy.ansible.com/buluma/ruby_gems)|[![Ansible Molecule](https://github.com/buluma/ansible-role-ruby_gems/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-ruby_gems/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-ruby_gems.svg)](https://github.com/shadowwalker/ansible-role-ruby_gems)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-travis/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Alpine](https://hub.docker.com/repository/docker/buluma/alpine/general)|all|
|[EL](https://hub.docker.com/repository/docker/buluma/enterpriselinux/general)|8|
|[Debian](https://hub.docker.com/repository/docker/buluma/debian/general)|all|
|[opensuse](https://hub.docker.com/repository/docker/buluma/opensuse/general)|all|
|[Ubuntu](https://hub.docker.com/repository/docker/buluma/ubuntu/general)|all|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-travis/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-travis/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-travis/blob/master/LICENSE).

## [Author Information](#author-information)

[buluma](https://buluma.github.io/)


### [Special Thanks](#special-thanks)

Template inspired by [Robert de Bock](https://github.com/robertdebock)
