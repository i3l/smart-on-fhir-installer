## Get started with one line, using Vagrant

The three prerequisites, which are available on Mac, Windows, and Linux 
are (we have tested with the versions below, but other versions may be fine too):

1. [VirtualBox 4.3.12](https://www.virtualbox.org/wiki/Downloads)
2. [Vagrant 1.6.2](http://www.vagrantup.com/downloads)
3. [Ansible 1.6.1](http://docs.ansible.com/intro_installation.html)

Make sure that you have enabled the VT-X hardware virtualization feature on your machine, otherwise Vagrant won't start the VM.
Once you have Virtualbox and Vagrant installed on your machine, you can:

```
vagrant plugin install vagrant-vbguest
git clone https://github.com/smart-on-fhir/smart-on-fhir-installer/
cd smart-on-fhir-installer
vagrant up
```

... wait ~20min while everything installs (depending on your Internet connection speed).

Now visit in a web browser on your local ("host") machine:

 * `http://localhost:9080`  for a FHIR API server
 * `http://localhost:9085`  for an OAuth2 authorization server
 * `http://localhost:9090`  for a SMART apps server

The authorization server uses the OpenLDAP server running on the virtual machine. 
The two sample accounts are `demo/demo` and `admin/password` by default. You should change
these for production environments. You can connect to the LDAP server on `localhost:1389`.

You can poke around the virtual machine by doing:

```
vagrant ssh
```

And when you're done you can shut the virtual machine down with:

```
vagrant halt
```

---

## Building SMART-on-FHIR on fresh Ubuntu 14.04 machine (without Vagrant)

```
apt-get update
apt-get install curl git python-pycurl python-pip python-yaml python-paramiko python-jinja2
pip install ansible==1.6
git clone https://github.com/smart-on-fhir/smart-on-fhir-installer
cd smart-on-fhir-installer/provisioning
```

At this point, you probably want to edit `custom_settings.yml` or pass a
vars file with settings that suit your needs.  For example, change `localhost`
to some world-routable hostname if that's what you need -- and set the
app_server public port to 80.

```
ansible-playbook  -c local -i 'localhost,' -vvvv smart-on-fhir-servers.yml 
```

---

## Notes

While, by default, the install process will not enable SSL, 
the script can be configured to generate self-signed SSL certificates for the servers
and enable secure HTTP. You can inject your own sertificates in the build process too. Should you choose
to try the self-signed certificates, please be aware that you will get a number of trust warning in your
web browser when you try to run the apps server. These can be resolved on a client-by-client basis
by adding certificate exceptions in your browser. Before you even try the apps, you should probably load the
API server and add the self-signed certificate to your browser's security exceptions.
