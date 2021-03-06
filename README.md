Role Name
=========

A role to help set the inactivity timeout on various OpenShift Container Platform 4.x oauthclients. 

Requirements
------------

The controller on which the playbookis run needs to have the OpenShift Command Line Interface (CLI) binary installed nad have a network connectivity to the api.

Role Variables
--------------

- ansible_name_module: The name this role or of the module/playbook the role is part of (default to the string name of the role).
- openshift_cli: The path to the CLI binary used on the controller host (default to 'oc').
- ocp_cluster_user: The cluster admin user that will be used to perform the update.
- ocp_cluster_user_password: The password of the admin user listed above.
- ocp_cluster_console_url: The URL of the api server to use to interact with the cluster. 
- ocp_cluster_console_port: The api port of the cluster (default to 6443).
- oauthclients_to_update: The various oauthclients to be updated. This is a dictinary with a key representing the client to update and a boolean value for the attribute/value update.
    - update: Flag to indicate if this client needs to be updated
    - name: Actual name of resource 
    - client_secret: The secret that applies to the client. Only used when create a new yaml file to apply instead of the patch command.
    - console_url: The FQDN of the console if applicable. Only used when create a new yaml file to apply instead of the patch command.
    - oauth_server_url: The FQDN or route to the oauth server if applicable. Only used when create a new yaml file to apply instead of the patch command.
- staging_dir: The directory where the rendered yaml config is placed on the controller to be used to update the config (default to /tmp/). 
- client_inactivity_timeout_seconds: The inactivity timeout being set for the client.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

To update or set the inactivity timeout for the console client use the following sample play. 
Note that the update key in the oauthclients_to_update dictionary for the console is set to true and that is why it is not being overriden here. 

    - hosts: localhost
      vars:
        oauthclients_to_update:
          oauthserver:
            update: false
            name: 'cluster'
            timeout: '15m'
          console-client:
            update: true
            name: 'console'
            timeout: '<your-desired-timeout-value-in-seconds>'
            client_secret: '<your-console-oauthclient-secret>'
            console_url: '<your-console-route>'
            oath_server_url: '<your-oauth-server-route>'
          browser-client:
            update: false
            name: 'openshift-challenging-client'
            timeout: '<your-desired-timeout-value-in-seconds>'
            client_secret: '<your-browser-oauthclient-secret>'
            console_url: ''
            oath_server_url: '<your-oauth-server-route>'
          challenging-client:
            update: false
            name: 'openshift-browser-client'
            timeout: '<your-desired-timeout-value-in-seconds>'
            client_secret: ''
            console_url: ''
            oath_server_url: '<your-oauth-server-route>'
        ansible_python_interpreter: /usr/bin/python3
        openshift_cli: '/usr/bin/oc'
        ocp_cluster_user: '<your-cluster-admin-user>'
        ocp_cluster_user_password: '<your-cluster-admin-user-password>'
        ocp_cluster_console_url: 'https://<your-api-route>'
        ocp_cluster_console_port: '6443'
      roles:
         - { role: config-inactivity-timeout-on-ocp4 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
