
# Dell EMC Cinder Backend Deployment Guide for Red Hat OpenStack Platform

## Overview

This document describes how to deploy the Dell EMC Block Storage services in a Red Hat OpenStack Platform Overcloud.
This document assumes the RHOSP is installed and managed by RHOSP Director toolset which is based primarily on the on the upstream TripleO project.  

This document mainly covers the Dell EMC storage backends that are not yet fully integrated with Director through Tripleo like
* [Dell EMC XtremIO Block Storage driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-emc-xtremio-driver.html)
* [Dell EMC PowerMax iSCSI and FC drivers](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-emc-powermax-driver.html)
* [Dell EMC SC Series Fibre Channel driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-storagecenter-driver.html)

The following Dell EMC storage drivers that are fully integrated with director and can be deployed using tripleo heat templates include
* [Dell EMC SC Series iSCSI driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-storagecenter-driver.html)
* [Dell EMC Unity driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-emc-unity-driver.html)
* [Dell EMC VNX driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-emc-vnx-driver.html)


### Prerequisites
- Red Hat OpenStack Platform (RHOSP) overcloud deployed through director.
- Dell EMC Storage Backend configured as storage repositories.
- Mapping of the configuration you want for the specific storage backend in /etc/cinder/cinder.conf

### Deployment Steps

#### Prepare the mapping
Map out the resulting /etc/cinder/cinder.conf file for the choosen backend storage configuration.

#### Create the Environment File
The environment file contains the settings for each back end you want to define. It also contains other settings relevant to the deployment of a custom back end. Having the environment file handy will help ensure that the back end settings persist through future Overcloud updates.  

Create the environment file that will orchestrate the back end settings. Use the sample file provided below for each backend.  
* [Dell EMC XtremIO Block Storage driver](link)
* [Dell EMC PowerMax iSCSI and FC drivers](link)
* [Dell EMC SC Series Fibre Channel driver](link)

#### Deploy the configured backends
When you have created the dellemc-<backend>-env.yaml file, deploy the backend configuration by running the openstack overcloud deploy command using the templates option. If you passed any extra environment files when you created the overcloud, pass them again here using the -e option. 
 
```bash
$ openstack overcloud deploy --templates -e /home/stack/templates/dellemc-<backend>-env.yaml 
```

When the director completes the overcloud deployment, check the cinder.conf on the controller nodes and verify the settings mapped.

#### Testing the configured Backend
After you deploy the back ends to the overcloud, test if you can successfully create volumes on them.


## References
* [Red Hat OpenStack Platform Overcloud Custom Block Storage Backend Guide](https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/13/html/custom_block_storage_back_end_deployment_guide/index)

  






