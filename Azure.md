# Azure Notes

## Cloud concepts 

- High Availability
  - Scale your application with Load balancers. supports both inbound and outbound, low latency 
  
- Scalability
  - Scalability applies to
    - Disk I/O
	- CPU
	- Memory
	- Network I/O
 - Horizontal Scaling
   - Complex
   - Add more services and spread the load 
   - must sync the applications and data
   - Long term
 - Vertical Scaling 
   - Easier 
   - Adding more resources (like memory, disk and cpu)
   - Short term
 - VM Scale Sets in Azure
   - Create and Manage groups of identical, load balanced VM's 
   - Allows to centrally manage, configure and update large number of VM's 

- Elasticity 
  - Ability to scale up and down
  - Handle high volume 
  - Use resources only when needed.
  - Avoids over provisioning and under provisioning 
  - Auto-scaling in Azure 
    - Add or remove VM's based on set of rules
	

- Agility
  - Value = Agility + Utilization 
  - Ability to provision and de-provision nearly unlimited resources as needed with complete control
  - Projects can be provisioned and deployed within 24 hours 
  
- Fault Tolerance 
  - Ability to continue providing services even after the failure of one or more cloud components  
  - Availability Sets in Azure 
  
- Disaster Recovery 
  - Types 
    - Completely Unavailable 
    - Partially available 
	- Fully available
  - Azure site recovery service 
    - Replicates workloads to a secondary location
	- it outage in primary site, fails over to secondary. once the outage is over fall back to primary. 
	
- CapEX vs OpEx
  - Capital Expenditures 
    - Money company spends on fixed assets
	- knows as PP&E - Property, Plant and Equipment 
	- One time purchases 
	- Servers, network devices, printers, scanners
	- Depreciation 
  - Operating Expenditures
    - Funds used to run day to day business 	
	- generally used up with in the year they purchased
	- printer cartridges, paper, electricity 
	- Web site hosting, web domain registrations, maintenance agreement, yearly service 
	- Pay as you go items 
	
- IAAS 
  - Provides infrastructure required on demand 
  - This service offering helps you avoid the expense and complexity of buying and managing your own physical servers and other data center infrastructure.
  - IAAS scenarios 
    - Test and Development 
    - Website hosting 
	- Storage, backup and recovery 
	- Web applications
	- High performance computing (eg : weather predictions, financial modeling)
	- Big data analysis 
  - Advantages 
	- Eliminates CapEx and reduces ongoing cost 
	- improves business continuity and DR 
	- Innovate rapidly 
	- Focus on core business
	- increase stability 
	- No need to troubleshoot equipment problems. cloud provides takes the responsibility of infra resources 
	- Get new applications to user faster 
  - When to use IAAS 
    - Startup or small company 
	- Large organization looking to only purchase what is needed
	- rapidly growing companies 
	- When you not sure of the demand
	- Google Compute Engine 

- PAAS 	
  - Complete development and deployment environment in cloud 
  - purchase resources when you need 
  - Includes IAAS (Services, Storage & networking)
  - also includes middle-ware, development tools and business intelligence 
  - Designed to support complete cycle of web apps
  - Avoid expenses of buying / Managing software licenses 
  - you manage the application and services 
  - PAAS scenarios  
    - Development framework 
	- Analytics 
	- Business Intelligence 
	- Security 
	- Middle-ware 
  - Advantages 
    - Cut coding time (work flow directory service)
	- provides inbuilt tools
	- Develop for multiple platforms (mobile)
	- Pay as you go 
	- efficiently manage application life cycle 
	- Geographically distributed 
  - When to use PASS
    - Multiple developers working on same project 
	- own customized applications 
	- if you rapidly developing and deploying application
  - Examples 
	- AWS ELastic Bean Stalk 
	- Google app Engine 
	- windows Azure 

- SAAS 
  - Connects to cloud based apps over Internet
  - SaaS also provides infrastructure as a service.
  - Email, Office 365
  - COmplete softwARE Solution 
  - Pay as you go
  - service provider manages software and hardware 
  - SAAS scenarios 
    - Outlook
	- Yahoo mail
	- Gmail 
	- EMail, Calender, Scheduling 
	- CRM 
	- ERP 
  - Advantages
    - pay only what you use 
	- no need to worry of hardware or software 
	- free client software (browser)
	- Access app data from anywhere 
  - When to use SAAS
    - short term projects requires collaboration 
	- applications need both web and mobile access 
  - Example 
    - Google Apps
	- Cisco Webex 
	- Drop box 
	- Concur 
	- GotoMeeting

- Public Cloud	
  - Public Internet 
  - Shared with other tenants 
  - Low cost 
  - No maintenance
  - near unlimited scalability
  - high reliability 
  
- Private Cloud 
  - Exclusively for one business or organization
  - located at organization's on-site data center or hosted by third party service provider  
  - Private network 
  - customize resources  for own requirements
  - more flexibility
  - more security 
  - Resources are not shared with others 
  - higher level of control and security 
  - high efficiency and low latency 
  
- Hybrid Cloud 
  - The best of both worlds
  - Combine on-premises infrastructure (or) private clouds with public clouds 
  - Organization can host sensitive data in private cloud and public data in public cloud 
  - Data and apps can move between private and public clouds 
  - Cloud bursting 
    - When spike in demand occurs, organization can burst to public cloud and tap into additional computing resources 
	- Example: Tax software
  - Control 
  - Flexibility 
  - cost effective 
  - Ease
	
	 
  
	
	
  
	
  
   
 
