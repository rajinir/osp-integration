
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
* [Dell EMC Unity driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-emc-unity-driver.html) - 
 For Unity please refer to  https://github.com/emc-openstack/osp-deploy
* [Dell EMC VNX driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-emc-vnx-driver.html) -  For VNX please refer toSee https://github.com/emc-openstack/osp-deploy



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

**1. Dell EMC XtremIO Block Storage driver**

**iSCSI Environment sample**

```yaml
parameter_defaults:
  CinderEnableIscsiBackend:true

parameter_defaults:
  ControllerExtraConfig:
    cinder::config::cinder_config:
        tripleo_dellemc_xtremio/volume_driver:
            value: cinder.volume.drivers.dell_emc.xtremio.XtremIOISCSIDriver
        tripleo_dellemc_xtremio/volume_backend_name:
            value: tripleo_dellemc_xtremio
        tripleo_dellemc_xtremio/san_ip:
            value: '10.10.10.10'
        tripleo_dellemc_xtremio/san_login:
            value: 'my_username'
        tripleo_dellemc_xtremio/san_password:
            value: 'my_password'
        tripleo_dellemc_xtremio/xtremio_volumes_per_glance_cache:
            value: 100  
    cinder_user_enabled_backends: ['tripleo_dellemc_xtremio']
```

**FC Environment sample**

```yaml
parameter_defaults:
  ControllerExtraConfig:
    cinder::config::cinder_config:
        tripleo_dellemc_xtremio/volume_driver:
            value: cinder.volume.drivers.dell_emc.xtremio.XtremIOFibreChannelDriver
        tripleo_dellemc_xtremio/volume_backend_name:
            value: tripleo_dellemc_xtremio
        tripleo_dellemc_xtremio/san_ip:
            value: '10.10.10.10'
        tripleo_dellemc_xtremio/san_login:
            value: 'my_username'
        tripleo_dellemc_xtremio/san_password:
            value: 'my_password'
        tripleo_dellemc_xtremio/xtremio_volumes_per_glance_cache:
            value: 100  
    cinder_user_enabled_backends: ['tripleo_dellemc_xtremio']
```

**2. Dell EMC PowerMax iSCSI and FC drivers**

**iSCSI Environment sample**

```yaml
parameter_defaults:
  CinderEnableIscsiBackend:true

parameter_defaults:
  ControllerExtraConfig:
    cinder::config::cinder_config:
        tripleo_dellemc_powermax/volume_driver:
            value: cinder.volume.drivers.dell_emc.vmax.iscsi.VMAXISCSIDriver
        tripleo_dellemc_powermax/volume_backend_name:
            value: tripleo_dellemc_powermax
        tripleo_dellemc_powermax/san_ip:
            value: '10.10.10.10'
        tripleo_dellemc_powermax/san_login:
            value: 'my_username'
        tripleo_dellemc_powermax/san_password:
            value: 'my_password'
        tripleo_dellemc_powermax/vmax_port_groups:
            value: '[OS-ISCSI-PG]'
        tripleo_dellemc_powermax/vmax_array:
            value: '000123456789'
        tripleo_dellemc_powermax/vmax_srp:
            value: 'SRP_1'
    cinder_user_enabled_backends: ['tripleo_dellemc_powermax']
```

**FC Environment sample**

```yaml
parameter_defaults:
  CinderEnableIscsiBackend:false

parameter_defaults:
  ControllerExtraConfig:
    cinder::config::cinder_config:
        tripleo_dellemc_powermax/volume_driver:
            value: cinder.volume.drivers.dell_emc.vmax.fc.VMAXFCIDriver
        tripleo_dellemc_powermax/volume_backend_name:
            value: tripleo_dellemc_powermax
        tripleo_dellemc_powermax/san_ip:
            value: '10.10.10.10'
        tripleo_dellemc_powermax/san_login:
            value: 'my_username'
        tripleo_dellemc_powermax/san_password:
            value: 'my_password'
        tripleo_dellemc_powermax/vmax_port_groups:
            value: '[OS-FC-PG]'
        tripleo_dellemc_powermax/vmax_array:
            value: '000123456789'
        tripleo_dellemc_powermax/vmax_srp:
            value: 'SRP_1'
    cinder_user_enabled_backends: ['tripleo_dellemc_powermax']
```
**3. Dell EMC SC Series Fibre Channel driver**

**FC Environment sample**
``` yaml
parameter_defaults:
  ControllerExtraConfig:
    cinder::config::cinder_config:
        tripleo_dellemc_dellsc/volume_driver:
            value: cinder.volume.drivers.dell_emc.sc.storagecenter_fc.SCFCDriver
        tripleo_dellemc_dellsc/volume_backend_name:
            value: tripleo_dellemc_dellsc
        tripleo_dellemc_dellsc/san_ip:
            value: '10.10.10.1'
        tripleo_dellemc_dellsc/san_login:
            value: 'Admin'
        tripleo_dellemc_dellsc/san_password:
            value: 'my_password'
        tripleo_dellemc_dellsc/dell_sc_ssn :
            value: '64702'
        tripleo_dellemc_dellsc/dell_sc_api_port:
            value: '3033'
        tripleo_dellemc_dellsc/dell_sc_server_folder:
            value: 'cindersrv'
        tripleo_dellemc_dellsc/dell_sc_volume_folder :
            value: 'cindervol'
    cinder_user_enabled_backends: ['tripleo_dellemc_dellsc']
```    
#### Deploy the configured backends

When you have created the dellemc-<backend>-env.yaml file, deploy the backend configuration by running the openstack overcloud deploy command using the templates option. If you passed any extra environment files when you created the overcloud, pass them again here using the -e option. 
 
```bash
(undercloud) $ openstack overcloud deploy --templates \
-e /home/stack/templates/dellemc-<backend>-env.yaml  \
-e <other templates>
```

#### Verify the configured changes

When the director completes the overcloud deployment, check the cinder.conf on the controller nodes and verify the settings mapped.

#### Testing the configured Backend
After you deploy the back ends to the overcloud, test if you can successfully create volumes on them.


## References
* [Red Hat OpenStack Platform Overcloud Custom Block Storage Backend Guide](https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/13/html/custom_block_storage_back_end_deployment_guide/index)

  






