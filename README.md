# wazo-ansible

## Prerequisites for all recipes

* Enough machines running with Debian Buster vanilla
* You can become root on the target machines (See https://docs.ansible.com/ansible/latest/user_guide/become.html)
* Ansible 2.7.9: `pip install ansible==2.7.9`

## Class 4: SBC component

* Edit `inventories/sbc` and set your host in `[sbc_host]`
* Run:

```shell
ansible-galaxy install -r requirements-postgresql.yml

ansible-playbook -i inventories/sbc c4-sbc.yml
```

## Class 4: Router/RTPEngine component

* Edit `inventories/router` and set your host in `[router_host]`
* Run:

```shell
ansible-galaxy install -r requirements-postgresql.yml

ansible-playbook -i inventories/router c4-router.yml
```

## Variables

* `wazo_locale` if defined, ensure the locale is set and
  generated. Must be an `UTF-8` locale.

### wazo-consul

* `wazo_consul_host`: (default `localhost`) host running the Consul server.
* `wazo_consul_port_scheme`: (default `http`) which port of the Consul server to use.
* `wazo_consul_port_http`: (default `8500`) HTTP port of the Consul server.
* `wazo_consul_port_https`: (default `8501`) HTTPS port of the Consul server.

### wazo-c4-router

* `debian_upgrade_first`: (default: `true`) do we `apt-get dist-upgrade` before installing Wazo Router?
* `router_api_endpoint_confd`: (default: `http://localhost:8000`) URI of the wazo-router-confd service
* `router_api_db_host`: (default: `localhost`) PostgreSQL host for wazo-router-confd
* `router_api_db_port`: (default: `5432`) PostgreSQL port for wazo-router-confd
* `router_api_db_name`: (default: `wazo`) database name for wazo-router-confd
* `router_api_db_user`: (default: `wazo`) database username for wazo-router-confd
* `router_api_db_password`: (default: `wazo`) database password for wazo-router-confd
* `router_api_redis_host`: (default: `localhost`) Redis host for wazo-router-confd
* `router_api_redis_port`: (default: `6379`) Redis port for wazo-router-confd
* `router_api_redis_database`: (default: `1`) Redis database for wazo-router-confd
* `router_interface`: (default: `{{ ansible_default_ipv4.interface }}`) network interface for Kamailio
* `router_redis_dialog`: (default: `1`) enables redis-based dialog replication
* `router_dburl_dialog`: (default: `redis://localhost:6379/2`) Redis uri to store dialogs
* `rtpengine_interface`: (default: `{{ ansible_default_ipv4.interface }}`) network interface for RTPEngine
* `rtpengine_private_address`: (default: `{{ ansible_default_ipv4.address }}`) private IP address for RTPEngine
* `rtpengine_public_address`: (default: `{{ ansible_default_ipv4.address }}`) public IP address for RTPEngine

### wazo-c4-sbc

* `debian_upgrade_first`: (default: `true`) do we `apt-get dist-upgrade` before installing Wazo Router?
* `sbc_advertise_address`: (default: `not set`) the advertised address for Kamailio, optional
* `sbc_advertise_port`: (default: `not set`) the advertised port for Kamailio, optional
* `sbc_interface`: (default `{{ ansible_default_ipv4.interface }}`) network interface for Kamailio
* `sbc_dispatcher_list`: (`1 sip:localhost:5060 16 10"`) dispatcher list configuration, replace `localhost` with the address of your router component
* `sbc_redis_dialog`: (default: `1`) enables redis-based dialog replication
* `sbc_dburl_dialog`: (default: `redis://localhost:6379/3`) Redis uri to store dialogs

### b2bua

* `b2bua_host`: (default: `localhost`) where other services should contact the B2BUA
* `b2bua_listen_address`: (default: `127.0.0.1`)
* `b2bua_port_ami`: (default: `5038`) TCP port for AMI
* `b2bua_port_http`: (default: `5039`) TCP port for HTTP interfaces
* `b2bua_port_sysconfd`: (default: `8668`) TCP port for wazo-sysconfd HTTP interface
* `b2bua_https_cert`: custom certificate filename for HTTPS
* `b2bua_https_private_key`: custom private key filename for HTTPS
* `b2bua_ami_permit_client_address`: (default: `127.0.0.1`)
* `b2bua_ami_permit_client_mask`: (default: `255.255.255.255`)
* `b2bua_listen_address`: (default: `127.0.0.1`)

### database

* `postgresql_port`: (default: `5432`) TCP port for PostgreSQL
* `postgresql_superuser_password`: password for superuser `postgres`
* `postgresql_listen_addresses`: (default: `127.0.0.1`)

### engine_api

* `engine_api_host`: (default: `localhost`) where other services should contact the engine API
* `engine_api_port`: (default: `443`) TCP port for HTTPS API
* `engine_api_port_confgend`: (default: `8669`) TCP port for wazo-confgend
* `engine_api_https_cert`: custom certificate filename for HTTPS
* `engine_api_https_private_key`: custom private key filename for HTTPS
* `engine_api_db_host`: (default: `localhost`) PostgreSQL host
* `engine_api_db_port`: (default: `5432`) PostgreSQL port
* `engine_api_db_admin_user`: (default: `postgres`) PostgreSQL superuser username
* `engine_api_db_admin_password`: PostgreSQL superuser password
* `engine_api_db_auth_name`: (default: `asterisk`) database name for wazo-auth
* `engine_api_db_auth_user`: (default: `asterisk`) database username for wazo-auth
* `engine_api_db_auth_password`: (default: `proformatique`) database password for wazo-auth
* `engine_api_db_confd_name`: (default: `asterisk`) database name for wazo-confd
* `engine_api_db_confd_user`: (default: `asterisk`) database username for wazo-confd
* `engine_api_db_confd_password`: (default: `proformatique`) database password for wazo-confd
* `engine_auth_path`: (default: `/api/auth/0.1`)
* `engine_confd_path`: (default: `/api/confd/1.1`)
* `engine_setupd_path`: (default: `/api/setupd/1.0`)
* `engine_api_listen_address`: (default: `127.0.0.1`)
* `engine_api_configure_wizard`: (default: `false`) run the configuration wizard. When `true`, `engine_api_root_password` must be set.
* `api_client_name`: (default: `api-client`) client name to manage the api. Used when `engine_api_configure_wizard` is `true`.
* `api_client_password`: (default: `api-password`) password for `api_client_name`. Used when `engine_api_configure_wizard` is `true`.
* `engine_api_root_password`: password for engine superuser `root`. Used when `engine_api_configure_wizard` is `true`.
* `engine_language`: (default: `en_US`). Used when `engine_api_configure_wizard` is `true`.
* `tenant_name`: (default: `my-company`) first tenant to create. Used when `engine_api_configure_wizard` is `true`.
### runtime

* `runtime`: (default: `true`) set to `true` when services are running while
  Ansible is executing. Set to `false` when Ansible must only make file
  modifications and no services are running (e.g. at docker build time)
