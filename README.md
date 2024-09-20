## NOTE: Hitachi Vantara released it's official collection here:
##       https://github.com/hitachi-vantara/vspone-block-ansible

## !!!   This repsitory is for a very specific usecase only.  !!!

## Ansible Collection - Hitachi.Raidcom
Ansible modules to manage Hitachi VSP storage based on hiraid Python library that uses Hitachi CCI/Raidcom (CLI)

## Purpose
Public community supported collection of utils/modules/playbooks to manage Hitachi VSP Storage

## Prerequisites
- Python >= 3.6.8
- Ansible >= 2.11.0
- CCI >= 1-66-03/01
- hiraid (python library) >= 1.0.16
- Hitachi storage supported by CCI
For storages that do not embed CCI on controller or if replication management is needed: 
- Hitachi Command Control Interface Enterprise (CCI) software from support.hitachivantara.com (requires customer login)
- CCI configured (HORCM files, horcmstart and login)

## Documentation
Example playbooks can be found in docs/examples  
A detailed syntax description is found in each module

## Contribute
If you miss a feature please open an issue and mark it as ENHANCEMENT: https://github.com/hv-ps/hitachi.raidcom.ansible/issues

## Support
This is a community supported collection.  
Please report issues marked as BUG here: https://github.com/hv-ps/hitachi.raidcom.ansible/issues

## Authors
For feedback reach the Hitachi Vantara development team by email: raidcomansible@hitachivantara.com
