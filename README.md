# Jenkins-ansible-aci
This is an ongoing project to provide another way to interact with Cisco's ACI. 

The idea is to provide a pre-configured jenkins server to which API calls can be made to push policies to ACI.

This project is based off the [jenkins/jenkins image on dockerhub](https://hub.docker.com/r/jenkins/jenkins/).

These build jobs and playbooks were tested on the [Cisco ACI sandbox](https://sandboxapicdc.cisco.com).

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

```
Docker
```

### Installing

1. Ensure docker is running*
```
docker --version
```
*Note: if running docker on Windows; "Linux containers" will have to be enabled

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

## API endpoints
Below is a list of the API endpoints that represent Jenkins build jobs that have been configured so far.

Each endpoint will have different parameters to provide with the API call depending on the policy being pushed.

The common parameters that can/should be included with every API call are:
* description (optional) - Policy description
* token - All Jenkins build jobs have been configured with "aci_helper" as the token
* username (required) - Username of ACI account 
* password (required) - Password of ACI account
* state (required) - "present" for creation, "absent" for removal
* apic (required) - hostname or IP of APIC
* output_level (optional) - debug, info or normal

### Tenant
Query params:
* tenant - Name of the tenant
```
GET
http://localhost:<port number>:8080/job/schedule_tenant/buildWithParameters?token=aci_helper&tenant=odysseus&description=test&username=<username>&password=<password>&state=absent&apic=<apic IP/hostname>
```
### VRF
Query params:
* vrf - Name of the VRF to be configured
* tenant - Name of the tenant that the VRF will be configured under
* policy_control_direction - Ingress or egress
* policy_control_preference - enforced unenforced
```
GET
http://localhost:<port number>:8080/job/vrf/buildWithParameters?token=aci_helper&tenant=DC1&description=test&username=<username>&password=<password>&state=absent&apic=<apic IP/hostname>&policy_control_direction=ingress&vrf=jenkinsjenkins&output_level=info&policy_control_preference=enforced
```
### Application Profile
Query params:
* ap - Name of the application profile to be configured
* tenant - Name of the tenant in which the application profile will be created
```
GET
http://localhost:<port number>:8080/job/application_profile/buildWithParameters?token=aci_helper&description=test&username=<username>&password=<password>&state=absent&apic=sandboxapicdc.cisco.com&ap=jenkins-testeroo&tenant=Heroes&host=sandboxapicdc.cisco.com
```

## Built With

* [Jenkins](https://jenkins.io/doc/)
* [Ansible](https://docs.ansible.com/ansible/latest/index.html) 
* [Docker](https://docs.docker.com/)

## Acknowledgments

* To the authors of all the out-of-the-box ansible aci modules
  * Jacob McGill (@jmcgill298) - [ACI tenant](https://docs.ansible.com/ansible/latest/modules/aci_tenant_module.html#aci-tenant-module), [ACI VRF](https://docs.ansible.com/ansible/latest/modules/aci_vrf_module.html#aci-vrf-module)
  * Swetha Chunduri (@schunduri) - [ACI application profile](https://docs.ansible.com/ansible/latest/modules/aci_ap_module.html#aci-ap-module)
* To the authors of https://hub.docker.com/r/jenkins/jenkins/
* To everyone else involved with writing the tools that this project is using

