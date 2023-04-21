## WINDOWS MONITORING
### Application Whitelisting
#### Applocker
One of the most challenging tasks of a Windows Administrator is to keep track and execute control on all applications being used within the scope of an organizations IT environment; however, there are many reasons to do it:
- detecting all applications being used on Server and Client devices 
- suppressing use of all undesired, unlicensed, insecure, unauthorized, non-business related software
- preventing IT environments to be compromised by monitoring and suppressing executables used by attackers

For more than 15 years, Microsoft offers a builtin solution to control application usage in Windows environments (for Server Operating Systems and selected Client Operating Systems) - [AppLocker](https://learn.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview). Being known for quite a long time, the Application Whitelisting Approach didn't make it into the average IT environment, as results of such initiatives turned out to be predictably unpredictable with many side-effects. 

The Centralized Log Management (CLM) approach allows to securely evolve through the value propositions brought up by [Microsoft](https://learn.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview#:~:text=AppLocker%20addresses%20the%20following%20app%20security%20scenarios): 
- Application inventory
- Protection against unwanted software
- Licensing conformance
- Software standardization
- Manageability improvement

However, this article focuses on organizations that try to improve their attack surface. In some cases, these organizations are guided by frameworks, such as _BSI Grundschutz_ - [Application Whitelisting, SYS.2.2.3.A5](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Grundschutz/Kompendium_Einzel_PDFs_2021/07_SYS_IT_Systeme/SYS_2_2_3_Clients_unter_Windows_10_Edition_2021.pdf), *ISO 27001* - [Application Whitelisting, 12.2.1](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Grundschutz/Kompendium/Zuordnung_ISO_und_IT_Grundschutz.pdf), _KRITIS_ or some of the industry-specific standards that arose over the last couple of years. 

#### Graylog
Graylog allows to collect, parse, normalize, enrich and analyze log data. This article describes how to prepare for a successful implementation of Application Whitelisting without catching all the usual pitfalls. The first pitfall - obv