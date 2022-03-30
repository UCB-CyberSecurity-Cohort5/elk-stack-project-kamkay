# Step By Step ELK Install

This was a three day project, This document is set up as so.

## Day 1 - ELK Installation
---
1. Create a new virtual network specifically for the ELK server
  - navigate to Virtual Networks page
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/1.%20go%20to%20virtual%20networks%20page.png)
  - click Create ensure resource group is the same as the RedTeam VN
  - *Name* for the ELK server will be ELKNetwork
**screenshot above was taken after the vNet was created thus discrepancy between network name and blank filled in below**
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/02.%20create%20new%20network.png)
  - select default as shown below for *IP Addresses*
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/03.%20click%20default%20subnet.png)
  - defaults okay for *Security* and *Tags*
  - once validation is passed, click *Create*
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/04.%20verify%26create.png)
2. Create a *Peer connection* between the two vNets
 **allows for traffic to pass between each of the vNets and regionally located servers**
  - navigate to *Virtual Network* page in the Azure portal
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/05.%20go%20to%20vn's%20to%20add%20peerlink.png)
  - select the *ELKNetwork* vNet
  - click *Peerings* icon to navigate to peerings section
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/06.%20click%20ELKNet%20%26%20find%20peerings.png)
  - click *Add* to create new peering
  - name the peering link  
    - *ELKtoRed.plink*
  - keep defaults 
  - name the remote network peering link with a unique name 
    - *RedtoELK.plink*
  - select wanted vNet for *Virtual Network*
  - keep defaults and click *Add* 
     **please ignore warning, screenshot was created after initial completion of step**
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/07.1%20add%20peering.png)
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/07.2%20add%20peering%20.png)

3. Create a new VM for the ELK stack
  - navigate to *Virtual Machine* page
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/09.%20find%20vm%20tab.png)
  - click *create*
  - choose wanted *Resource group* 
    - *RedTeam*
  - *name* the new virtual machine
    - *ELKServer*
  - choose wanted *Region*
    - *US West 2* 
      - **same as ELK vnet**
  - select 'No infrastructure redundancy required' for *Availability options*
    **this is because it is specifically for the ELK stack and will not be needing backups of the machine** 
  - *standard* is fine for *Security type*
  - *Image* should be 'Ubuntu Server 18.04 LTS - Gen2'
  - *size needs to be at least 8 GiB to be able to support the ELK stack
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.%201%20create%20new%20vm%20basics.png)
  - choose SSH public key for a secure way to access the VM
  - select an appropriate username 
    - *ELKuser*
  - Input public key 
    - *For this project, our ssh key was located inside of the ansible container located on the Jumpbox*
     *ssh public key not shown in screenshot*
  - click *Allow selected ports* for *public inbound ports*
  - click *Next:Disks*
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.2%20create%20vm%20basics.png)
  - click *Delete with VM*
  - defaults for *Disks* should be fine
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.5.1%20next.png)
  - make sure *Virtual network* is the correct one
    - *ELKNetwork*
  - *subnet* should be default
  - select *Basic* for *NIC network security group*
  - default for *select inbound ports* should be *SSH (22)*
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.3%20create%20new%20pub%20ip.png)
  - select *Delete public IP and NIC when VM is deleted*
  - click *Next:Management*
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.4%20delete%20when%20vm%20deleted.png)
  - defaults should be fine for *Management*
  - click *Next:Advanced*
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.5%20next.png)
  - defaults for *Advanced* should be fine
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.6%20next.png)
  - add tags for the web server if wanted 
  **we left it empty for this project**
  - click *Next:Review+Create*
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.7%20nxt.png)
  - once validation is passed, click *Create*
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.8%20create.png)

4. Downloading and Configuring the Container
