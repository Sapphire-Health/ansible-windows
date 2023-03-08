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

### Get a kerberos ticket
```
kinit username@DOMAIN.TLD
```

### Run a playbook
```
ansible-playbook -i hosts.yml -e @vars.yml updates/search.yml
```