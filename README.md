# VMware AVI Vantage Platform

- [VMware AVI Vantage Platform](#vmware-avi-vantage-platform)
  - [Introduction](#introduction)
  - [Documentation](#documentation)
  - [Architecture](#architecture)
  - [Administration](#administration)
    - [REST API](#rest-api)
      - [Authentication](#authentication)
      - [Request Examples](#request-examples)

## Introduction

**Avi Vantage Platform** is an modern appication delivery solution developed by AVI Networks. It includes local and global load balancing, application visibility, security and monitoring. AVI Networks were acquired by VMware in July 2019 and integrated into NSX ecosystem as **VMware NSX Advanced Load Balancer**. 

## Documentation

- [Avi Networks REST API Usage](https://avinetworks.com/docs/latest/api-guide/)


## Architecture

The main difference from traditional application delivery architecture that AVI brings is the separation of control plane from data plane. 

The control plane is composed of **Avi Controller Cluster** which hosts the data analytics engine and provides an REST API, Web UI or CLI interface and for administrative interaction. It also provides integration to infrastructure providers (on-prem or cloud) and can scale out data plane components as required.

The data plane is composed of **Avi Service Engines** which handle user traffic forwarding based on rules configured by controller. The SEs also provide real-time application visibility and telemetry.


## Administration

### REST API

#### Authentication

In order to interact with the controller through REST API you need to first use username and password to retrieve a `CSRFTOKEN` from cookies returned after successfull authentication. This token is then used in subsequent requests.

Start by renaming the `environment.example` file to `environment.sh` in `ingredients` folder and update the content to reflect your particular environment. Load the variables from this script using `source` shell built in.

```bash
# Load environment script
cd ingredients
. environment.sh
```

This will generate required `curl` configuration files for GET and POST operations that will be used in all subsequent calls.

```bash
# List the generated files
ls
avi.cookie  avi.get  avi.logout  avi.post  environment.sh

# Check content of sample GET file
cat avi.get
--silent
--insecure
--cookie avi.cookie
--header "X-CSRFToken:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
--header "X-Avi-Version:18.1.2"
--header "Accept-Encoding:application/json"
--request GET
```

When you are finish with requests, you can execute `avi.logout` script which will logout from the controller and cleans up all temporary files.

```bash
# Logout and remove temporary files
./avi_logout
Logout sucessfull
Configuration files removed

# Verify the files were removed
ls
environment.sh
```

#### Request Examples

Retrieve cluster version.

```bash
curl --noproxy '*' \
      --config avi.get \
      $AVI_ENDPOINT/api/cluster/version \
      | jq -r '.'
{
  "Date": "2021-04-15T07:08:29+00:00",
  "Product": "controller",
  "Version": "20.1.5",
  "Tag": "20.1.5-9148-20210415.070829",
  "ProductName": "Avi Cloud Controller",
  "min_version": 15.2,
  "build": 9148
}
```