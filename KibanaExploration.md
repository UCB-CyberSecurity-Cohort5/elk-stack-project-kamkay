## Activity File: Exploring Kibana

* You are a DevOps professional and have set up monitoring for one of your web servers. You are collecting all sorts of web log data and it is your job to review the data regularly to make sure everything is running smoothly. 

* Today, you notice something strange in the logs and you want to take a closer look.

* Your task: Explore the web server logs to see if there's anything unusual. Specifically, you will:

:warning: **Heads Up**: These sample logs are specific to the time you view them. As such, your answers will be different from the answers provided in the solution file. 

---

### Instructions

1. Add the sample web log data to Kibana.
  - Navigate to Kibana home page
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/1.KibanaHome.png)
  - Add sample web logs data, click view data to reach dashboard
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/2.LoadDataSet.png)
  - Dashboard 
    ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/3.LogWebData.png)

2. Answer the following questions:

    - In the last 7 days, how many unique visitors were located in India?
       - 249 
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/4.UniqueVisitors.png)
    - In the last 24 hours, of the visitors from China, how many were using Mac OSX?
       - 8
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/5.%20ChinaMacOSX.png)
    - In the last 2 days, what percentage of visitors received 404 errors? How about 503 errors?
       - 503 = 1.02% & 404 = 6.122%
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/6.503%26404.png)
    - In the last 7 days, what country produced the majority of the traffic on the website?
       - China = 61
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/7.uniquebycountry.png)
    - Of the traffic that's coming from that country, what time of day had the highest amount of activity?
       - Hour 13
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/8.hour13.png)
    - List all the types of downloaded files that have been identified for the last 7 days, along with a short description of each file type (use Google if you aren't sure about a particular file type).
       - gz: file format and software application used for file compression and decompression. This file format is primarily used on Unix operating systems. [Reference](https://fileinfo.com/extension/gz)
       - zip: archive file format that supports lossless data compression. Can also contain more than one file or directory which is compressed into a single zipped file. This allows for easy download, and transportation of the file. [Reference](https://www.google.com/search?client=firefox-b-1-d&sxsrf=APq-WBtytD_DfMoJEyNpHHucJlqscNDqAg:1648595723928&q=ZIP+(file+format)&si=ANhW_NoCZx1_PD6GdONlC84cm3ga5T0mFwVILPoTDjPpD15GkmRsX8JQmsP479ylWBlGSfnLEGb5ZbMwR2RXqaO43EFrham3yEBszjLZwxSuseDTAcVQFSyiFKu9Mz5Jqv2u_a8YuJKVDY39GKFXGR42Filznq_Mk2TP0ZSg9s4pOC7cmZL5IzStAx5qCWgvjHv-W3ZMWsxv&sa=X&ved=2ahUKEwjy14LUuez2AhUMKUQIHQTqBccQ6RN6BAgYEAE&biw=798&bih=719&dpr=2)
       - css: Cascading style sheet file used to format the contents of a webpage. Contains customized global properties for displaying HTML elements. [Reference](https://fileinfo.com/extension/css)
       - deb: Debian Linux distribution, a software package primarily used to install and/or update Unix applications. [Reference](https://fileinfo.com/extension/deb)
       - rpm: Red Hat package manager file that's used to store installation packages on Linux operating systems. This type of file makes it easy to distribute, install, upgrade and even remove packages. [Reference](https://www.lifewire.com/rpm-file-2622217)
         ![alt text](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/Kibana/9.logdatafolder.png)

3. Now that you have a feel for the data, Let's dive a bit deeper. Look at the chart that shows Unique Visitors Vs. Average Bytes.
     - Locate the time frame in the last 7 days with the most amount of bytes (activity).
         - The located time frame that shows to have used the most amount of bytes is 15:00 to 18:00 
           ![alt text](
     - In your own words, is there anything that seems potentially strange about this activity?
         - From the screenshot above, we can gather that one unique user was able to generate enough activity that approximately 8,780 bytes of memory were used. For one user to generate that much activity is definitely suspicious if it was not a scheduled maintenance update or site update.  

4. Filter the data by this event.
     - What is the timestamp for this event?
       - 17:15 
         ![alt text](
     - What kind of file was downloaded?
       - The file type is unidentifiable
         ![alt text](
     - From what country did this activity originate?
       - China
         ![alt text](
     - What HTTP response codes were encountered by this visitor?
       - 404
         ![alt text](

5. Switch to the Kibana Discover page to see more details about this activity.
     - What is the source IP address of this activity?
     - What are the geo coordinates of this activity?
     - What OS was the source machine running?
     - What is the full URL that was accessed?
     - From what website did the visitor's traffic originate?

6. Finish your investigation with a short overview of your insights. 

     - What do you think the user was doing?
     - Was the file they downloaded malicious? If not, what is the file used for?
     - Is there anything that seems suspicious about this activity?
     - Is any of the traffic you inspected potentially outside of compliance guidlines?

---
© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.  