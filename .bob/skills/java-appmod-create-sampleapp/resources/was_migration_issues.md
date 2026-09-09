The file contains a list of migration issues when migrating from traditional WAS to Liberty. The skill will select some of the skills, by default:
- all issues with automated fixes
- one or two of the issues with no automated fix


# Issues with automated fixes

1. WSSecurityHelper.revokeSSOCookies
    - Technology issue: Avoid using the deprecated WSSecurityHelper revokeSSOCookies and getLTPACookieFromSSOToken methods
    - Automatic fix: yes
    - How to implement: Implement a **logout method** that calls `WSSecurityHelper.revokeSSOCookies(HttpServletRequest, HttpServletResponse)`
    

2. WSSecurityHelper.getLTPACookieFromSSOToken
    - Technology issue: Avoid using the deprecated WSSecurityHelper revokeSSOCookies and getLTPACookieFromSSOToken methods
    - Automatic fix: yes
    - How to implement: Implement an **SSO method** that retrieves the SSO token details via the method "WSSecurityHelper.getLTPACookieFromSSOToken()". Keep in mind that getLTPACookieFromSSOToken() returns Cookie not String


3. com.ibm.websphere.runtime.ServerName
    - Technology issue: Getting the server name on Liberty
    - Automatic fix: yes
    - How to implement: Implement a method that displays the server display name retrieved by the methods "com.ibm.websphere.runtime.ServerName.getDisplayName()".


4. websphere.naming.WsnInitialContextFactory
    - Technology issue: Use the default InitialContext JNDI properties
    - Automatic fix: yes
    - How to implement: Implement a **JNDI lookup** button.  Use the WebSphere-specific JNDI object using `InitialContext` with the property: `java.naming.factory.initial=com.ibm.websphere.naming.WsnInitialContextFactory`



# Issues without automated fixes 

1. com.ibm.wsspi.ssl.RetrieveSignersHelper
    - Technology issue: Some WebSphere Security APIs and SPIs are unavailable
    - Automatic fix: no
    - How to implement: Implement a **certificate validation method** using the `RetrieveSignersHelper` SPI (`com.ibm.wsspi.ssl`)


2. com.ibm.websphere.servlet.response.ResponseUtils
    - Technology issue: The WebSphere Servlet API was superseded by a newer implementation
    - Automatic fix: no
    - How to implement: Implement a "Feedback" page to enter feedback. The inserted comments will be encoded using the method ResponseUtils.encodeDataString from the package `com.ibm.websphere.servlet.response.ResponseUtils`


3. Do not use the same XmlType name across multiple classes
    - Technology issue: Do not use the same XmlType name across multiple classes
    - Automatic fix: no
    - How to implement: Implement three JAX-WS web services which the following details
      - getUserByID with @XmlType(name="retryEventRequest")
      - getUserByName with @XmlType(name="retryEventRequest")
      - getUserByFullName with @XmlType(name="retryEventRequest")


4. Review use of the dynamic cache service
    - Technology issue: Do not use the same XmlType name across multiple classes
    - Automatic fix: no
    - How to implement: 
    Implement a method that uses the WebSphere Dynamic Cache Service API (`DistributedMap`) to implement caching.

5. JAX-RPC is supported in Liberty
    - Technology issue: Migrate JAX-RPC to JAX-WS
    - Automatic fix: no
    - How to implement: Implement a JAX-RPC service to retrieve the userid
    - Important:
        - Make sure to import javax.xml.rpc.* and to add a JAX-RPC client-side class that references javax.xml.rpc.Service or javax.xml.rpc.Stub. 
        - Add the IBM-specific WAS deployment extension file ibm-webservices-ext.xmi and if needed also ibm-webservices-bnd.xmi. These are the WAS-specific web services descriptor files that identify a WAS JAX-RPC deployment.



