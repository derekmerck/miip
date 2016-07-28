# Medical Imaging Informatics Platform

Derek Merck <derek_merck@brown.edu>
Rhode Island Hospital
Summer 2016

Uses Ansible to configure and spin up a Docker-based open-source<sup><a name="^splunk_ref">[1](#^splunk)</a></sup> medical imaging informatics platform.  Originally developed to support the RIH Clinical Imaging Research Repository (CIRR).


## Services

- Clinical/PHI Facing Receiver - [Orthanc] on 4280 (HTTP/REST), 4242 (DICOM)
- Research/Anonymized Facing Repository - [XNAT] 1.6.5 on 5280 (HTTP/REST), 5242 (DICOM)
- Database - [Postgresql] 9.5 on 1432 (SQL)
- Log Monitoring - [Splunk] Lite on 1580 (HTTP/REST), 1514 (syslog)

[Splunk]:http://www.splunk.com
[Postgresql]:http://www.postgresql.org
[Orthanc]:http://www.orthanc-server.com
[XNAT]:http://www.xnat.org
[Tithonus]:https://github.com/derekmerck/Tithonus


## Dependencies

- [Docker] for service virtualization
- [Python] 2.7, [Ansible], [jinja2] for orchestration

[Docker]:http://www.docker.com
[docker-compose]:https://docs.docker.com/compose/
[Python]:http://www.python.org
[pyyaml]:http://pyyaml.org
[jinja2]:http://jinja.pocoo.org
[ansible]:http://www.ansible.com


## Usage

Setup all services on a single machine:

```bash
$ ansible-playbook -i hosts -v miip-allinone.yml
```


## Configuration

_Warning_: once data has been ingested, be _very_ careful about rebuilding the containers or changing configs, which can trigger rebuilds in the containers and drop data volumes or databases.  An additional "ALLOW_CLEAN" variable flag serves as a protection against this.<sup><a name="^database_ref">[2](#^database)</a></sup>

Ansible requires a `hosts` definition file, and any hosts that are to be configured as all-in-one targets should be included in the `miip` group, to get access to the default settings and port assignments.  Ansible also requires login information for setting up the various services.  A `credentials.example.yml` template is included.

If `./pkg/xnat-1.6.5.tar.gz` exists, it will use that to build XNAT from source instead of downloading the tarball from the NRG.


### Administration

By default, the Splunk container monitors the Orthanc and XNAT log files.  They are also exposed on the host in `\var\log\orthanc` and `\var\log\xnat` by default.


## Troubleshooting

If using [Vagrant] with [VirtualBox], the Maven build for XNAT requires a lot of memory, so you may need to bump the VM RAM up from the default to 2GB.

[vagrant]: http://www.vagrantup.com
[virtualbox]: https://www.virtualbox.org

## Acknowledgements

Uses Docker images from:

- [jodogne/orthanc](https://github.com/jodogne/OrthancDocker)
- [chaseglove/xnat](https://github.com/chaselgrove/xnat-docker)
- [outcoldman/splunk](https://github.com/outcoldman/docker-splunk)


## License

MIT

---

<a name="^splunk">1</a>: Splunk is not open source, but Splunk Lite will work for this volume of logs and it _is_ free.  Replace it with you open-source syslog server of choice if necessary.[:arrow_heading_up:](#^splunk_ref)

<a name="^database">2</a>: Orthanc will happily use an existing database, or create a new one if necessary.  However, XNAT _requires_ that the database and image store volume be initialized during build, so the database may be dropped if Ansible decides to rebuild the XNAT container.[:arrow_heading_up:](#^database_ref)

