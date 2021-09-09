# cp_cspm_rotate_external_id
CSPM/Dome9 and AWS - Rotate External Id on the IAM Role's Trust Policy

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
