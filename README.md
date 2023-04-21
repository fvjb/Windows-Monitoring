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

#### Graylog
