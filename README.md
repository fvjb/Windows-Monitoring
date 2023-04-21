## WINDOWS MONITORING
### Application Whitelisting
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

The value propositions above do fully contribute to Cybersecurity objectives, as they do help to diminish the attack surface of an IT environment. Some organizations are guided by frameworks, such as __BSI Grundschutz__ - [Application Whitelisting, SYS.2.2.3.A5](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Grundschutz/Kompendium_Einzel_PDFs_2021/07_SYS_IT_Systeme/SYS_2_2_3_Clients_unter_Windows_10_Edition_2021.pdf), __ISO 27001__ - [Application Whitelisting, 12.2.1](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Grundschutz/Kompendium/Zuordnung_ISO_und_IT_Grundschutz.pdf), __KRITIS__ or some of the industry-specific standards that arose over the last couple of years, requiring to implement some type of Application Whitelisting Solution. 

This can cause massive headaches in IT Teams, as they are aware that such projects failed in the past. They know they don't have the necessary transparency to create the complete Application Whitelist representing the condition for a seamless rollout. 


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
    - [Installing the Illuminate Pack for proper parsing](https://go2docs.graylog.org/5-0/what_more_can_graylog_do_for_me/graylog_illuminate.html)
    - [Configuring CLM importing the Applocker Demo Content Pack](https://github.com/fvjb/Windows-Monitoring/tree/master/contentpacks)
    - [Configure Windows Servers to create the required Logs](https://github.com/fvjb/Windows-Monitoring/tree/master/grouppolicy)
    - Configure Windows Servers to send the required Logs
        - [Sidecar](https://go2docs.graylog.org/5-0/getting_in_log_data/graylog_sidecar.html)
        - [NXLog - Gelf]() - optional
    - Verify Results: Open the __"Windows - Applocker"__ Dashboard and start investigating: 

    ![Dashboard](https://github.com/fvjb/Windows-Monitoring/blob/master/images/Applocker-5.png)

- Determine legitimate and non-authorized applications
    - Investigate the "Application Overview" Dashboard and understand the user-application-system triangle 
        - __who uses__ (user_name) 
        - __what application__ (apppath) 
        - __on which system__ (source) with 
        - _what consequences (action)_
    - Export relevant data required for organization-wide discussions and potential further processing in Excel

    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/images/Applocker-11.png)

    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/images/Applocker-13.png)

- Create Application Whitelist
    - Work through all applications and understand which of these require your attention. The base path ("appbase") can be helpful when prioritizing activities. 
    
    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/images/Applocker-15.png)

- Refine Application Whitelist (optional)
    - Once you have a list with applications to be whitelisted, feed them back into the system as lookup table. You can then focus on all applications that are not contained in the list but logged anyway. 

    ![Export](https://github.com/fvjb/Windows-Monitoring/blob/master/images/Applocker-15.png)

- Rollout Applocker