---
title: "Run Apache Kafka on Kubernetes in Minutes with Strimzi"
datePublished: Sun Jun 09 2024 23:28:58 GMT+0000 (Coordinated Universal Time)
cuid: clx86ebbf000009mggdz7hfk0
slug: run-apache-kafka-on-kubernetes-in-minutes-with-strimzi
canonical: https://sredevops.org/en/run-apache-kafka-on-kubernetes-in-minutes-with-strimzi/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1717975736926/ec80557d-1431-4342-a01d-4b12398ea61c.png
tags: cloud, apps, opensource, kubernetes, devops, english, sre

---

### Key Features of Strimzi

*   **Secure by Default**: Strimzi includes built-in security features like TLS, SCRAM-SHA, and OAuth authentication, as well as automated certificate management.
*   **Simple yet Configurable**: Strimzi offers flexible deployment options, including NodePort, Load Balancer, and Ingress, as well as features like rack awareness for high availability.
*   **Kubernetes Native Experience**: Strimzi uses a Kubernetes Operator to manage the entire Kafka lifecycle, allowing you to use familiar kubectl commands to deploy and manage your Kafka clusters.

What is Strimzi?
----------------

![Run Apache Kafka on Kubernetes in Minutes with Strimzi](https://cdn.hashnode.com/res/hashnode/imageupload/v1717975735114/875c9131-4896-4389-a192-ab1842bad2b9.png)

Strimzi is an open-source project that provides a way to run an Apache Kafka cluster on Kubernetes in various deployment configurations. It abstracts away the underlying infrastructure, allowing developers and operations teams to focus on the applications themselves rather than worrying about the intricacies of the hosting environment.

One of the key benefits of Strimzi is its ability to automate the deployment, scaling, and management of Kafka clusters. By using Kubernetes Custom Resources, Strimzi makes it easy to create, configure, and maintain Kafka clusters, topics, and users, all within your Kubernetes environment.

Strimzi also includes built-in security features, such as TLS encryption, SCRAM-SHA authentication, and OAuth support, ensuring that your Kafka cluster is secure by default. Additionally, Strimzi provides options for exposing your Kafka cluster outside of Kubernetes, including NodePort, Load Balancer, and Ingress, making it easy to integrate with other applications and services.

Whether you're running Kafka on a local Minikube cluster, a Kubernetes Kind environment, or a production-grade Kubernetes deployment, Strimzi makes it simple to get started and manage your Kafka infrastructure.

Getting Help
------------

If you encounter any issues while using Strimzi, you can get help in the following ways:

*   Send an email to the [Strimzi Mailing list](https://lists.cncf.io/g/cncf-strimzi-users/topics?ref=sredevops.org)
*   Ask your question on the [Strimzi Slack Workspace](https://slack.cncf.io/?ref=sredevops.org)

Quickstarts
-----------

Minikube
--------

Minikube provides a local Kubernetes, designed to make it easy to learn and develop for Kubernetes. The Kubernetes cluster is started either inside a virtual machine, a container or on bare-metal, depending on the minikube driver you choose.

Installing the dependencies
---------------------------

This quickstart assumes that you have the latest version of the `minikube` binary, which you can get from the [minikube website](https://minikube.sigs.k8s.io/docs/start/?ref=sredevops.org).

Minikube requires a container or virtual machine manager. The Minikube documentation includes a list of suggested options in the [getting started guide](https://minikube.sigs.k8s.io/docs/start/?ref=sredevops.org).

You'll also need the `kubectl` binary, which you can get by following the [`kubectl` installation instructions](https://kubernetes.io/docs/tasks/tools/?ref=sredevops.org) from the Kubernetes website.

Once you have all the binaries installed, make sure everything works:

    # Validate minikube
    minikube version
    
    # Validate kubectl
    kubectl version
    

Starting the Kubernetes cluster
-------------------------------

Start a local development cluster of [Minikube](https://minikube.sigs.k8s.io/docs/start/?ref=sredevops.org) that runs in a container or virtual machine manager.

    minikube start --memory=4096 # 2GB default memory isn't always enough
    

Deploy Strimzi using installation files
---------------------------------------

Before deploying the Strimzi cluster operator, create a namespace called `kafka`:

    kubectl create namespace kafka
    

Apply the Strimzi install files, including `ClusterRoles`, `ClusterRoleBindings` and some **Custom Resource Definitions** (`CRDs`). The CRDs define the schemas used for the custom resources (CRs, such as `Kafka`, `KafkaTopic` and so on) you will be using to manage Kafka clusters, topics and users.

    kubectl create -f 'https://strimzi.io/install/latest?namespace=kafka' -n kafka
    

The YAML files for `ClusterRoles` and `ClusterRoleBindings` downloaded from strimzi.io contain a default namespace of `myproject`. The query parameter `namespace=kafka` updates these files to use `kafka` instead. By specifying `-n kafka` when running `kubectl create`, the definitions and configurations without a namespace reference are also installed in the `kafka` namespace. If there is a mismatch between namespaces, then the Strimzi cluster operator will not have the necessary permissions to perform its operations.

Follow the deployment of the Strimzi cluster operator:

    kubectl get pod -n kafka --watch
    

You can also follow the operator's log:

    kubectl logs deployment/strimzi-cluster-operator -n kafka -f
    

Once the operator is running it will watch for new custom resources and create the Kafka cluster, topics or users that correspond to those custom resources.

Create an Apache Kafka cluster
------------------------------

Create a new Kafka custom resource to get a single node Apache Kafka cluster:

    # Apply the `Kafka` Cluster CR file
    kubectl apply -f https://strimzi.io/examples/latest/kafka/kraft/kafka-single-node.yaml -n kafka 
    

Wait while Kubernetes starts the required pods, services, and so on:

    kubectl wait kafka/my-cluster --for=condition=Ready --timeout=300s -n kafka 
    

The above command might timeout if you're downloading images over a slow connection. If that happens you can always run it again.

Send and receive messages
-------------------------

With the cluster running, run a simple producer to send messages to a Kafka topic (the topic is automatically created):

    kubectl -n kafka run kafka-producer -ti --image=quay.io/strimzi/kafka:0.41.0-kafka-3.7.0 --rm=true --restart=Never -- bin/kafka-console-producer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic
    

Once everything is set up correctly, you'll see a prompt where you can type in your messages:

    If you don't see a command prompt, try pressing enter.
    
    >Hello Strimzi!
    

And to receive them in a different terminal, run:

    kubectl -n kafka run kafka-consumer -ti --image=quay.io/strimzi/kafka:0.41.0-kafka-3.7.0 --rm=true --restart=Never -- bin/kafka-console-consumer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic --from-beginning
    

If everything works as expected, you'll be able to see the message you produced in the previous step:

    If you don't see a command prompt, try pressing enter.
    
    >Hello Strimzi!
    

Enjoy your Apache Kafka cluster, running on Minikube!

Deleting your Apache Kafka cluster
----------------------------------

When you are finished with your Apache Kafka cluster, you can delete it by running:

    kubectl -n kafka delete $(kubectl get strimzi -o name -n kafka)
    

This will remove all Strimzi custom resources, including the Apache Kafka cluster and any KafkaTopic custom resources but leave the Strimzi cluster operator running so that it can respond to new Kafka custom resources.

Next, delete the Persistent Volume Claim (PVC) that was used by the cluster:

    kubectl delete pvc -l strimzi.io/name=my-cluster-kafka -n kafka
    

Without deleting the PVC, the next Kafka cluster you might start will fail as it will try to use the volume that belonged to the previous Apache Kafka cluster.

Deleting the Strimzi cluster operator
-------------------------------------

When you want to fully remove the Strimzi cluster operator and associated definitions, you can run:

    kubectl -n kafka delete -f 'https://strimzi.io/install/latest?namespace=kafka'
    

Deleting the `kafka` namespace
------------------------------

Once it is not used, you can also delete the Kubernetes namespace:

    kubectl delete namespace kafka
    

Where next?
-----------

*   For an overview of the Strimzi components check out the [overview guide](https://strimzi.io/docs/operators/latest/overview?ref=sredevops.org).
*   For alternative examples of the custom resource that defines the Kafka cluster have a look at these [examples](https://github.com/strimzi/strimzi-kafka-operator/tree/0.41.0/examples/kafka?ref=sredevops.org)

Kubernetes Kind
---------------

Kubernetes Kind is a Kubernetes cluster implemented as a single Docker image that runs as a container. It was primarily designed for testing Kubernetes itself, but may be used for local development or CI.

Docker Desktop
--------------

Docker Desktop includes a standalone Kubernetes server and client, designed for local testing of Kubernetes. You can start a Kubernetes cluster as a single-node cluster within a Docker container on your local system.