# Ansible Role: nomad

An Ansible Role that installs and configures `nomad`.

**NOTE**: this role uses the integrated package system for the folowing OS:
- OpenBSD

[![Actions Status](https://github.com/tristan-weil/ansible-role-nomad/workflows/molecule/badge.svg?branch=master)](https://github.com/tristan-weil/ansible-role-nomad/actions)

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| nomad_config | a dictionnary of parameters to configure nomad 

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| nomad_certs_dir | /etc/ssl/nomad | the path where SSL certs are stored |
| nomad_config_tls_files | [] | a list of certs to store |

If the package system is not used, the following variables are available to handle directly the artifacts:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| nomad_artifact_version | 0.11.1 | the version |
| nomad_artifact_url | https://releases.hashicorp.com/nomad/....zip | the path to the artifacts to install |
| nomad_artifact_sum_url  | https://releases.hashicorp.com/nomad/..._SHA256SUMS | the path to the SUMS file |
| nomad_artifact_signature_url | https://releases.hashicorp.com/nomad/..._SHA256SUMS.sig | the path to the SUMs signatures file| 
| nomad_artifact_sum_algo | sha256 | the hash algorithm used in `nomad_artifact_sum_url` |
| nomad_pgp_key | ... | the PGP key of the project |
| nomad_artifact_version_flag_path | /etc/nomad.version | the files prevents reinstallation of the artifacts |
| nomad_artifact_tmpdir | /tmp/nomad | the installation work directory |
| nomad_force_update | False | *True/False* to force the update of the artifacts |
| nomad_daemon_nofiles | infinity | the maximum number of files the daemon can open |
| nomad_daemon_nproc | infinity | the maximum number of processes the daemon can have |
| nomad_daemon_tasksmax | infinity | the maximum number of tasks the daemon can have |

## Example Playbook

    - hosts: 'webservers'
      tasks:
        - import_role: 
            name: 'ansible-role-nomad'
          vars:
            nomad_config:
              datacenter: 'bi-sbg'
              region: 'eu'
              disable_anonymous_signature: 'true'
              disable_update_check: 'true'
              enable_syslog: 'true'
              leave_on_interrupt: 'true'
              leave_on_terminate: 'true'
              name: "{{ ansible_facts['hostname'] }}"
              bind_addr: "{{ '{{' }} GetInterfaceIP `eth0` {{ '}}' }}"
              log_level: 'INFO'
              client:
                enabled: 'true'
                node_class: 'prod'
                network_interface: "eth0"
              addresses:
                http: '127.0.0.1'
              advertise:
                http: '127.0.0.1'
              consul:
                address: '127.0.0.1:8500'
                server_service_name: 'nomad'
                client_service_name: 'nomad-client'
                auto_advertise: 'true'
                server_auto_join: 'true'
                client_auto_join: 'true'
              tls:
                rpc: 'true'
                ca_file: "{{ nomad_certs_dir }}/nomad-ca.pem"
                cert_file: "{{ nomad_certs_dir }}/nomad-client.pem"
                key_file: "{{ nomad_certs_dir }}/nomad-client-key.pem"
                verify_server_hostname: 'true'
              telemetry:
                collection_interval: '1s'
                disable_hostname: 'true'
                prometheus_metrics: 'true'
                publish_allocation_metrics: 'true'
                publish_node_metrics: 'true'

## Todo

None.

## Dependencies

See [requirements_galaxy.yml](https://github.com/tristan-weil/ansible-role-nomad/blob/master/requirements_galaxy.yml)

## Supported platforms

See [meta/main.yml](https://github.com/tristan-weil/ansible-role-nomad/blob/master/meta/main.yml)

## License

See [LICENSE.md](LICENSE.md)
