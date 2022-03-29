# rally playground

This is a playground for fiddling with rally. Idea is to replace the
OpenStack health monitor with rally tasks. Let's see if this will work
out. ;)

## getting started

First get the docker container up and running:

``` bash
$ docker pull xrally/xrally-openstack
```

Next point rally at the existing OpenStack deployment. Prepare your
credentials via json:

``` json
{
    "openstack": {
        "auth_url": "https://api.gx-scs.sovereignit.cloud:5000/v3/",
        "region_name": "RegionOne",
        "endpoint_type": "public",
        "users": [
            {
                "username": "rally-demo@pco.invalid",
                "password": "password",
                "user_domain_name": "pco",
                "project_name": "amazing-rally-demo",
                "project_domain_name": "pco"
            }
        ],
        "https_insecure": false,
        "https_cacert": ""
    }
}
```

change your working directory to this git repo & enter the container interactively:

``` bash
$ cd rally-foo

$ docker run --rm --name openstack-rally -ti --entrypoint /usr/bin/bash \
     -v `pwd`:/home/rally/rally-foo \
     -v rally_volume:/home/rally/.rally \
     -w /home/rally/rally-foo \
        xrally/xrally-openstack
```

Import json into rally:

``` bash
$ rally deployment create --filename /tmp/credentials.json --name=rally-demo1
+--------------------------------------+----------------------------+-------------+------------------+--------+
| uuid                                 | created_at                 | name        | status           | active |
+--------------------------------------+----------------------------+-------------+------------------+--------+
| cf11c5f6-0f07-40fe-90f7-8a3abf8be165 | 2022-03-29T09:29:31.472568 | rally-demo1 | deploy->finished |        |
+--------------------------------------+----------------------------+-------------+------------------+--------+
Using deployment: cf11c5f6-0f07-40fe-90f7-8a3abf8be165
~/.rally/openrc was updated

HINTS:

* To use standard OpenStack clients, set up your env by running:
        source ~/.rally/openrc
  OpenStack clients are now configured, e.g run:
        openstack image list
```

You can check everything works:

``` bash
$ rally deployment check

--------------------------------------------------------------------------------
Platform openstack:
--------------------------------------------------------------------------------

Available services:
+-------------+-----------------+-----------+
| Service     | Service Type    | Status    |
+-------------+-----------------+-----------+
| __unknown__ | compute_legacy  | Available |
| __unknown__ | placement       | Available |
| barbican    | key-manager     | Available |
| cinder      | volumev3        | Available |
| cloud       | cloudformation  | Available |
| designate   | dns             | Available |
| glance      | image           | Available |
| heat        | orchestration   | Available |
| keystone    | identity        | Available |
| magnum      | container-infra | Available |
| neutron     | network         | Available |
| nova        | compute         | Available |
| octavia     | load-balancer   | Available |
| swift       | object-store    | Available |
+-------------+-----------------+-----------+
```

## rally vs openstack-rally vs xrally vs xrally-openstack

*Summary*

-   Opendev and Github repo are mirrors of each other. Same number of
    commits.
-   Xrally are built containers based on the Dockerfile in rally
    projects.
-   Xrally Github projects are just garbage

### Rally projects

| Project         | Link                                          | Mirror                                        | Description                                                    |
|-----------------|-----------------------------------------------|-----------------------------------------------|----------------------------------------------------------------|
| Openstack Rally | https://opendev.org/openstack/rally           | https://github.com/openstack/rally            | Rally project with origin Dockerfile / without rally_openstack |
| Openstack Rally | https://opendev.org/openstack/rally-openstack | https://github.com/openstack/rally-openstack/ | Rally project with xrally Dockerfile / with rally_openstack    |

### Docker container

| Container               | Source                                       |
|-------------------------|----------------------------------------------|
| xrally/xrally-openstack | https://github.com/openstack/rally-openstack |
| xrally/xrally           | https://github.com/openstack/rally           |

### Garbage

| Link                                       |
|--------------------------------------------|
| https://github.com/xrally/xrally-openstack |
| https://github.com/xrally/xrally           |
