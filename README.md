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

- There are ports that need to remain open in order for the ASE to work.
- Internal DNS or Azure Private Zone

### ASE Networking

- Can set UDR to for example for all traffic through a Firewall

### ASE Recommended for

- Highly regulated environments (PCI, HIPAA, Government)
- For intensive loads where hardware and network isolation are required
