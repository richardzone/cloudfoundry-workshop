<!-- page_number: true -->
# Pivotal Cloud Foundry 101 Course

![](https://upload.wikimedia.org/wikipedia/en/thumb/b/bb/CloudFoundryCorp_vertical.svg/1280px-CloudFoundryCorp_vertical.svg.png)

---

# What you'll learn from this course

---

- What is [Cloud Foundry](https://www.cloudfoundry.org/)
- What are [Pivotal Cloud Foundry](https://pivotal.io/platform) and [Pivotal Web Services](http://run.pivotal.io/)
- How to develop your app as [Cloud-native application](12factor.net)
- How to deploy and maintain a cloud-native app in Cloud Foundry
- How to access external resources/services from your app
- How is Cloud Foundry designed

---

# Why would I learn CF?

---

- Reportedly, some of the giants in IT industry actively use it in massive-scale production load, e.g. [Baidu](https://www.wired.com/2013/07/cloudfoundry/) and IBM Bluemix.
- Our enterprise clients(Like ~~XXX~~, ~~XXX~~) use it.
- One of our industry spotlights is **Cloud**, and the recent heat shifted from **IaaS** to **PaaS** - and **CF is a major and mature player in PaaS solutions**
- Gain insights of modern PaaS architecture and design

---

# But I want to use Container-as-a-service!

I want to use [Docker](https://www.docker.com/)-native solutions!

I want to use [kubernetes](https://kubernetes.io/)!

I want to use [Docker Swarm](https://github.com/docker/swarm)!

I want to use [Rancher](http://rancher.com/)!

---

# You'd still benefit from this course, because:

- Cloud Foundry actually **supports deployment of Docker containers**
- If you use [Heroku](https://www.heroku.com/) or other PaaS/CaaS, it shares similarities with CF, e.g. buildpacks.
- You also learn how to design and implement apps that are **cloud-native**.
- You still gain insights of how a fully-mature & production-ready PaaS is architecturally designed.

---

# OK, before our introduction to CF,

## we push an app straight to the cloud!

---

# Workshop #1

---

# Introduction to CloudFoundry

---

## Why PaaS

![cloud platform](https://stack247.files.wordpress.com/2015/05/azure-on-premises-vs-iaas-vs-paas-vs-saas.png)

---

## Why Cloud Foundry

- Open source
- Stable & Robust
- Large user group/community
- Both old & modern

---
## Difference between CF, PCF and PWS

### CF: Cloud Foundary - open source project

### PCF: Pivotal Cloud Foundary = CF + proprietary add-ons

### PWS: Pivotal Web Service - a public PaaS powered by PCF
---

## How PCF differs from CF
![difference](https://docs.pivotal.io/pivotalcf/1-10/customizing/images/pcf-commercialization.png "PCF VS CF")

---

## What happened after `cf push`

![](https://docs.cloudfoundry.org/concepts/images/app_push_flow_diagram_diego.png)

---

## Cloud Foundry Architecture

---

![Cloud Foundry architecture](https://docs.pivotal.io/pivotalcf/1-7/concepts/images/diego/diego-flow.png)

---

# Manifest & Logging

---

# Workshop #2

---

# Cloud Foundry's HA architecture

---



## 4 levels of HA in CF

- Availability Zones
- Health Management for App Instances
- Process Monitoring
- Resurrection for VMs

Ref: https://docs.cloudfoundry.org/concepts/high-availability.html#health-management-apps

---

# Services

---

## 12-factor app: Store config in the environment.

http://12factor.net/config

---

## 12-factor app: Treat backing services as attached resources.

http://12factor.net/backing-services

---

# Workshop #4: services

---

## Managed Service & User provided service instance

---

## Service instance lifecycle

- Create
- bind
- unbind
- delete

