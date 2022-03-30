# Step By Step ELK Install

This was a three day project, This document is set up as so.

---
## Day 1 - ELK Installation

1. Create a new virtual network specifically for the ELK server
  - navigate to Virtual Networks page
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/1.%20go%20to%20virtual%20networks%20page.png)
  - click Create ensure resource group is the same as the RedTeam VN
  - *Name* for the ELK server will be ELKNetwork
      *screenshot above was taken after the vNet was created thus discrepancy between network name and blank filled in below*
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/02.%20create%20new%20network.png)
  - select default as shown below for *IP Addresses*
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/03.%20click%20default%20subnet.png)
  - defaults okay for *Security* and *Tags*
  - once validation is passed, click *Create*
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/04.%20verify%26create.png)
2. Create a *Peer connection* between the two vNets
 *allows for traffic to pass between each of the vNets and regionally located servers*
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
     *please ignore warning, screenshot was created after initial completion of step*
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
      - *same as ELK vnet*
  - select 'No infrastructure redundancy required' for *Availability options*
    *this is because it is specifically for the ELK stack and will not be needing backups of the machine* 
  - *standard* is fine for *Security type*
  - *Image* should be 'Ubuntu Server 18.04 LTS - Gen2'
  - *size needs to be at least 8 GiB to be able to support the ELK stack
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.%201%20create%20new%20vm%20basics.png)
  - choose SSH public key for a secure way to access the VM
  - select an appropriate username 
    - *ELKuser*
  - Input public key 
    - *For this project, our ssh key was located inside of the ansible container located on the Jumpbox*
     - *ssh public key not shown in screenshot*
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
  - default for *select inbound ports* should be 'SSH (22)'
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
     - *we choose not to add tags for this project*
  - click *Next:Review+Create*
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.7%20nxt.png)
  - once validation is passed, click *Create*
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/11.8%20create.png)

4. Downloading and Configuring the Container
  - add the new ELK server vm to the Ansible *hosts* file 
    - edit with `nano /etc/ansible/hosts`
    - when adding the new vm, also make sure to specify python3 with `ansible_python_interpreter=/usr/bin/python3`
    - the following screenshot shows the ELK machine listed under the elk group as well as the preexisting web servers group added in a previous assignment 
     ![alt text](hosts+elk
  - create a new ansible playbook to install the ELK container
    - cd `/etc/ansible` to ensure your in the correct file
    - create new playbook `nano install-elk.yml`
    - *tasks* should do the following 
      - *remote_user* specified username for the ELKServer as it is different then the rest of the network username 
      - increase memory of the elk server in order to handle the processing power the ELK stack requires
      - install 'docker.io' 'python3-pip' 'docker'
         - 'docker.io' and 'python3-pip' - `apt` packages
         - 'docker' is a Docker Python pip module - `pip` package
      - download the Docker container `sebp/elk:761`
         - sebp is the organization that made the container
         - elk is the container
         - 761 is the version
      -  configure the `sebp/elk:761` container to start with the following port mappings
         - `5601:5601`
         - `9200:9200`
         - `5044:5044`
      - start the container
      - enable the `docker` service at boot up 
         - allows the ELKserver to restart the docker service at boot up
      ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/Playbooks/install-elk.yml)

5. Launching and Exposing the Container
  - run playbook with `ansible-playbook install-elk.yml`
    - *ran twice due to error in playbook*
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/unsuccessful%20run%201%20of%20playbook.png)
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/successful%20run%202%20of%20playbook%20.png)
  - ssh into the ELKserver to make sure `sebp/elk:761` is properly installed
    - *screenshot was taken after completion of project, so `sudo docker container list` was used to show the container present on the ELKserver*
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/dockerListOutput.png)

6. Identity and Access Management
  - specify and add your public IP address to be able to access the ELKserver via the webpage
    - add your public IP to the ELKserver-nsg *ELK server Network Security Group*
    - navigate to network security groups page and locate the ELKserver-nsg
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/elk%20server%20nsg%201.png)
    - click *add* to create new rule
      - *Source* should be 'IP Addresses'
      - *Source IP addresses* should be your public IP address 
      - *Source port ranges* can be *
      - *Destination* should be 'Any'
      - *Destination port ranges* should be '5601'
        - This is the specific port that the ELK stack uses
      - *Protocol* should be 'TCP'
      - *Action* is 'Allow'
      - *Priority* should be anything less in value than the default rules
        - *298*
      - *Name* it something unique
        - *ELKaccessport*
      - click *add*
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/elk%20server%20add%20rule.png)
  - once rule is added, your screen should look like this 
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/elk%20access%20rule%20added%20completed.png)
  - verify your access of the server by navigating to `http://[ELKserverPublicIP]:5601/app/kibana` 
    - homepage should look like the following 
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/screenshots/successful%20access%20of%20elk%20server%20via%20webpage.png)

END OF DAY 1
---

## Day 2 - Filebeat and Metricbeat Installation

1. Installing Filebeat on the DVWA container
  - Navigate to `http://[ELKserverPublicIP]:5601/app/kibana`
    - click 'Explore on my own' if prompted
  - From homepage click *add log data*
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Day2scs/click%20add%20data.png)
  - click system logs
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Day2scs/click%20system%20logs.png)
  - navigate to deb and follow instructions
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Day2scs/deb%20and%20follow.png)
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Day2scs/step1install.png)
  - edit the configuration file `/etc/filebeat/filebeat-config.yml` and cp it to the ansible file `/etc/ansible/`
    - `nano /etc/filebeat/filebeat-config.yml`
    - add the following to the file under the 'Kibana' and 'ElasticSearch output'
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Day2scs/filebeatconfig1.png)
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Day2scs/filebeatconfig2.png)
    - `cp /etc/filebeat/filbeat-config.yml /etc/ansible`
 
2. Create the Filebeat Installation playbook + installation and verification
  - make sure you are in the anisble file `cd /etc/ansible/roles`
    - *roles* is where I put my playbooks
  - `nano filebeat-playbook.yml`
   - *tasks* should be the following
    - download the `.deb` file via curl
    - install the `.deb` file using `dpkg`command
    - copy the filebeat config file from your ansible container to your WebVM's filebeat file
    - run the `filebeat modules enable system` command
    - run the `filebeat setup` command
    - run the `service filebeat start` command
    - enable the filebeat service at boot up 
     [filebeat-playbook.yml](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/Playbooks/filebeat-playbook.yml)
  - run via `ansible-playbook filebeat-playbook.yml`
    - successful run of the playbook will look similar to the output we saw for the elk install playbook run 
  - navigate to the Filebeat installation page and verify data is reaching the ELK stack 
  - click *Check Data* if done correctly, the output should look like the following
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Day2scs/filebeatsuccess.png)

3. Create the Metricbeat Installation playbook + installation and verification
  - navigate to docker metrics, from home, add data, and choose docker metrics
    - same path as previous step so refer to that if confused 
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Day2scs/docker%20metric%20homepage.png)
  - run step one on your elk server 
  - create metric beat playbook
    - ensure you are in the anible file, same path as the previous step
      - `nano metricbeat-playbook.yml`
      - *tasks* should be the following
        - download the Metricbeat .deb file
        - use `dpkg` to install the `.deb` file
        - copy the metric beat config from your ansible container to your WebVM's filebeat file 
        - run the `metricbeat modules enable docker` command
        - run the `metricbeat setup` command
        - run the `metricbeat -e` command
        - enable the metricbeat service on boot
     [Metricbeat-playbook.yml](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/Playbooks/metricbeat-playbook.yml)
  - run the play book `ansible-playbook metricbeat-playbook.yml`
  - verify the connection on the Kibana gui (webpage)
     ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Day2scs/metricsuccess.png)

END OF DAY 2
---

## Day 3

Day 3 consisted of completing the Kibana Exploration file, linked below. 
     ![KibanaExplorationFile](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/KibanaExploration.md

Additionally, the network diagram present within the Readme.md file was created. Also provided below. 
     ![Readme.md](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/README.md)

Lastly, we worked on sample interview questions. Mine is provided below 
     ![SampleInterviewQuestion.md](

END OF DAY 3
---
