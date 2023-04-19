## WINDOWS MONITORING
### Windows AppLocker
Windows Environments often run many applications that we are not aware of. This might represent a significant risk to IT. To allow a higher level of control on what applications run on a Windows System, AppLocker was introduced about 15 years ago. 

Most Administrators hesitate to start using it in production. Many projects happened to fail due to the fact that Applocker is basically whitelisting applications ([enjoy this article from 11 years ago](https://www.itninja.com/blog/view/the-right-wrong-ways-to-prevent-inappropriate-app-execution)) - and the success of a project depends on the whitelist to be complete. As usual, those whitelists were never complete and required functionality broke, availability of relevant business services degraded and after rolling back all AppLocker related changes no one ever tried using this feature again. 

However, 