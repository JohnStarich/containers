---

copyright:
  years: 2014, 2021
lastupdated: "2021-03-22"

keywords: kubernetes, iks, disaster recovery, dr, ha, hadr

subcollection: containers

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}
 


# Understanding high availability and disaster recovery for {{site.data.keyword.containerlong_notm}}
{: #ha}

Use the built-in Kubernetes and {{site.data.keyword.containerlong}} features to make your cluster more highly available and to protect your app from downtime when a component in your cluster fails.
{: shortdesc}

## About high availability
{: #ha-about}

High availability (HA) is a core discipline in an IT infrastructure to keep your apps up and running, even after a partial or full site failure. The main purpose of high availability is to eliminate potential points of failures in an IT infrastructure. For example, you can prepare for the failure of one system by adding redundancy and setting up failover mechanisms.
{: shortdesc}

**What level of availability do I need?**

You can achieve high availability on different levels in your IT infrastructure and within different components of your cluster. The level of availability that is right for you depends on several factors, such as your business requirements, the Service Level Agreements that you have with your customers, and the resources that you want to expend.

**What level of availability does {{site.data.keyword.cloud_notm}} offer?**

See [How {{site.data.keyword.cloud_notm}} ensures high availability and disaster recovery](/docs/overview?topic=overview-zero-downtime).

The level of availability that you set up for your cluster impacts your coverage under the [{{site.data.keyword.cloud_notm}} HA service level agreement terms](/docs/overview?topic=overview-slas). For example, to receive full HA coverage under the SLA terms, you must set up a multizone cluster with a total of at least 9 worker nodes, three worker nodes per zone that are evenly spread across three zones.

**What other cluster components can I set up in highly available architecture?**

See [Overview of potential points of failure in {{site.data.keyword.containerlong_notm}}](#fault_domains).

**Where is the service located?**

See [Locations](/docs/containers?topic=containers-regions-and-zones#regions-and-zones).

**What am I responsible to configure disaster recovery options for?**

See [Your responsibilities with using {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-responsibilities_iks).

## Overview of potential points of failure in {{site.data.keyword.containerlong_notm}}
{: #fault_domains}

The {{site.data.keyword.containerlong_notm}} architecture and infrastructure is designed to ensure reliability, low processing latency, and a maximum uptime of the service. However, failures can happen. Depending on the service that you host in {{site.data.keyword.cloud_notm}}, you might not be able to tolerate failures, even if failures last for only a few minutes.
{: shortdesc}

{{site.data.keyword.containerlong_notm}} provides several approaches to add more availability to your cluster by adding redundancy and anti-affinity. Review the following image to learn about potential points of failure and how to eliminate them.

<img src="images/cs_failure_ov.png" alt="Overview of fault domains in a high availability cluster within an {{site.data.keyword.cloud_notm}} region." width="250" style="width:250px; border-style: none"/>


### 1. Container or pod availability
{: #ha-container}

Containers and pods are, by design, short-lived and can fail unexpectedly. For example, a container or pod might crash if an error occurs in your app. To make your app highly available, you must ensure that you have enough instances of your app to handle the workload plus additional instances in the case of a failure. Ideally, these instances are distributed across multiple worker nodes to protect your app from a worker node failure.
{: shortdesc}

See [Deploying highly available apps](/docs/containers?topic=containers-plan_deploy#highly_available_apps).

### 2. Worker node availability
{: #ha-worker}

A worker node is a VM that runs on top of physical hardware. Worker node failures include hardware outages, such as power, cooling, or networking, and issues on the VM itself. You can account for a worker node failure by setting up multiple worker nodes in your cluster.
{: shortdesc}

See [Creating clusters with multiple worker nodes](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create).

Worker nodes in one zone are not guaranteed to be on separate physical compute hosts. For example, you might have a cluster with three worker nodes, but all three worker nodes were created on the same physical compute host in the IBM zone. If this physical compute host goes down, all your worker nodes are down. To protect against this failure, you must [set up a multizone cluster to spread worker nodes across three zones, or create multiple single zone clusters](/docs/containers?topic=containers-ha_clusters#ha_clusters) in different zones.
{: note}

### 3. Cluster availability
{: #ha-cluster}

<p>The [Kubernetes master](/docs/containers?topic=containers-service-arch) is the main component that keeps your cluster up and running. The master stores cluster resources and their configurations in the etcd database that serves as the single point of truth for your cluster. The Kubernetes API server is the main entry point for all cluster management requests from the worker nodes to the master, or when you want to interact with your cluster resources.<br><br>If a master failure occurs, your workloads continue to run on the worker nodes, but you cannot use `kubectl` commands to work with your cluster resources or view the cluster health until the Kubernetes API server in the master is back up. If a pod goes down during the master outage, the pod cannot be rescheduled until the worker node can reach the Kubernetes API server again.<br><br>During a master outage, you can still run `ibmcloud ks` commands against the {{site.data.keyword.containerlong_notm}} API to work with your infrastructure resources, such as worker nodes or VLANs. If you change the current cluster configuration by adding or removing worker nodes to the cluster, your changes do not happen until the master is back up.</p>
<p class="important">Do not restart or reboot a worker node during a master outage. This action removes the pods from your worker node. Because the Kubernetes API server is unavailable, the pods cannot be rescheduled onto other worker nodes in the cluster.</p>
{: shortdesc}

The cluster masters are highly available and include replicas for your Kubernetes API server, etcd, scheduler, and controller manager on separate hosts to protect against an outage such as during a master update.

To protect your cluster master from a zone failure, you can:
* Create a cluster in a [classic](/docs/containers?topic=containers-regions-and-zones#zones-mz) or [VPC](/docs/containers?topic=containers-regions-and-zones#zones-vpc) multizone location, which spreads the master across zones.
* Set up a second cluster in another zone.

See [Setting up highly available clusters](/docs/containers?topic=containers-ha_clusters#ha_clusters).

### 4. Zone availability
{: #ha-zone}

A zone failure affects all physical compute hosts and NFS storage. Failures include power, cooling, networking, or storage outages, and natural disasters, like flooding, earthquakes, and hurricanes. To protect against a zone failure, you must have clusters in two different zones that are load balanced by an external load balancer.
{: shortdesc}

See [Setting up highly available clusters](/docs/containers?topic=containers-ha_clusters#ha_clusters).

### 5. Region availability
{: #ha-region}

Every region is set up with a highly available load balancer that is accessible from the region-specific API endpoint. The load balancer routes incoming and outgoing requests to clusters in the regional zones. The likelihood of a full regional failure is low. However, to account for this failure, you can set up multiple clusters in different regions and connect them by using an external load balancer. If an entire region fails, the cluster in the other region can take over the work load.

See [Setting up highly available clusters](/docs/containers?topic=containers-ha_clusters#ha_clusters).

A multi-region cluster requires several Cloud resources, and depending on your app, can be complex and expensive. Check whether you need a multi-region setup or if you can accommodate a potential service disruption. If you want to set up a multi-region cluster, ensure that your app and the data can be hosted in another region, and that your app can handle global data replication.
{: note}

### 6. Storage availability
{: #ha-storage}

In a stateful app, data plays an important role to keep your app up and running. Make sure that your data is highly available so that you can recover from a potential failure. In {{site.data.keyword.containerlong_notm}}, you can choose from several options to persist your data. For example, you can provision NFS storage by using Kubernetes native persistent volumes, or store your data by using an {{site.data.keyword.cloud_notm}} database service.
{: shortdesc}

See [Planning highly available data](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).


