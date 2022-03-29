# rally playground

This is a playground for fiddling with rally. 
Idea is to replace the OpenStack health monitor with rally tasks. Let's see if this will work out. ;)

## getting started

First get the docker container up and running:

```
# docker pull xrally/xrally-openstack

# docker run --rm --name openstack-rally -ti --entrypoint /usr/bin/bash -v rally_volume:/home/rally/.rally xrally/xrally-openstack
```

Next point rally at the existing openstack deployment. This can be done with an existing OpenStack RC file.
Make sure the `OS_PROJECT_DOMAIN_NAME` is set in the RC file.

```
$ . openrc admin admin
$ rally deployment create --fromenv --name=existing
```

Example OpenStack rc file for the gx-scs environment:

```
export OS_AUTH_URL=https://api.gx-scs.sovereignit.cloud:5000
export OS_PROJECT_ID=<os_project_id>
export OS_PROJECT_NAME=<os_project_name>
export OS_USER_DOMAIN_NAME="keycloak"
export OS_PROJECT_DOMAIN_ID=<os_project_domain_id>
export OS_REGION_NAME="RegionOne"
export OS_INTERFACE=public
export OS_IDENTITY_API_VERSION=3
export OS_PROJECT_DOMAIN_NAME='keycloak'
```

You can check everything works:

```
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

