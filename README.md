Storyteller
=========

Installs and configure Nginx, PHP5-FPM, Mysql and Wordpress in Ubuntu (trusty 14.04).

This role will apply some recommended security settings when setting up the Mysql database, like deleting anonymous user and the test database.
This role will also fetch random salts from the [wordpress salt API](https://api.wordpress.org/secret-key/1.1/salt/) to apply in the wordpress config file.

Requirements
------------

To run this role in your playbook, you need to set 2 environment variables: MYSQL_ROOT_PASS and WP_DB_PASS.
The first one is the desired password to the root user in Mysql. The second one is the password for the wordpress user in Mysql.

Role Variables
--------------

The password to root user in Mysql, the default value comes from the environment variable MYSQL_ROOT_PASS.
It's not recommended to override this variable to a fixed value in the playbook, use a environment variable instead.
```
storyteller_mysql_root_password: "{{ lookup('env', 'MYSQL_ROOT_PASS') }}"
```

The website URL to download the wordpress tar.gz (you should specify it to download the proper language):
```
storyteller_wp_file_url: "http://br.wordpress.org"
```

The tar.gz wordpress file full name (you should specify wordpress version and language here):
```
storyteller_wp_file: "wordpress-4.0-pt_BR.tar.gz"
```

The tar.gz wordpress file sha256 sum. To verify the downloaded file integrity:
```
storyteller_wp_sha256sum: "cd2ecbbc50aa2698eb13b01536a9f1f491dac670827d791432dabd94cc4bb28d"
```

The wordpress database name:
```
storyteller_wp_db_name: wordpress
```

The wordpress database user name:
```
storyteller_wp_db_user: wordpress
```

The password to wordpress user in Mysql, the default value comes from the environment variable WP_DB_PASS.
It's not recommended to override this variable to a fixed value in the playbook, use a environment variable instead.
```
storyteller_wp_db_password: "{{ lookup('env', 'WP_DB_PASS') }}"
```

The hostname of your new blog. Used in the nginx config file:
```
storyteller_hostname: localhost
```

Dependencies
------------

None

Example Playbook
----------------

    - hosts: blog-server
      remote_user: root
      roles:
         - { role: oesgalha.storyteller, storyteller_hostname: "http://my-awesome-blog.com" }

Testing
----------------

This repository comes with a Vagrantfile to boot a virtual machine and test the role in action.

To boot the machine and apply the role, execute this from the project root folder:
```
vagrant up
```

You can test the idempotency of the role by running it again using:
```
vagrant provision
```

And you can see the live result at localhost:8080

To turn the virtual machine off:
```
vagrant suspend
```

And to wipe it forever:
```
vagrant destroy
```

License
-------

MIT

Author Information
------------------

This role was created in 2014 by Oscar Esgalha.
