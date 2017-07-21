
# Ansible Role: Nagios

[![Build Status](https://travis-ci.org/networklore/ansible-role-nagios.svg?branch=master)](https://travis-ci.org/networklore/ansible-role-nagios)

Installs Nagios 4.1.1 from source. Once Nagios is installed you can login to http://ip-address/nagios/ using the username and password you configure in the nagios_users variable.

## Requirements

None.

## Role Variables

The variables you can configure are listed below. For the default settings you can look in `defaults/main.yml`.

    download_dir: /home/user/download/nagios

This is the directory where the downloaded files will be placed and extracted.

    nagiosurl: https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.1.1.tar.gz
    nagiossrc: nagios-4.1.1

The download url for Nagios along with the directory name which will be created when the source file is
decompressed to. I.e. with `tar -xzvf nagios-4.1.1.tar.gz`

    pluginsurl: http://www.nagios-plugins.org/download/nagios-plugins-2.1.1.tar.gz
    pluginssrc: nagios-plugins-2.1.1

Same as for Nagios but for the Nagios plugins.

    nagios_users:
      - user: nagiosadmin
        pass: Password123
      - user: tyrion
        pass: red_w1ne
      - user: daenerys
        pass: dragon_fir3

The users which should be allowed to login to the Nagios web GUI.

## Dependencies

None.

## Example Playbook

Install Nagios and setup the password for your nagiosadmin user.

    - hosts: monitoring-servers
      vars_files:
       - vars/main.yml    
      roles:
         - { role: networklore.nagios }

*Contents of vars/main.yml*:

    nagios_users:
      - user: nagiosadmin
        pass: S3cre7Passw0rd

## Upgrading

The role has automatic upgrade when you change the version with var: 

    nagios_version: 4.3.2

But the upgrade requires to remove some old files to, check [build-nagios.yml](tasks/build-nagios.yml) for details.

## Supported versions

Actually ubuntu/latest is failing in molecule tests: 

```shell
failed: [ansible-ubuntu-latest] (item=[u'apache2', u'php5-gd', u'libgd2-xpm-dev', u'libapache2-mod-php5', u'unzip']) => {"failed": true, "item": ["apache2", "php5-gd", "libgd2-xpm-dev", "libapache2-mod-php5", "unzip"], "msg": "No package matching 'php5-gd' is available"}
```

Error en molecule ubuntu/trusty y debian8

```
fatal: [ansible-trusty]: FAILED! => {"changed": false, "failed": true, "msg": "Failed to validate the SSL certificate for assets.nagios.com:443. Make sure your managed systems have a valid CA certificate installed. If the website serving the url uses SNI you need python >= 2.7.9 on your managed machine or you can install the `urllib3`, `pyOpenSSL`, `ndg-httpsclient`, and `pyasn1` python modules to perform SNI verification in python >= 2.6. You can use validate_certs=False if you do not need to confirm the servers identity but this is unsafe and not recommended. Paths checked for this platform: /etc/ssl/certs, /etc/pki/ca-trust/extracted/pem, /etc/pki/tls/certs, /usr/share/ca-certificates/cacert.org, /etc/ansible. The exception msg was: [Errno 1] _ssl.c:510: error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed."}
fatal: [ansible-debian8]: FAILED! => {"changed": false, "failed": true, "msg": "Failed to validate the SSL certificate for assets.nagios.com:443. Make sure your managed systems have a valid CA certificate installed. You can use validate_certs=False if you do not need to confirm the servers identity but this is unsafe and not recommended. Paths checked for this platform: /etc/ssl/certs, /etc/pki/ca-trust/extracted/pem, /etc/pki/tls/certs, /usr/share/ca-certificates/cacert.org, /etc/ansible. The exception msg was: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:581)."}
```


## License

BSD

## Author Information

This role was created in 2016 by [Patrick Ogenstad](http://networklore.com).
