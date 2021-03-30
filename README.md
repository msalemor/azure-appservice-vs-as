# Azure App Service vs App Service Environment

## Common

- App Service Plan
  - This of this as the rack where the compute resources are stored including CPU, memory and IO
  - An App service plan can houst "unlimited" sites
  - When App Service scales, what scales is the App Service
  - Consider multiple service plans if you need to scale the Web apps independently.
- Portal management
- Deployment slots
- Cutom domains
- Auto scalabiltiy
- Monitoring and alerting

## App Service "limitations"

- Always gets a public IP
- The IP can change on restart, 
  - IP can be fixed if an IP SSL certificate is deployed
- Premium plans have netwrok integration
- Traffic to App Service can be filtered using access rules

## ASE unique features

- Hardware and Network isolation
- Deployed inside a vnet
- Gets a private IP

### ASE requirements


- Internal DNS or Azure Private Zone
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

### ASE Networking

- Can set UDR to for example for all traffic through a Firewall

### ASE Recommended for

- Highly regulated environments (PCI, HIPAA, Government)
- For intensive loads where hardware and network isolation are required
