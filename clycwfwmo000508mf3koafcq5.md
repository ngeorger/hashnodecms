---
title: "Check Kubernetes with Popeye! Security, configs, problems, scores and more with an open source and lightweight CLI tool"
datePublished: Mon Jul 08 2024 11:28:49 GMT+0000 (Coordinated Universal Time)
cuid: clycwfwmo000508mf3koafcq5
slug: check-kubernetes-with-popeye-security-configs-problems-scores-and-more-with-an-open-source-and-lightweight-cli-tool
canonical: https://sredevops.org/en/check-kubernetes-with-popeye-security-configs-problems-scores-and-more-with-an-open-source-and-lightweight-cli-tool/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1720438128419/9f27172a-d80c-4154-8c9e-b625706a9596.png
tags: apps, opensource, security, kubernetes, devops, english, sre, devsecops, cve

---

TL/DR;
------

![Check Kubernetes with Popeye! Security, configs, problems, scores and more with an open source and lightweight CLI tool](https://cdn.hashnode.com/res/hashnode/imageupload/v1720438124563/dd4cede4-11b3-4ee2-a310-8b3b1bf74502.png)

Tired of manually combing through your Kubernetes cluster for issues? Popeye is like a health check-up for your cluster, finding potential problems with your configurations and resource usage. It's a command-line tool that scans your live cluster, not just static files, and points out things like misconfigurations, unused resources, and even potential resource over-allocations. It's read-only, so it won't touch your cluster, just give you a friendly (or maybe not-so-friendly, depending on your cluster's health) report. You can even get fancy with different output formats (JSON, HTML, you name it), send reports to S3, and integrate it with Prometheus and Grafana for ongoing monitoring.

![Check Kubernetes with Popeye! Security, configs, problems, scores and more with an open source and lightweight CLI tool](https://cdn.hashnode.com/res/hashnode/imageupload/v1720438125190/adc12f18-8c92-4a93-a82b-ac9c0484d1ae.png)

Why You Need a Kubernetes Linter Like Popeye
--------------------------------------------

Let's face it, Kubernetes is awesome for orchestrating your containerized applications. However, as your deployments grow, so does the complexity. Suddenly, you're drowning in a sea of YAML files, wondering if that `Service` in the `default` namespace is _actually_ talking to your `Pod`, or if that `PersistentVolumeClaim` from a deleted project is still hanging around like a bad smell.

That's where Popeye comes in, flexing its spinach-powered muscles to give your cluster a thorough checkup.

### Popeye to the Rescue!

Popeye dives into your _live_ cluster, inspecting your resources as they're actually running. This isn't just some dry-run static analysis tool. It's the real deal, looking for common problems that can trip you up:

*   **Misconfigurations:** Are your container port mappings correct? Do your `Pod` labels match your `Service` selectors?
*   **Resource Usage:** Popeye can even tap into your metrics server (if you're using one) and warn you about potential CPU or memory over-allocations _before_ your cluster throws in the towel.
*   **Stale Resources:** Remember that `Namespace` you thought you deleted months ago? Popeye will find it. Those unused `Secrets`? Yep, it'll flag those too.
*   **Security Best Practices:** Popeye can help you catch things like Pods running as root, missing resource limits, and other security gotchas.

![Check Kubernetes with Popeye! Security, configs, problems, scores and more with an open source and lightweight CLI tool](https://cdn.hashnode.com/res/hashnode/imageupload/v1720438125716/edc0bb37-b677-46a9-a21d-29bdf3cdc7e2.png)

**Installation**
----------------

 You've got options! _(Yay! fuN!)_ Download binaries, use `brew install`, or `go install` if you're a Go aficionado.

    brew install derailed/popeye/popeye
    

    go install github.com/derailed/popeye@latest
    

Getting Started with Popeye
---------------------------

**Interpreting the Report:** Popeye color-codes its findings to give you a clear picture of your cluster's health:

*   **✅ OK:** Everything looks good!
*   **🔊 Info:** Just some FYI messages.
*   **😱 Warn:** Potential issues that you might want to look into.
*   **💥 Error:** Action required! These are problems that need fixing.

**Level Up with Prometheus and Grafana:** Integrate Popeye with Prometheus to collect metrics and visualize your cluster's health over time in Grafana. You can even set up alerts so you're notified when Popeye finds something fishy.

![Check Kubernetes with Popeye! Security, configs, problems, scores and more with an open source and lightweight CLI tool](https://cdn.hashnode.com/res/hashnode/imageupload/v1720438126417/e7a3985a-763c-4845-b56a-3928e31d09ae.png)

### **Customizing Scans (Spinach, Anyone?)**

You can fine-tune Popeye's behavior using a `spinach.yaml` configuration file. Want to adjust resource utilization thresholds, exclude specific resources, or even override the severity of certain checks? Spinach has got you covered!

    popeye:
      allocations:
        cpu:
          overPercUtilization: 70 # Trigger a warning if CPU utilization goes above 70%
    

**Run the Scan:** Popeye works right out of the box. Just point it at your cluster:

    popeye
    

Want to scan a specific namespace? No problem:

    popeye -n my-awesome-app 
    

### Popeye in Action: A Practical Example

Let's say you're running a web app in your cluster. You've got a `Deployment`, a `Service`, and a few other resources. You run Popeye, and it spits out the following:

    😱  WARN  po  Pods  default/my-awesome-app-7c94985768-x5fzk  Container 'my-awesome-app' has no resource requests or limits defined!
    

Uh oh! Looks like you forgot to set resource limits on your `Pod`. This means your app could potentially consume all the resources on your node, starving out other applications. Time to update that YAML file!

Keep Your Cluster Healthy with Popeye
-------------------------------------

Popeye is an essential tool for anyone running Kubernetes. It's like having a Kubernetes expert constantly looking over your shoulder, pointing out potential issues before they turn into major headaches. So, add Popeye to your toolbox and start giving your cluster the health checks it deserves!

Useful Links
------------

*   [Popeye GitHub Repository](https://github.com/derailed/popeye?ref=sredevops.org)

[

GitHub - derailed/popeye: 👀 A Kubernetes cluster resource sanitizer

👀 A Kubernetes cluster resource sanitizer. Contribute to derailed/popeye development by creating an account on GitHub.

![Check Kubernetes with Popeye! Security, configs, problems, scores and more with an open source and lightweight CLI tool](https://cdn.hashnode.com/res/hashnode/imageupload/v1720438126705/d3686f46-6dcc-40c5-907b-46e728582843.svg)GitHubderailed

![Check Kubernetes with Popeye! Security, configs, problems, scores and more with an open source and lightweight CLI tool](https://cdn.hashnode.com/res/hashnode/imageupload/v1720438126927/4d9b5580-5102-4793-acfc-8339a3b31ef8.png)

](https://github.com/derailed/popeye?ref=sredevops.org)

*   [Official website](https://popeyecli.io/?ref=sredevops.org)

[

popeye

👀 A Kubernetes cluster resource sanitizer

popeye

![Check Kubernetes with Popeye! Security, configs, problems, scores and more with an open source and lightweight CLI tool](https://cdn.hashnode.com/res/hashnode/imageupload/v1720438127314/4a6ac5fc-1b15-4387-9d01-4b7e8a51d06f.png)

](https://popeyecli.io/?ref=sredevops.org)

### Some of the available linters

K8s Resource

Linters

Aliases

🛀

Node

no

Conditions ie not ready, out of mem/disk, network, pids, etc

Pod tolerations referencing node taints

CPU/MEM utilization metrics, trips if over limits (default 80% CPU/MEM)

🛀

Namespace

ns

Inactive

Dead namespaces

🛀

Pod

po

Pod status

Containers statuses

ServiceAccount presence

CPU/MEM on containers over a set CPU/MEM limit (default 80% CPU/MEM)

Container image with no tags

Container image using `latest` tag

Resources request/limits presence

Probes liveness/readiness presence

Named ports and their references

🛀

Service

svc

Endpoints presence

Matching pods labels

Named ports and their references

🛀

ServiceAccount

sa

Unused, detects potentially unused SAs

🛀

Secrets

sec

Unused, detects potentially unused secrets or associated keys

🛀

ConfigMap

cm

Unused, detects potentially unused cm or associated keys

🛀

Deployment

dp, deploy

Unused, pod template validation, resource utilization

🛀

StatefulSet

sts

Unused, pod template validation, resource utilization

🛀

DaemonSet

ds

Unused, pod template validation, resource utilization

🛀

PersistentVolume

pv

Unused, check volume bound or volume error

🛀

PersistentVolumeClaim

pvc

Unused, check bounded or volume mount error

🛀

HorizontalPodAutoscaler

hpa

Unused, Utilization, Max burst checks

🛀

PodDisruptionBudget

Unused, Check minAvailable configuration

pdb

🛀

ClusterRole

Unused

cr

🛀

ClusterRoleBinding

Unused

crb

🛀

Role

Unused

ro

🛀

RoleBinding

Unused

rb

🛀

Ingress

Valid

ing

🛀

NetworkPolicy

Valid, Stale, Guarded

np

🛀

PodSecurityPolicy

Valid

psp

🛀

Cronjob

Valid, Suspended, Runs

cj

🛀

Job

Pod checks

job

🛀

GatewayClass

Valid, Unused

gwc

🛀

Gateway

Valid, Unused

gw

🛀

HTTPRoute

Valid, Unused

gwr