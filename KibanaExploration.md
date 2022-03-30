## Activity File: Exploring Kibana

* You are a DevOps professional and have set up monitoring for one of your web servers. You are collecting all sorts of web log data and it is your job to review the data regularly to make sure everything is running smoothly. 

* Today, you notice something strange in the logs and you want to take a closer look.

* Your task: Explore the web server logs to see if there's anything unusual. Specifically, you will:

---

### Instructions

1. Add the sample web log data to Kibana.
  - Navigate to Kibana home page
    *click 'load a data set and a Kibana dashboard'*
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/01.KibanaHome.png)
  - Add sample web logs data, click view data to reach dashboard
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/02.LoadDataSet.png)
  - Dashboard 
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/03.LogWebData.png)

2. Answer the following questions:

    - In the last 7 days, how many unique visitors were located in India?
       - 249 
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/04.UniqueVisitors.png)
    - In the last 24 hours, of the visitors from China, how many were using Mac OSX?
       - 8
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/05.%20ChinaMacOSX.png)
    - In the last 2 days, what percentage of visitors received 404 errors? How about 503 errors?
       - 503 = 1.02% & 404 = 6.122%
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/06.503%26404.png)
    - In the last 7 days, what country produced the majority of the traffic on the website?
       - China = 61
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/07.uniquebycountry.png)
    - Of the traffic that's coming from that country, what time of day had the highest amount of activity?
       - Hour 13
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/08.hour13.png)
    - List all the types of downloaded files that have been identified for the last 7 days, along with a short description of each file type (use Google if you aren't sure about a particular file type).
       - gz: file format and software application used for file compression and decompression. This file format is primarily used on Unix operating systems. [Reference](https://fileinfo.com/extension/gz)
       - zip: archive file format that supports lossless data compression. Can also contain more than one file or directory which is compressed into a single zipped file. This allows for easy download, and transportation of the file. [Reference](https://www.google.com/search?client=firefox-b-1-d&sxsrf=APq-WBtytD_DfMoJEyNpHHucJlqscNDqAg:1648595723928&q=ZIP+(file+format)&si=ANhW_NoCZx1_PD6GdONlC84cm3ga5T0mFwVILPoTDjPpD15GkmRsX8JQmsP479ylWBlGSfnLEGb5ZbMwR2RXqaO43EFrham3yEBszjLZwxSuseDTAcVQFSyiFKu9Mz5Jqv2u_a8YuJKVDY39GKFXGR42Filznq_Mk2TP0ZSg9s4pOC7cmZL5IzStAx5qCWgvjHv-W3ZMWsxv&sa=X&ved=2ahUKEwjy14LUuez2AhUMKUQIHQTqBccQ6RN6BAgYEAE&biw=798&bih=719&dpr=2)
       - css: Cascading style sheet file used to format the contents of a webpage. Contains customized global properties for displaying HTML elements. [Reference](https://fileinfo.com/extension/css)
       - deb: Debian Linux distribution, a software package primarily used to install and/or update Unix applications. [Reference](https://fileinfo.com/extension/deb)
       - rpm: Red Hat package manager file that's used to store installation packages on Linux operating systems. This type of file makes it easy to distribute, install, upgrade and even remove packages. [Reference](https://www.lifewire.com/rpm-file-2622217)
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/09.logdatafolder.png)

3. Now that you have a feel for the data, Let's dive a bit deeper. Look at the chart that shows Unique Visitors Vs. Average Bytes.
     - Locate the time frame in the last 7 days with the most amount of bytes (activity).
         - The located time frame that shows to have used the most amount of bytes is 12:00 to 15:00
           ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/11.png)
           ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/12.png)
     - In your own words, is there anything that seems potentially strange about this activity?
         - From the screenshot above, we can gather that one unique user was able to generate enough activity that approximately 16,750 bytes of memory were used. For one user to generate that much activity is definitely suspicious if it was not a scheduled maintenance update or site update.  

4. Filter the data by this event.
     - What is the timestamp for this event?
       - 14:58:15
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/13.png)
     - What kind of file was downloaded?
       - The file type is gz
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/14.png)
     - From what country did this activity originate?
       - China
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/15.png)
     - What HTTP response codes were encountered by this visitor?
       - 200 success
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/16.png)

5. Switch to the Kibana Discover page to see more details about this activity.
       ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/21.png)
     - What is the source IP address of this activity?
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/17.png)
     - What are the geo coordinates of this activity?
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/18.png)
     - What OS was the source machine running?
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/19.png)
     - What is the full URL that was accessed?
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/20.png)
     - From what website did the visitor's traffic originate?
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/22.png)

6. Finish your investigation with a short overview of your insights. 

     - What do you think the user was doing?
       - The User was trying to download a gz file onto the web server. Specifically, a linux package gz file.  
     - Was the file they downloaded malicious? If not, what is the file used for?
       - The file itself may not be malicious, at this moment we cannot determine the contents of the file. It can be a system administrator installing new packages or updates on the web server. 
     - Is there anything that seems suspicious about this activity?
       - Not necessarily, it depends on what the contents of the file. Since that can not be determined, it is recommended to increase monitoring on specific user.   
     - Is any of the traffic you inspected potentially outside of compliance guidelines?
       - None of the inspected traffic is outside of compliance guidelines as it is a user downloading a file onto the server. However, further inquiry into this event and user should be conducted to fully determine the activity of the user. 

--- 