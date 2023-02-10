# Factory of Hammers

Welcome to the factory of hammers


# Projects

- [docker-operator](https://github.com/6zacode-toolbox/docker-operator):  It a tool to manage docker deployments based on docker-compose simple descriptors to leverage k8s to orchestrate native docker artifacts outside of the boundraries of the k8s cluster that manages it. 
- [docker-agent](https://github.com/6zacode-toolbox/docker-agent): It is the tooling used by the [docker-operator](https://github.com/6zacode-toolbox/docker-operator) to interact with docket hosts. 


# Motivation for "docker-operator"

*Why to use docker these days? Does Kubernetes already does it for you?*

yes, kinda, but it is not for all scenarios. K8s is cool, but it is a bit too "corporate"/"complex"  for some family of problems. 

For example, one motivation is to manage cluster of [raspberry pis like in this post](https://medium.com/p/10e2a20d5695). I created several tools to help manage the PIs and ensure they [shared similar configs](https://github.com/6za/pi-gen) to make easier to manage them, but it lacked a "brain".  It worked well for most of scenarios I needed, but it was a bit annoying to mix "projects" abstractions and get the current state of the universe, event using tools for monitoring such as prometheus, grafana and other observations tools.  


With [K8S](https://medium.com/p/how-to-start-on-gitops-fast-435b8ff0a57a) in the game, it makes easier to leverage gitops as a way to have a state of the universe of this cluster in a gitops repo and this state is composed by a collective of raspberry pis and x86 machines that are simple docker hosts running state based workloads.  

On these machines I run more complex tooling and heavily dependent of state on disk, such as: 
- [Kafka](https://github.com/6za/kafka)
- [Hadoop](https://github.com/6za/hdfs-sample)
- Minio 

So, these kinda of tools that usually even in the cloud are not well suited to be run as "pods" on k8s clusters for high-performance scenarios, as k8s abstractions may confuse their ability of optimize its state sharing and data routing magic. Or some are just, "pre-cloud-native-hype". 

So bringing them to k8s is not impossible, just may require extra effort. 

I will share what made me to keep this tools out of Kubernetes for general research:

"On bright sunny day, I deployed Kafka on k8s as recommended on that time, as usually after any cool tool I install, I run some benchmark on that Kafka cluster. As expected when running benchmark, stuff can go wrong. fine.. 

As k8s has containers, I expected worst case scenario stuff get slow and I kill some pods and all go back to normal. 

That is not what happen, the pod bursting data to Kafka locked the network between itself and the Kafka cluster in some pods and different hosts. The issue that in the process it also, prevented k8s to talk between itself(in the end k8s is just a bunch of pods too)... and pods went bananas and cluster went to a unreachable state. The only solution was to hard kill the hosts VMs and install all over again, all state was lost and I had to start over from zero. I deployed Kafka on docker hosts directly, did the same benchmark and never got kicked out of my machine again, and a lesson was learned. Respect tools that care about state and like to lve on the limit of the host resources."


Some other references on this discussion k8s vs docker host: 
- https://geekflare.com/docker-vs-kubernetes/
- https://www.appvia.io/blog/5-reasons-you-should-not-use-kubernetes/
- https://convit.de/blog-cassandra-on-kubernetes-is-it-a-good-fit-convit
- https://foojay.io/today/kubernetes-and-apache-cassandra-what-works-and-what-doesnt/
- https://johanngyger.medium.com/kafka-on-kubernetes-a-good-fit-95251da55837
- https://redpanda.com/blog/kafka-kubernetes-deployment-pros-cons


The point is not that these tools compete, but to use the [proper hammer to the given situation](https://www.electronicshub.org/types-of-hammers/). Both should work, for most scenarios, but some allow you to get that extra bit without so much effort and tunning.

![](https://media.giphy.com/media/woblmoiaEOk6c/giphy.gif)



Welcome to the journey, 

@6za
