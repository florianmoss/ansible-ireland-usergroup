# Ansible Ireland Meetup 

[LinkedIn](https://www.linkedin.com/in/florianmoss/)

[E-Mail](mailto:fmoss@redhat.com)

The repo contains the source code for the talk "Ansible and Podman for IoT and Edge devices"

[Directory](ansible_code/)


## How to replicate this?

Make sure to have Vagrant installed locally.

```bash
# Clone the repository
git clone https://github.com/florianmoss/ansible-ireland-usergroup.git

# Get the vagrant hosts running
cd ansible-ireland-usergroup/server
vagrant up
```

To understand what is happening, inspect the /server/Vagrantfile file. Essentially, we are creating a minimal Fedora installation and are executing the /server/site.yml playbook against it.

The playbook does the following:
1. Create a service 'ansible-pull' with the contents from /server/template/ansible-pull.service.j2
2. Create a timer to run this service every 5 minutes

What does that mean exactly?

Essentially, the timer will reach out to the Git repo specified in /server/template/ansible-pull.service.j2 and pull down (every 5 min) the site.yml file and  execute it locally.

Think about this for a second - once we have this specified, our system will configure itself essentially on a pull basis rather than a push basis, as you would usually do it with Ansible.

Instead of me trying to push changes to the host, the host reaches now out to draw down any changes I make to the configuration file.

In my case, I want to run and manage a container. The /server/template/container-web.service.j2 will go out and pull the specified container image. 

## Use Cases 

The specific use case for this project was a research project that deployed hundreds of Rasberry PI's in a remote location. These machines have an internet connection but not continiously, sometimes the connection would drop or be quite bad. 

This also allowed us to deploy these PI's and not worry about managing them individually or manage inventories. 

Other use cases can be IoT deployments where you don't know the actual IP address/hostname...

All you need to do is ship the end device with the site.yml file and let the device configure itself.


