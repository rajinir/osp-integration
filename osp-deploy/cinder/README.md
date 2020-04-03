
# Dell EMC Cinder Backend Deployment Guide for Red Hat OpenStack Platform

## Overview

This document describes how to deploy the Dell EMC Block Storage services in a Red Hat OpenStack Platform Overcloud.
This document assumes the RHOSP is installed and managed by RHOSP Director toolset which is based primarily on the on the upstream TripleO project.  

This document mainly covers the Dell EMC storage backends that are not yet fully integrated with Director through Tripleo like
* [Dell EMC XtremIO Block Storage driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-emc-xtremio-driver.html)
* [Dell EMC PowerMax iSCSI and FC drivers](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-emc-powermax-driver.html)
* [Dell EMC SC Series Fibre Channel driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-storagecenter-driver.html)
* [Dell EMC VxFlex OS (ScaleIO) Storage driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-emc-vxflex-driver.html)

The following Dell EMC storage drivers that are fully integrated with director and can be deployed using tripleo heat templates include
* [Dell EMC SC Series iSCSI driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-storagecenter-driver.html)
* [Dell EMC Unity driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-emc-unity-driver.html)
* [Dell EMC VNX driver](https://docs.openstack.org/cinder/latest/configuration/block-storage/drivers/dell-emc-vnx-driver.html)


### Prerequisites
- Red Hat OpenStack Platform (RHOSP) overcloud deployed through director.
- Dell EMC Storage Backend configured as storage repositories.
- Mapping of the configuration you want for the specific storage backend in /etc/cinder/cinder.conf

### Description

### Deploymentment Steps



