KUBERNETES
Although we can manually maintain a couple of containers or write scripts to manage the lifecycle of dozens of containers, orchestrators make things much easier for users especially when it comes to managing hundreds or thousands of containers running on a global infrastructure.
Most container orchestrators can:
    ▪ Group hosts together while creating a cluster, in order to leverage the benefits of dictributed systems.
    ▪ Schedule containers to run on hosts in the cluster based on resources availability.
    ▪ Enable containers in a cluster to communicate with each other regardless of the host they are deployed to in the cluster.
    ▪ Bind containers and storage resources.
    ▪ Group sets of similar containers and bind them to load-balancing constructs to simplify access to containerized applications by creating an interface, a level of abstraction between the containers and the client.
    ▪ Manage and optimize resource usage.
    ▪ Allow for implementation of policies to secure access to applications running inside containers.
With all these configurable yet flexible features, container orchestrators are an obvious choice when it comes to managing containerized applications at scale. In this course, we will explore Kubernetes, one of the most in-demand container orchestration tools available today.