# How to for Grafana Dashboard on LetsDRNK

## IMPORTANT

URL of the dashboard: `https://dash.omniaz.io/`

Due to k8s metrics collector plugin ~~bug~~ specifics, only users with **Admin** permissions can view all dashboards properly. Thus even users meant to be **Viewers** will have **Admin** access if need to see all dashboards. 

In short - **Please do not make any changes if you are not administrating Grafana**.

## Signup

1) Receive invitation URL from an administrator
2) Follow the link and fill in fields with password/login

## Troubleshooting & inspecting

There is no use from monitoring if you don't know what you are looking for (and on what you look in the first place). So as a small guide for anyone - from mobile app dev to backend guys, here are tips on how to check if things can turn badly soon, and how to detect if they are already bad.

If something is not working, and you want to quickly check if it is infra issue ~~to beat the shit out of DevOps~~:

1) Check if on **K8s Cluster** dashboard if there are **Failed Nodes** (big green panel if there are none, and **red if there are failed**). If there are - call DevOps 
2) Check if on **K8s Cluster** or **K8s Deployments** dashboards is there are any **Pods Failed**. If there is *one*, it is possible that deployment of the new version of service is on the process, so no reasons for instant panic. If there are *more than one*  - call DevOps
        
        There are two possible reasons mainly - issue in code, that leads to continuous restarts of specific service, or infra issue, such as Nodes OOM, etc

If you want to dig into the issue deeper - there is plenty of data for troubleshooting:

* Check load on Nodes (CPU, RAM, Disk space, Disk IOPS)
* Check Network load on any layer - from Node to single Container
* Check resources consumption per each service/container/pod to determine any leaks

Checkout dashboards description and navigation tips below.

## Dashboards

There is an instrument panel on the left side of the Grafana interface. There you will find an icon with 4 squares, which is Dashboards menu. Move cursor on it to view its submenu. There click on **Manage** to get to the list of dashboards.

There are 4 dashboards that provide different info on GCP production and staging k8s clusters:

+ K8s Cluster - generally cover everything, and can be used for overall monitoring
+ K8s Node - detailed Nodes state (not fully operational at the moment)
+ K8s Deployments - detailed Deployments state
+ K8s Container - detailed Pods state and containers metrics

### Navigation

In each dashboard, there are filters (in the upper left corner) that allow switching between clusters (production/staging) and additional filters as well, like Nodes or Namespaces. 

On Graphs, there is a legend map (at the right) that can be used to check data per single point from legend. Just click on it to filter the graphs display. That is quite useful for heavily loaded graphs.

### K8s Cluster

Collects cluster-wide metrics, and overall info on basic k8s entities:

+ Cluster Health - percentage of cluster resources usage and total capacity (CPU, RAM, Disk)
+ Deployments - quantity, last updates, unavailable
+ Nodes - Total number, Unavailable, OOM nodes
+ Pods - total running, pending, failed, with unknown state
+ Containers - total running, waiting and terminated quantity, restarts quantity in the last 30 minutes interval, and overall containers resources usage
+ Jobs - the quantity of succeded and failed jobs

### K8s Node

At the moment not functioning properly, most of the Nodes data is not shown.

Still, once it is polished, should display detailed info on each Node (the host that contain k8s containers in k8s terminology):

+ List with Nodes, their health status, and summarized info like pods quantity (up/desired), CPU cores (requested/total) and RAM usage (used/available) in bytes
+ Graph with CPU load history in percents per each Node
+ Graph with RAM usage history, same as a graph with CPU 
+ Graph with HDD/SSD usage, same as for CPU and RAM
+ Graph with HDD/SSD throughput 

### K8s Deployments

Provide detailed info on Deployments:

+ Desired quantity of Replicas (bunches of Pods)
+ Quantity of Replicas that are up and running
+ Quantity of Replicas that are up to date (with latest deployed changes)
+ Quantity of Replicas that are failed or stoped
+ Graph with a quantity of generations (new versions deployments) per Replica(equal to service)

### K8s Container

Provide detailed info on containers resources consumption. Filters available to show data only for containers of specific Pod or filter containers by k8s tags.
Provided data:

+ Graph with RAM usage per container
+ Graph with CPU usage per container
+ Graph with Network download traffic per container
+ Graph with Network upload traffic per container
+ Graph with Disk IOPS on reading (per container)
+ Graph with Disk IOPS on writing (per container)
