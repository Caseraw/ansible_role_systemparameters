# Ansible role filemanager

Managing sysctl, kernel modules and limits configuration.

- [Ansible role filemanager](#ansible-role-filemanager)
  - [License](#license)
  - [Author Information](#author-information)
  - [Requirements](#requirements)
  - [Dependencies](#dependencies)
  - [Compatibility](#compatibility)
  - [Role Variables](#role-variables)
  - [Example Playbook](#example-playbook)
  - [Useful shell commands](#useful-shell-commands)
  - [Additional documentation resources](#additional-documentation-resources)
  - [Testing with Molecule](#testing-with-molecule)
  - [CI/CD with Travis CI](#cicd-with-travis-ci)
  - [Useful links](#useful-links)

## License

MIT / BSD

## Author Information

- Made and maintained by: [Kasra Amirsarvari](https://www.linkedin.com/in/caseraw)
- Ansible Galaxy community author: <https://galaxy.ansible.com/caseraw>
- Dockerhub community user: <https://hub.docker.com/u/caseraw>

## Requirements

- Ensure sufficient privileged permissions to manage `/etc/sysctl.conf` and files in `/etc/sysctl.d/`.
- Ensure sufficient privileged permissions to manage files in `/etc/modprobe.d/` and `/etc/modules-load.d/`.

## Dependencies

N/A

## Compatibility

Compatible with the following list of operating systems:

- CentOS 7
- CentOS 8
- RHEL 7.x
- RHEL 8.x

## Role Variables

| Variable name | Description |
|---------------|-------------|
| role_systemparameters_sysctl_list | Combined list of other lists that start with the name `role_systemparameters_sysctl_list_`. Each list contains files and parameters to manage. |
| role_systemparameters_modprobe_list | Combined list of other lists that start with the name `role_systemparameters_modprobe_list_`. Each list contains files and parameters to manage. |
| role_systemparameters_kernelmodules_list | Combined list of other lists that start with the name `role_systemparameters_kernelmodules_list_`. Each list contains files and parameters to manage. |

> Keep in mind that the list contain raw values for each line of the configuration. Please refer to the documentation of each specific part for further guidance. From this role point of view it only deploys the files from containing the raw parameters. This role does not contain any specific logic to help configuring each property. It just deploys files from the variable definitions given through the inventory.

## Example Playbook

```yaml
---
- name: Managing sysctl, kernel modules and limits configuration
  become: False
  gather_facts: False
  tasks:
    - import_role:
        name: ansible_role_systemparameters
      vars:
        role_systemparameters_sysctl_list_some_profile_01:
          - dest: /etc/sysctl.d/CIS.conf
            src: generic_conf_file.j2
            state: present
            owner: root
            group: root
            mode: '0644'
            parameters:
              - 'fs.suid_dumpable = 0'
              - 'kernel.randomize_va_space = 2'
              - 'net.ipv4.ip_forward = 0'
              - 'net.ipv4.conf.all.send_redirects = 0'
              - 'net.ipv4.conf.default.send_redirects = 0'
              - 'net.ipv4.conf.all.accept_source_route = 0'
              - 'net.ipv4.conf.default.accept_source_route = 0'
              - 'net.ipv4.conf.all.accept_redirects = 0'
              - 'net.ipv4.conf.default.accept_redirects = 0'
              - 'net.ipv4.conf.all.secure_redirects = 0'
              - 'net.ipv4.conf.default.secure_redirects = 0'
              - 'net.ipv4.conf.all.log_martians = 1'
              - 'net.ipv4.conf.default.log_martians = 1'
              - 'net.ipv4.icmp_echo_ignore_broadcasts = 1'
              - 'net.ipv4.icmp_ignore_bogus_error_responses = 1'
              - 'net.ipv4.conf.all.rp_filter = 1'
              - 'net.ipv4.conf.default.rp_filter = 1'
              - 'net.ipv4.tcp_syncookies = 1'
              - 'net.ipv6.conf.all.accept_ra = 0'
              - 'net.ipv6.conf.default.accept_ra = 0'
              - 'net.ipv6.conf.all.accept_redirects = 0'
              - 'net.ipv6.conf.default.accept_redirects = 0'
        role_systemparameters_modprobe_list_some_profile_01:
          - dest: /etc/modprobe.d/CIS.conf
            src: generic_conf_file.j2
            state: present
            owner: root
            group: root
            mode: '0644'
            parameters:
              - 'install cramfs /bin/true'
              - 'install freevxfs /bin/true'
              - 'install jffs2 /bin/true'
              - 'install hfs /bin/true'
              - 'install hfsplus /bin/true'
              - 'install squashfs /bin/true'
              - 'install udf /bin/true'
              - 'install vfat /bin/true'
              - 'install usb-storage /bin/true'
              - 'install firewire-core /bin/true'
              - 'install dccp /bin/true'
              - 'install sctp /bin/true'
              - 'install rds /bin/true'
              - 'install tipc /bin/true'
        role_systemparameters_kernelmodules_list_some_profile_01:
          - dest: /etc/modules-load.d/sample_configuration.conf
            src: generic_conf_file.j2
            state: present
            owner: root
            group: root
            mode: '0644'
            parameters:
              - '# Sample line'
        role_systemparameters_limits_list_some_profile_01:
          - dest: /etc/security/limits.d/CIS.conf
            src: generic_conf_file.j2
            state: present
            owner: root
            group: root
            mode: '0644'
            parameters:
              - '* hard core 0'

...
```

## Useful shell commands

N/A

## Additional documentation resources

N/A

## Testing with Molecule

This role is locally tested with the use of [Molecule](https://molecule.readthedocs.io/en/latest/), the configuration is located at: [molecule/default](molecule/default).  
The Molecule tests are run (using the [docker driver](https://molecule.readthedocs.io/en/latest/configuration.html#docker)) on [Dockerhub images](https://hub.docker.com/u/caseraw) built for this purpose:

- [CentOS](https://hub.docker.com/r/caseraw/ansible-molecule-centos)
- [Fedora](https://hub.docker.com/r/caseraw/ansible-molecule-fedora)

## CI/CD with Travis CI

This role uses [Travis CI](https://travis-ci.org/) to run online tests with the use of [Molecule](https://molecule.readthedocs.io/en/latest/) and pushes notifications to import the role into [Ansible Galaxy](https://galaxy.ansible.com/) once the tests are successful. The Travis CI configuration is located at the root of the Ansible role [.travis.yml](.travis.yml)

## Useful links

- GitHub repository: <https://github.com/Caseraw/ansible_role_systemparameters>
- Travis CI build status: <https://travis-ci.org/Caseraw/ansible_role_systemparameters>
- Ansible Galaxy role: <https://galaxy.ansible.com/caseraw/ansible_role_systemparameters>
