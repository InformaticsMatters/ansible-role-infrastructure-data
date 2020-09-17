Ansible Role - informaticsmatters.infrastructure_data
=====================================================

[![Build Status](https://travis-ci.com/InformaticsMatters/ansible-role-infrastructure-data.svg?branch=master)](https://travis-ci.com/InformaticsMatters/ansible-role-infrastructure-data)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/informaticsmatters/ansible-role-infrastructure-data)
![Ansible Role](https://img.shields.io/ansible/role/45912)

[![CodeFactor](https://www.codefactor.io/repository/github/informaticsmatters/ansible-role-infrastructure-data/badge)](https://www.codefactor.io/repository/github/informaticsmatters/ansible-role-infrastructure-data)

A Kubernetes-based Role for the configuration of a pre-deployed infrastructure.
This role provides actions to add, remove and alter PostgreSQL databases
in the infrastructure deployment.
cluster.

Requirements
------------

-   Kubernetes

Role Variables
--------------

    # An infrastructure configuration action (an action on the database).
    # One of: -
    #
    # - 'create' To create a database and owner
    # - 'delete' To delete a database and owner
    # - 'alter' To alter a database owners's password
    #
    # The infrastructure must be deployed before this role is executed.
    # The user must also have a destination namespace, where secrets
    # will be deployed by this role.
    id_action: create
    
    # The namespace of the deployed infrastructure,
    # where the database can be expected to be found, along with
    # its secrets.
    # (required for all actions)
    id_infra_namespace: ''
    
    # Variables to control the chosen action: -
    #
    # The database name
    # (required for all actions)
    id_db: ''
    
    # The user's namespace and service account
    # (required for 'create' and 'alter')
    #
    # A secret called 'database-secrets-<id_db>' will be deposited
    # in the user's namespace and it will contain the following fields: -
    #
    # - database_name
    # - database_user
    # - database_user_password
    id_db_user_namespace: ''
    
    # The user's namespace service account
    # (required for all actions)
    # The config Jobs run in the user's namespace with this service account.
    id_db_user_namespace_sa: ''
    
    # The database user (owner)
    # (required for 'create' and 'alter')
    # Randomly generated if not defined
    id_db_user: "{{ lookup('password', '/dev/null length=8 chars=ascii_letters') }}"
    
    # The database user password
    # (required for 'create' and 'alter')
    # Randomly generated if not defined
    id_db_user_password: "{{ lookup('password', '/dev/null length=12 chars=ascii_letters,digits') }}"

    # On removal the secrets (in the user namespace)
    # are normally expected to exist. If they do not
    # (i.e. you've deleted them by accident) then
    # set the following to 'no' to skip the built-in assertion.
    id_check_user_secrets_on_delete: yes
    
Dependencies
------------

-   (none)

Example Playbook
----------------

**NOTE** The example below assumes that you have a running Kubernetes
cluster.

    - hosts: servers
      tasks:
      - include_role:
          name: informaticsmatters.infrastructure_data
        vars:
          id_action: create
          id_db: blob
          id_db_user: alan
          id_db_user_password: blob1234
          id_db_user_namespace: example
          id_db_user_namespace_sa: blob
          id_infra_namespace: im-infra

License
-------

Apache 2.0 License

Author Information
------------------

alanbchristie
