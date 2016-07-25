# Medical Inmaging Informatics Platform

Derek Merck, Summer 2016

Uses Ansible and Docker to setup a medical imaging research data archive.


## Usage

Setup all services on a single machine

```bash
$ ansible-playbook -i hosts -v miip-allinone.yml
```

## Troubleshooting

If using Vagrant+VirtualBox, the Maven build for XNAT requires a lot of RAM, and may need to bump the VM RAM up to 2GB.