# Azure Notes

## Cloud concepts 

- High Availiaility
  - Scale your application with Load balancers. supports both inbound and outbound, low latency 
  
- Scalability
  - Scalability applies to
    - Disk I/O
	- CPU
	- Memory
	- Network I/O
 - Horizantal Scaling
   - Complex
   - Add more services and spread the load 
   - must sync the applications and data
   - Long term
 - Vertical Scaling 
   - Easier 
   - Adding more resouces (like memory, disk and cpu)
   - Short term
 - VM Scale Sets in Azure
   - Create and Manage groups of identical, load balanced VM's 
   - Allows to centerally manage, configure and update large number of VM's 

- Elasticity 
  - Ablity to scale up and down
  - Handle high volumn 
  - Use resouces only when needed.
  - Avoids over provisioning and under provisioning 
  - Autoscaling in Azure 
    - Add or remove VM's based on set of rules

- Agility
  - Value = Agility + Utilization 
  - Ability to provision and de-provison nearly unlimited resources as needed with complete control
  - Projects can be provisoned and deployed within 24 hours 
  
- Fault Tolerance 
  - Abiliy to contionue providing services even after the failure of one or more cloud components  
  - Availiablility Sets in Azure 
  
- Disaster Recovery 
  - Types 
    - Completely Unavailiable 
    - Partially availiable 
	- Fully availiable
  - Azure site recovery service 
    - Replicates workloads to a secondary location
	- it outage in primary site, fails over to secondary. once the outage is over fall back to primary. 
	
- CapEX vs OpEx
  - Capital Expenditures 
    - Money company spends on fixed assests
	- knows as PP&E - Property, Plant and Equipment 
	- One time purchases 
	- Servers, network devices, printers, scanners
	- Depreciation 
  - Operating Expenditures
    - Funds used to run day to day business 	
	- generally used up with in the year they purchased
	- printer cartridges, paper, electricity 
	- Web site hosting, web domain registrations, maintancer agreement, yearly service 
	- Pay as you go items 
	
  
	
  
   
 
