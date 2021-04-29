# VMware AVI Vantage Platform

- [VMware AVI Vantage Platform](#vmware-avi-vantage-platform)
  - [Introduction](#introduction)
  - [Documentation](#documentation)
  - [Architecture](#architecture)

## Introduction

**Avi Vantage Platform** is an modern appication delivery solution developed by AVI Networks. It includes local and global load balancing, application visibility, security and monitoring. AVI Networks were acquired by VMware in July 2019 and integrated into NSX ecosystem as **VMware NSX Advanced Load Balancer**. 

## Documentation

- [Avi Networks REST API Usage](https://avinetworks.com/docs/latest/api-guide/)


## Architecture

The main difference from traditional application delivery architecture that AVI brings is the separation of control plane from data plane. 

The control plane is composed of **Avi Controller Cluster** which hosts the data analytics engine and provides an REST API, Web UI or CLI interface and for administrative interaction. It also provides integration to infrastructure providers (on-prem or cloud) and can scale out data plane components as required.

The data plane is composed of **Avi Service Engines** which handle user traffic forwarding based on rules configured by controller. The SEs also provide real-time application visibility and telemetry.