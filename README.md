## Manage Windows OS with Ansible

### Set environment variables
N/A

### Create an inventory

```
cat << EOF > hosts.yml
all:
    hosts:
        ansibletest01.fqdn.tld:
vars:
    ansible_user: "username@FQDN.TLD"
    ansible_connection: winrm
    ansible_winrm_transport: kerberos
    ansible_winrm_server_cert_validation: ignore
EOF
```

### Create a variable file

```
cat << EOF > vars.yml
match_host: ansibletest01.fqdn.tld
category_names:
  - Security Updates
  - Critical Updates
  - Update Rollups
  - Updates
  - Drivers
  - Definition Updates
server_selection: windows_update
reboot: false
EOF
```

### Install require collection
```
ansible-galaxy collection install -r collections/requirements.yml
```

### Get a kerberos ticket
```
kinit username@DOMAIN.TLD
```

### Run a playbook
```
ansible-playbook -i hosts.yml -e @vars.yml updates/search.yml
```

### Request a Citrix Authentication Certificate
```
export AZURE_SUBSCRIPTION_ID=
export AZURE_CLIENT_ID=
export AZURE_SECRET=
export AZURE_TENANT=

ansible-playbook -i hosts.yml --limit=epic-dc1-ica03.lcmchealth.org -e username=lyas.spiehler -e computername=vm-lspiehler -e ca=EPIC-DC1-ICA03.lcmchealth.org\\DC1-ICA03-FAS-CA -e template=CCExamRoom -e resource_group=LCMC-Shared -e storage_account_name=lcmcsharedgeneraleast01 -e container=certs certificate/citrix-auth-cert-request.yml
```

### Build a custom Ansible Automation Platform EE
https://access.redhat.com/solutions/6984770
https://www.redhat.com/en/blog/unlocking-efficiency-harnessing-the-capabilities-of-ansible-builder-3.0
pip install ansible-builder
podman login registry.redhat.io
ansible-builder build --tag docker.io/lspiehler/azure-ee:latest
podman login docker.io
podman push docker.io/lspiehler/azure-ee:latest