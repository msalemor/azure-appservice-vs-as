# Azure App Service vs App Service Environment

## 1.0 - Common

- App Service Plan
  - This of this as the rack where the compute resources are stored including CPU, memory and IO
  - An App service plan can houst "unlimited" sites
  - When App Service scales, what scales is the App Service
  - Consider multiple service plans if you need to scale the Web apps independently.
- Can support Window, Linux code (.Net,Python,Java,Node) and container workloads
- Azure Portal management and role-based access control (management plane)
- Deployment slots
- Cutom domains
- Local and attached storage
- Auto scalabiltiy
- Monitoring and alerting
- Application Gateway in WAF mode is generally recommended to protect incoming requests against commong attacks
- DevOps integration


## 2.0 - App Service "limitations"

- Always gets a public IP
- The DNS name but the IP can change on 
  - IP can be fixed if an IP SSL certificate is deployed
- Premium plans have netwrok integration allowing access to resources on vnets
- Traffic to App Service can be restricted by using access restrictions, for example:
  - Filtering on the IP address from a corporate firewall
  - Filtering from Application Gateway subnet

## 3.0 - ASE unique features

- Hardware and Network isolation
- Deployed inside a vnet
- External and internal modes
  - In internal mode the app service gets a private IP

### 3.1 - DNS requirements

- Internal DNS or Azure Private Zone
  - To configure DNS in your own DNS server with your ILB ASE:
    - create a zone for <ASE name>.appserviceenvironment.net
    - create an A record in that zone that points * to the ILB IP address
    - create an A record in that zone that points @ to the ILB IP address
    - create a zone in <ASE name>.appserviceenvironment.net named scm
    - create an A record in the scm zone that points * to the ILB IP address
  - To configure DNS in Azure DNS Private zones:
    - create an Azure DNS private zone named <ASE name>.appserviceenvironment.net
    - create an A record in that zone that points * to the ILB IP address
    - create an A record in that zone that points @ to the ILB IP address
    - create an A record in that zone that points *.scm to the ILB IP address


### 3.2 - Port reqruiements

- The required entries in an NSG, for an ASE to function, are to allow traffic:
  - Inbound
    - TCP from the IP service tag AppServiceManagement on ports 454,455
    - TCP from the load balancer on port 16001
    - from the ASE subnet to the ASE subnet on all ports
  - Outbound
    - UDP to all IPs on port 53
    - UDP to all IPs on port 123
    - TCP to all IPs on ports 80, 443
    - TCP to the IP service tag Sql on ports 1433
    - TCP to all IPs on port 12000
    - to the ASE subnet on all ports

### 3.3 - Other networking capabilities

- Can set UDR to for example for all traffic through a Firewall

### 3.3 - Recommended for

- Highly regulated environments (PCI, HIPAA, Government)
- For intensive loads where hardware and network isolation are required

### 3.1 - Create an ASE

```bash
rgname=rg-ase-eus-demo
location=eastus
vnetname=domainvent
subnetname=aseSubnet
asename=domainase

az group create -g $rgname --location $location

az network vnet create -g $rgname -n $vnetname \
  --address-prefixes 10.0.0.0/16 --subnet-name $aseSubnet --subnet-prefixes 10.0.0.0/24

az appservice ase create -n $asename -g $rgname --vnet-name $vnetname \
  --subnet $subnetname
```

## 4.0 - Reference

- [Best practices](https://azure.github.io/AppService/2020/05/15/Robust-Apps-for-the-cloud.html)
