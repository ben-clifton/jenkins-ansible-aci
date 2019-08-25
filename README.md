# Jenkins-ansible-aci
This is an ongoing project to provide another way to interact with Cisco's ACI. 
The idea is just to provide a pre-configured jenkins server to which API calls can be made to push policies to ACI.
This project is based off the [jenkins/jenkins image on dockerhub](https://hub.docker.com/r/jenkins/jenkins/).

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

```
Docker
```

### Installing

1. Ensure docker is running
```
docker --version
```

2. Clone this repo
```
git clone https://github.com/ben-clifton/jenkins-ansible-aci.git
```

3. Run the dockerfile to build the image that the container will be run on
```
git clone https://github.com/ben-clifton/jenkins-ansible-aci.git
```

4. List all docker images and copy the image id
```
docker image ls
```

5. Start a container based on the newly created image
```
docker run --rm -p <host machine port number>:8080 <image id>
```

6. Start sending API requests to the server.

### API endpoints 

Tenant
```
GET
http://localhost:<port number>:8080/job/schedule_tenant/buildWithParameters?token=aci_helper&tenant=odysseus&description=test&username=admin&password=ciscopsdt&state=absent&apic=sandboxapicdc.cisco.com
```
VRF
```
GET
http://localhost:<port number>:8080/job/vrf/buildWithParameters?token=aci_helper&tenant=DC1&description=test&username=admin&password=ciscopsdt&state=absent&apic=sandboxapicdc.cisco.com&policy_control_direction=ingress&vrf=jenkinsjenkins&output_level=info&policy_control_preference=enforced
```

## Built With

* [Jenkins](https://jenkins.io/doc/)
* [Ansible](https://docs.ansible.com/ansible/latest/index.html) 
* [Docker](https://docs.docker.com/)

## Acknowledgments

* To the authors of all the out-of-the-box ansible aci modules
  * Jacob McGill (@jmcgill298) - [ACI tenant](https://docs.ansible.com/ansible/latest/modules/aci_tenant_module.html#aci-tenant-module), [ACI VRF](https://docs.ansible.com/ansible/latest/modules/aci_vrf_module.html#aci-vrf-module)
* To the authors of https://hub.docker.com/r/jenkins/jenkins/
* To everyone else involved with writing the tools that this project is using

