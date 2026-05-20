# File Systems In The Cloud

## Goal
The Goal of the simulearn lab was to assist a pet modeling agency to share files across its branch offices without managing the physical infrastructure.

### The Situation at the Company is as follows:
---
The pet modeling agency wants a way to share files across its branch offices without managing physical storage infrastructure. In the past year, they have established **three new company branches in the city**. Each branch has its own pet image server that connects to a local client management application. Each server stores images of all our pet clients along with informational metadata. We use a custom application to sync the client data across all three branches. But it takes too much time to access the images, and it's not always consistent. Our synching solution fails sometimes because one branch server might run out of storage space. We need a solution that centralizes our image storage and scales automatically. all branches access and update the same pet client files. And we have certain folders for our VIP clients that only our concierge team has access to.
---
#### The situation at the agency can be visualized as follows:
![Agency Arch Diag](./Resources/FileSystemsInTheCloud/PetsShopOriginalArch.drawio.svg)

#### AWS Solution to the Agency's I.T issues
In summary, the agency's issues are centered around *Files Management and Storage*. Their other concerns regard latency issues, access management, synchronization, service availability, and scalability of resources as demand increases.

Amazon EFS meets the agency's requirements as follows"
- The agency can create *shared network drives* so that the branches can access pet client photos from a *central location*, instead of files and folders residing in discrete and isolated locations.
- They can *restrict access* with *file-level permissions* allowing the concierge team and the VIP client's maximum privacy.
- Amazon EFS provides *petabyte-scale storage* that *grows and shrinks* automatically as you add and remove files. This saves the agency money as they need not buy bigger storage for their servers or EC2 instances which they may not size appropriately (paying for unused storage)
- Amazon EFS is *highly available* and is designed for *99.999999999 percent durability*. Its a service that is always online and highly accessible/available.

### The Proposed AWS Solution
The proposed AWS EFS solution can be visualised below:
![*image from the actual lab. Property of AWS Simulearn*](./Resources/FileSystemsInTheCloud/img5.jpg)

#### The solution is further explained below:
- A web server is provisioned to a nearby availability zone that is closer to each branch, to eliminate/deal with latency, and to be a dedicated server for a specific office only. 
- The EFS file system enables simultaneous access for EC2 instances (AWS servers) within your VPC (Your own isolated(rented) section of the cloud), facilitating rapid management of dynamic content. 
- A mount target is created for the EFS, that provides an IP address endpoint for instances to connect to the file system. Web servers mount the target from their respective AZ (availability Zones) as a local folder on the instance.
- Instances interact with the EFS file system as local storage, **with changes visible to all authorized instances**
- As the agency grows, **with more files and folders to be stored (and accessed), more branches opened world wide meaning more employees, clients and VIP clients**, the file system (AWS EFS) automatically scales based on data volume. The file system accommodates additional instances through mount target creation and attachment in the appropriate AZ.
