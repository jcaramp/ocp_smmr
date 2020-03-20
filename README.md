ocp_smmr
=========

This role implements tasks for add or delete a project as member in the Service Mesh Member Roll in Openshift, to be managed by Service Mesh.

Also, it is **idempotent**, because if you try to add a role that is yet a member of Service Mesh Member Roll, the role will not perform any action. And the same to delete action.

Requirements
------------

The below requirements are needed on the host that executes this role.

Minimum Ansible version: 2.8.0

Ansible modules:

- k8s
- k8s_auth
- k8s_info / k8s_facts

Python modules:

- python >= 2.7
- openshift >= 0.6
- PyYAML >= 3.11
- urllib3
- requests
- requests-oauthlib

Role Variables
--------------

Roles needed to use this role:

Variable | Description | Required | Choices/***Defaults***
------------ | ------------- | ------------- | -------------
api_url | Openshift API URL | yes | -
ocp_username |  Openshift Username | no | -
ocp_password | Openshift Password | no | - 
ocp_token | Openshift Service Account Access Token | no | -
ocp_verify_ssl | Verify SSL | no | ***true***, false
project_name | Openshift Project Name | yes | -
project_sm_name | Openshift project name of the Service Mesh Control Plane project | yes | -
smmr_name | Service Mesh Member Roll object name | yes **\*** | ***default***
action_smmr | Action to perform with project in Service Mesh Member Roll object | yes | ***add***, delete

**\*** If the Service Mesh Member Roll object has a different name than ***default*** you need to set ``smmr_name`` variable with the right value.

Dependencies
------------

No dependencies.

Example Playbook
----------------

This is an example using an API token to authenticate to **add** a project as member of Service Mesh Member Roll:

    - hosts: servers
      roles:
        - role: ocp_smmr
          vars:
            api_url: "https://openshift.example.com:6443"
            ocp_token: "{{ service_account_token }}"
            ocp_verify_ssl: false
            project_name: "example-project"
            project_sm_name: "istio-system"
            smmr_name: "default"
            action_smmr: "add"

And this is an example using an user/password to authenticate to **delete** a project as member of Service Mesh Member Roll:

    - hosts: servers
      roles:
        - role: ocp_smmr
          vars:
            api_url: "https://openshift.example.com:6443"
            ocp_username: "clusteradmin"
            ocp_password: "xxxxxxxxxxx"
            ocp_verify_ssl: true
            project_name: "example-project"
            project_sm_name: "istio-system"
            smmr_name: "default"
            action_smmr: "delete"

Platforms
------------

Tested on:

- Red Hat Enterprise Linux 7.7
- Red Hat Openshift Container Platform 4.2
- Red Hat Openshift Service Mesh 1.0

License
-------

GNU General Public License v3.0

Author Information
------------------

This role was written in 2020 by Jesús Carmona Ampuero