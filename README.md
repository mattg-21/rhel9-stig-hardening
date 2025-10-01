# Rhel9-stig-hardening (manual and automated with ansible)

## Overview
This project demonstrates how to apply DISA STIGs to RHEL9 systems using OpenSCAP and STIG Viewer. The goal is to demonstrate compliance scanning, remediation, and validation.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Technology Used
- Red Hat Enterprise Linux 9 (1 control  node, and 2 test nodes)
- Ansible
- Stig Viewer
- OpenSCAP and SCAP Security Guide
- Jinja
- DISA STIG v2r4 (4/29/2025)
- Ansible Vault


## Phase 1: Manual STIG Application and Validation

First I validated compliance manually by running:

**sudo /usr/lib/systemd/systemd-sysctl -- cat-config | grep net.ipv4**

This command returned the following non-compliant values on my host system (adminwks):
- net.ipv4.conf.default.rp_filter = 2 (should be "1" according to tenable)
- net.ipv4.conf .*. rp_filter = 2 (should be "1" according to tenable)
- net.ipv4.conf.default.accept_source_route = 1 (should be "0" according to tenable)

### Manual Remediation
To correct the values I went to tenable and looked up the STIG ID for the three entries. Then I created the following file with the correct changes:

**sudo vim /etc/sysctl.d/stig-ipv4.conf**

Inside the file:
### RHEL-09-253050: Enable reverse path filtering on IPv4 interfaces by default
net.ipv4.conf.default.rp_filter = 1
### RHEL-09-253035: Enable reverse path filtering on all IPv4 interfaces
net.ipv4.conf.all.rp_filter = 1
### RHEL-09-253045: Configure RHEL 9 to not forward IPv4 source-routed packets by default
net.ipv4.conf.default.accept_source_route = 0

Applying the settings:

**sudo sysctl --system**

Please see the following screenshots here: To see the results of the commands. The STIGs were succesfully applied.
