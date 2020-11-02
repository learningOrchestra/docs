# Installation

## Requirements

* Debian Linux OS hosts
* [Docker Engine](https://docs.docker.com/engine/install/) must be installed in all instances of your cluster
* Cluster configured in swarm mode, check [creating a swarm](https://docs.docker.com/engine/swarm/swarm-tutorial/create-swarm/)
* [Docker Compose](https://docs.docker.com/compose/install/) must be installed in the manager instance of your cluster

*Ensure that your cluster environment does not block any traffic such as firewall rules in your network or in your hosts.*

*If in case, you have firewalls or other traffic-blockers, add learningOrchestra as an exception.*

Ex: In Google Cloud Platform each of the VMs must allow both http and https traffic.

## Deployment

In the manager Docker swarm machine, clone the repo using:

```
git clone https://github.com/riibeirogabriel/learningOrchestra.git
```

Navigate into the `learningOrchestra` directory and run:

```
cd learningOrchestra
sudo ./run.sh
```

That's it! learningOrchestra has been deployed in your swarm cluster!

## Cluster State

`CLUSTER_IP:80` - To visualize cluster state (deployed microservices and cluster's machines).
`CLUSTER_IP:8080` - To visualize spark cluster state.

*\** `CLUSTER_IP` *is the external IP of a machine in your cluster.*
