# Medical Imaging Informatics Platform

Derek Merck, Summer 2016

Uses Ansible and Docker to setup a medical imaging research data archive.

## Usage

Setup all services on a single machine:

```bash
$ ansible-playbook -i hosts -v miip-allinone.yml
```

## Tips

Requires a `secrets.yml` file with credentials.

If `pkg/xnat-1.6.5.tar.gz` exists, it will use that instead of downloading from NRG.

## Troubleshooting

If using Vagrant and VirtualBox, the Maven build for XNAT requires a lot of memory, so you may need to bump the VM RAM up from the default to 2GB.