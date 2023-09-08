#### Challenge
One of the most challenging tasks of a Windows Administrator is to keep track and execute control on all applications being used within the scope of an organizations IT environment; however, there are many reasons to do it:
- __detecting__ all applications being used on Server and Client devices 
- __controlling__ use of all undesired, unlicensed, insecure, unauthorized, non-business related software
- __preventing__ IT environments against compromise by monitoring and suppression of executables used by attackers or careless insiders

Being known for quite a long time, the Application Whitelisting Approach didn't make it into the average IT environment, as results of such initiatives turned out to be predictably unpredictable with many side-effects. The reason for failure? Inability to create an accurate Application Whitelist, thus breaking required functionality right after Rollout enforcing immediate Rollback and causing a lot of "feedback" from Business Units. 

For more than 15 years, Microsoft offers a builtin solution to control application usage in Windows environments for Server Operating Systems: [AppLocker](https://learn.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview). Promoting it, the following advantages are named by [the vendor](https://learn.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview#:~:text=AppLocker%20addresses%20the%20following%20app%20security%20scenarios): 
- Application inventory
- Protection against unwanted software
- Licensing conformance
- Software standardization
- Manageability improvement

The value propositions above do fully contribute to Cybersecurity objectives, as they do help to significantly diminish the attack surface of an IT environment. Some organizations are guided by frameworks, such as __BSI Grundschutz__ - [Application Whitelisting, SYS.2.2.3.A5](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Grundschutz/Kompendium_Einzel_PDFs_2021/07_SYS_IT_Systeme/SYS_2_2_3_Clients_unter_Windows_10_Edition_2021.pdf), __ISO 27001__ - [Application Whitelisting, 12.2.1](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Grundschutz/Kompendium/Zuordnung_ISO_und_IT_Grundschutz.pdf), __KRITIS__ or some of the industry-specific standards that arose over the last couple of years, requiring to implement some type of Application Whitelisting Solution. 

While the advantages are obvious - attackers can't execute scripts or applications on your servers and endpoints any more - starting an implementation project can cause massive headaches in IT Teams, as they are aware that such projects failed in the past. They know they don't have the necessary transparency to create the complete Application Whitelist representing the condition for a seamless rollout. 


#### Approach
What needs to be done, though? 
- Collect Audit Data to ensure all applications running on systems to be protected by the Application Whitelisting mechanism are __fully documented__
- Based on the data collected in the inventory, __determining legitimate and non-authorized applications__; setting up appropriate communication channels with teams and individuals (to allow this software to be removed by those) 
- Based on inventory data, __creating an Application Whitelist__; continue collecting Audit Data related to this Application Whitelist and determining what applications it would cause to break
- Adapting the Whitelist according to assessment data; __iteratively refining__ it until no undesired effects are reported 
- __Rollout__: Rolling out AppLocker, starting with a hand full of systems; thoroughly monitoring impact
- Continuous Security Monitoring: Keep an overview on all applications being blocked, as this might be an indicator of compromise


#### Solution
How to achieve this? 
- Collect Audit Data: 
    - [Installing a Centralized Log Management System (CLM)](https://go2docs.graylog.org/5-0/downloading_and_installing_graylog/installing_graylog.html)
        - [Repo: Docker-Compose](https://github.com/Graylog2/docker-compose) for DEV/TEST environments (_real quickstart_)
        - [Video: Ubuntu Install](https://www.graylog.org/videos/ubuntu-install)
    - [Getting a license key](https://go2.graylog.org/request-graylog-operations)
    - [Installing the Illuminate Pack](https://go2docs.graylog.org/5-0/what_more_can_graylog_do_for_me/graylog_illuminate.html)
    - [Configuring CLM importing the Applocker Demo Content Pack](https://github.com/fvjb/Windows-Monitoring/tree/master/contentpacks)
    - [Configure Windows Servers to create the required Logs](https://github.com/fvjb/Windows-Monitoring/tree/master/grouppolicy)
    - Configure Windows Servers to send the required Logs
        - [Install Graylog Sidecar](https://go2docs.graylog.org/5-0/getting_in_log_data/graylog_sidecar.html)
        - [Install NXLog]() - optional
        - [Configure Sidecars](https://go2docs.graylog.org/5-0/getting_in_log_data/graylog_sidecar.html#SidecarConfiguration)
    - Verify Results: Open the __"Windows - Applocker"__ Dashboard and start investigating: 


    ![Dashboard](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-1.png)


- Now determine legitimate and non-authorized applications
    - Start with the "Application Overview" Dashboard and find out
        - what application types are mostly used within your organization
        - how many applications you have to consider in the first step
        - what base directories those applications start from 

    - Investigate the "Application List" and understand the user-application-system triangle 
        - __who uses__ (user_name) 
        - __what application__ (apppath) 
        - __on which system__ (source) with 
        - _what consequences (appaction)_

        ![Dashboard](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-4.png)

    - Notice that the flag "appteam" is unused. In case the number of applications is high and you need multiple teams to work with you, we recommend to enrich your logs with the responsibility flag ("appteam"): 
        - create a csv-list of systems with the corresponding teams following this format and make sure it's utf-8 encoded: ([Template](https://github.com/fvjb/Windows-Monitoring/blob/master/lookuptables/my-servermanagementlist.csv))
        - upload this list to your graylog server(s) and store it in __"/etc/graylog/lookup-tables"__ (must be the same path on all machines)
        - make sure the user "graylog" has __read and write permissions__ on the file (_chown graylog:graylog yourlookuptable.csv_)
        - now you will get the required context to assign and delegate responsibilities: 

        ![Dashboard](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-5.png)

    - Export relevant data required for organization-wide discussions and potential further processing in Excel; get as much context data as you need to have successful conversation with other team members or departments.
        - create a list with relevant log messages: 

        ![Dashboard](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-2.png)

        - create a CSV export file to leverage collaboration with other teams: 

        ![Dashboard](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-3a.png)

        - make sure you get all fields you need for making reliable decisions (block/allow) and grant technical uniqueness ("appid")

        ![Dashboard](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-7.png)

    - Open it in Excel (or any other appropriate tool) review the Information you collected; now you can decide to split the list and let different teams work on figuring out what applications are legitimate and what applications are not. 

    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-8.png)

- Create Application Whitelist
    - Work through all applications and understand which of these require your attention. The base path ("appbase") can be helpful when prioritizing activities. Add a column "appcat" and decide how to categorize an application
        - "AUTHORIZE" or
        - "SUPPRESS" 

    - CAREFUL: "appcat" __MUST__ reside between "appid" and "apppath" (if you want to keep this column)
    
    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-12a.png)

    - You then remove all columns and just leave "appid" and "appcat" (and "apppath", if you find it helpful for having human-readable App identifiers)
    
    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-13.png)

    -  Make sure to remove all double-entries per "appid": 

    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-15.png)

    - Be careful to export your working results from Excel (or any other tool) using the right format ([Example](https://github.com/fvjb/Windows-Monitoring/blob/master/lookuptables/my-applicationwhitelist.csv)): 

    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-14.png)

    - Feed the Outcome of your work to Graylog: 
        - upload this list to your graylog server(s) and store it in __"/etc/graylog/lookup-tables"__ (must be the same path on all machines)
        - make sure the user "graylog" has __read and write permissions__ on the file (_chown graylog:graylog my-applicationwhitelist.csv_)
        - now you will get the required context to assign and delegate responsibilities:


- Refine Application Whitelist 
    - Now you can analyze what's going on within the monitored environment. Identify all uncategorized applications and make sure you define whether to flag them as "AUTHORIZED" or "SUPPRESSED". Try to get the number of uncategorized applications to "0". 

    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-19.png)

    - Try then to identify inconsistend configurations: 
        - Whitelisted Applications that are indicated to be blocked
        - Blacklisted Applications that are indicated to be run

    - Once you updated your application whitelist, make sure to feed it back to the system and review the Dashboard afterwards

    __CAREFUL__ the additional information is only added to incoming logs. __The existing logs will not be tagged__. During the phase of continuous refinement you may want to overwrite the Search Time Range to have results that properly reflect your changes: 

    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-20.png)

    - Example: You find a number of Apps, that would be blocked while you categorized them as "AUTHORIZE": 

    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-22.png)

    - Take action and adopt your Applocker Policy accordingly to allow this execution. Go through this process until you get a result confirming that all "AUTHORIZED" sofware is allowed by Applocker: 

    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-24.png)

- Rollout Applocker Enforcement Policies
    - It's time to get started. Apply your first enforcement rule: 
        - keep the scope tight (do __NOT__ enroll it throughout your entire environment but start with selected systems first)
        - make sure to work with the "Whitelist Validation (complete)" Dashboard, otherwise you migth miss default applications and see failures in operations
        - use Graylog to monitor the outcome of what you did
        - whenever you find a blocked Application, look into it: 

        ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-25.png)

        ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-26.png)

        ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/Application%20Whitelisting/images/Applocker-27.png)

        - this example shows an application, that was marked as "AUTHORIZED", but the Applocker Policy allowed access for a specific user group only. 

- Good Luck!