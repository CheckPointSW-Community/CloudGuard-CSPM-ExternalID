# CloudGuard CSPM External ID Rotation
Oftentimes, customers require having the AWS external ID utilized by the CP CSPM SaaS rotated as stipulted by various compliance  frameworks.  This repo contains an ansible tool to assit custoemers with this need. 

## Purpose
Create random External Id on the AWS IAM Role's Trust Policy, update and re-validate CloudGuard CSPM account

## Content
`ansible.cfg` - Ansible project level configuration file

`aws_trust_policy.j2` - AWS IAM Role Trust Policy Jinja2 template

`inventory.ini` - Ansible inventory INI file

`update_role.yml` - Ansible Playbook

`d9_payload.j2` - CSPM/Dome9 CloudAccountCredentialsViewModel HTTP body payload Jinja2 template
```
https://api-v2-docs.dome9.com/?python#cloudaccounts_updatecloudaccountcredentials
```

## Instructions
### AWS Environment Variables
Set AWS environmental variables for your shell session to work with the `iam_*` Ansible modules.
```
 export AWS_ACCESS_KEY='<API ACCESS KEY>'
 export AWS_SECRET_KEY='<API SECRET KEY>'
```

### Clone Repo
```
git clone https://github.com/Senas23/cp_cspm_rotate_external_id.git
```

### Install AWS Ansible modules requirements
Either with pip or compiled binaries from your Linux distro
```
boto
boto3
botocore
python >= 2.6
```
E.g. on Ubuntu 18
```
apt install python-boto python-boto3 python3-boto python3-boto3
```
See Ansible Docs for the module `iam_role`
```
https://docs.ansible.com/ansible/2.9/modules/iam_role_module.html
```

### Create Ansible Vault
 Create Ansible Vault file to store Check Point CloudGuard CSPM/Dome9 API credentials
```
ansible-vault create vault.yml
```
Two variables are needed:

`d9_api_key` - CSPM/Dome9 API Key

`d9_api_secret` - CSPM/Dome9 API Secret

Go to CSPM Settings and create v2 API Key
```
https://secure.dome9.com/v2/settings/credentials
```

## Run
Run the Ansible playbook and provide Vault encryption password
```
ansible-playbook update_role.yml -e '@vault.yml' --ask-vault-pass
```

## Development Environment
Ansible 2.9.24; Python 2.7.17 & 3.6.9; Ubuntu 18.04.5 LTS;
