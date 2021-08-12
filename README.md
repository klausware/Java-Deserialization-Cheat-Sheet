# Java-Deserialization-Cheat-Sheet
A cheat sheet for pentesters and researchers about deserialization vulnerabilities in various Java (JVM) serialization libraries.

Please, use **#javadeser** hash tag for tweets.

##  Table of content
- [Java Native Serialization (binary)](#java-native-serialization-binary)
	- [Overview](#overview)
	- [Main talks & presentations & docs](#main-talks--presentations--docs)
	- [Payload generators](#payload-generators)
	- [Exploits](#exploits)
	- [Detect](#detect)
	- [Vulnerable apps (without public sploits/need more info)](#vulnerable-apps-without-public-sploitsneed-more-info)
	- [Protection](#protection)
	- [For Android](#for-android)
- [XMLEncoder (XML)](#xmlencoder-xml)
- [XStream (XML/JSON/various)](#xstream-xmljsonvarious)
- [Kryo (binary)](#kryo-binary)
- [Hessian/Burlap (binary/XML)](#hessianburlap-binaryxml)
- [Castor (XML)](#castor-xml)
- [json-io (JSON)](#json-io-json)
- [Jackson (JSON)](#jackson-json)
- [Fastjson (JSON)](#fastjson-json)
- [Genson (JSON)](#genson-json)
- [Red5 IO AMF (AMF)](#red5-io-amf-amf)
- [Apache Flex BlazeDS (AMF)](#apache-flex-blazeds-amf)
- [Flamingo AMF  (AMF)](#flamingo-amf--amf)
- [GraniteDS  (AMF)](#graniteds--amf)
- [WebORB for Java  (AMF)](#weborb-for-java--amf)
- [SnakeYAML (YAML)](#snakeyaml-yaml)
- [jYAML (YAML)](#jyaml-yaml)
- [YamlBeans (YAML)](#yamlbeans-yaml)
- ["Safe" deserialization](#safe-deserialization)

## Java Native Serialization (binary)

### Overview
- [Java Deserialization Security FAQ](https://christian-schneider.net/JavaDeserializationSecurityFAQ.html)
- [From Foxgloves Security](http://foxglovesecurity.com/2015/11/06/what-do-weblogic-websphere-jboss-jenkins-opennms-and-your-application-have-in-common-this-vulnerability/)

### Main talks & presentations & docs
##### Marshalling Pickles
by [@frohoff](https://twitter.com/frohoff) & [@gebl](https://twitter.com/gebl)

- [Video](https://www.youtube.com/watch?v=KSA7vUkXGSg)
- [Slides](http://www.slideshare.net/frohoff1/appseccali-2015-marshalling-pickles)
- [Other stuff](http://frohoff.github.io/appseccali-marshalling-pickles/ )

##### Exploiting Deserialization Vulnerabilities in Java
by [@matthias_kaiser](https://twitter.com/matthias_kaiser)

- [Video](https://www.youtube.com/watch?v=VviY3O-euVQ)

##### Serial Killer: Silently Pwning Your Java Endpoints
by [@pwntester](http://twitter.com/pwntester) & [@cschneider4711](http://twitter.com/cschneider4711)

- [Slides](https://www.rsaconference.com/writable/presentations/file_upload/asd-f03-serial-killer-silently-pwning-your-java-endpoints.pdf)
- [White Paper](http://community.hpe.com/hpeb/attachments/hpeb/off-by-on-software-security-blog/722/1/HPE-SR%20whitepaper%20java%20deserialization%20RSA2016.pdf)
- [Bypass Gadget Collection](https://github.com/pwntester/SerialKillerBypassGadgetCollection)

##### Deserialize My Shorts: Or How I Learned To Start Worrying and Hate Java Object Deserialization
by [@frohoff](https://twitter.com/frohoff) & [@gebl](https://twitter.com/gebl)

- [Slides](http://www.slideshare.net/frohoff1/deserialize-my-shorts-or-how-i-learned-to-start-worrying-and-hate-java-object-deserialization)

##### Surviving the Java serialization apocalypse
by [@cschneider4711](http://twitter.com/cschneider4711) & [@pwntester](http://twitter.com/pwntester)

- [Slides](http://www.slideshare.net/cschneider4711/surviving-the-java-deserialization-apocalypse-owasp-appseceu-2016)
- [Video](https://www.youtube.com/watch?v=m1sH240pEfw)
- [PoC for Scala, Grovy](https://github.com/pwntester/JVMDeserialization)

##### Java Deserialization Vulnerabilities - The Forgotten Bug Class
by [@matthias_kaiser](https://twitter.com/matthias_kaiser)

- [Slides](http://www.slideshare.net/codewhitesec/java-deserialization-vulnerabilities-the-forgotten-bug-class)

##### Pwning Your Java Messaging With Deserialization Vulnerabilities
by [@matthias_kaiser](https://twitter.com/matthias_kaiser)

- [Slides](https://www.blackhat.com/docs/us-16/materials/us-16-Kaiser-Pwning-Your-Java-Messaging-With-Deserialization-Vulnerabilities.pdf)
- [White Paper](https://www.blackhat.com/docs/us-16/materials/us-16-Kaiser-Pwning-Your-Java-Messaging-With-Deserialization-Vulnerabilities-wp.pdf)
- [Tool for jms hacking](https://github.com/matthiaskaiser/jmet)

##### Defending against Java Deserialization Vulnerabilities
by [@lucacarettoni](https://twitter.com/lucacarettoni)

- [Slides](http://www.slideshare.net/ikkisoft/defending-against-java-deserialization-vulnerabilities)

##### A Journey From JNDI/LDAP Manipulation To Remote Code Execution Dream Land
by [@pwntester](http://twitter.com/pwntester) and O. Mirosh

- [Slides](https://www.blackhat.com/docs/us-16/materials/us-16-Munoz-A-Journey-From-JNDI-LDAP-Manipulation-To-RCE.pdf)
- [White Paper](https://www.blackhat.com/docs/us-16/materials/us-16-Munoz-A-Journey-From-JNDI-LDAP-Manipulation-To-RCE-wp.pdf)

##### Fixing the Java Serialization mess
by [@e_rnst](https://twitter.com/e_rnst)

- [Slides+Source](https://t.co/zsDnQBgw0Y)

##### Blind Java Deserialization
by deadcode.me

- [Part I - Commons Gadgets](https://deadcode.me/blog/2016/09/02/Blind-Java-Deserialization-Commons-Gadgets.html)
- [Part II - exploitation rev 2](https://deadcode.me/blog/2016/09/18/Blind-Java-Deserialization-Part-II.html)

##### An Overview of Deserialization Vulnerabilities in the Java Virtual Machine (JVM)
by [@joaomatosf](https://twitter.com/joaomatosf)

- [Slides](https://www.slideshare.net/joaomatosf_/an-overview-of-deserialization-vulnerabilities-in-the-java-virtual-machine-jvm-h2hc-2017)
- [Examples](https://github.com/joaomatosf/JavaDeserH2HC)

### Payload generators
##### ysoserial
[https://github.com/frohoff/ysoserial](https://github.com/frohoff/ysoserial)

ysoserial 0.6 payloads:

payload | author | dependencies | impact (if not RCE)
------|--------|------ |------
BeanShell1          |@pwntester, @cschneider4711 |bsh:2.0b5
C3P0                |@mbechler                   |c3p0:0.9.5.2, mchange-commons-java:0.2.11
Clojure             |@JackOfMostTrades           |clojure:1.8.0
CommonsBeanutils1   |@frohoff                    |commons-beanutils:1.9.2, commons-collections:3.1, commons-logging:1.2
CommonsCollections1 |@frohoff                    |commons-collections:3.1
CommonsCollections2 |@frohoff                    |commons-collections4:4.0
CommonsCollections3 |@frohoff                    |commons-collections:3.1
CommonsCollections4 |@frohoff                    |commons-collections4:4.0
CommonsCollections5 |@matthias_kaiser, @jasinner |commons-collections:3.1
CommonsCollections6 |@matthias_kaiser            |commons-collections:3.1
FileUpload1         |@mbechler                   |commons-fileupload:1.3.1, commons-io:2.4 | file uploading
Groovy1             |@frohoff                    |groovy:2.3.9
Hibernate1          |@mbechler|
Hibernate2          |@mbechler|
JBossInterceptors1  |@matthias_kaiser            |javassist:3.12.1.GA, jboss-interceptor-core:2.0.0.Final, cdi-api:1.0-SP1, javax.interceptor-api:3.1, jboss-interceptor-spi:2.0.0.Final, slf4j-api:1.7.21
JRMPClient          |@mbechler|
JRMPListener        |@mbechler|
JSON1               |@mbechler                   |json-lib:jar:jdk15:2.4, spring-aop:4.1.4.RELEASE, aopalliance:1.0, commons-logging:1.2, commons-lang:2.6, ezmorph:1.0.6, commons-beanutils:1.9.2, spring-core:4.1.4.RELEASE, commons-collections:3.1
JavassistWeld1      |@matthias_kaiser            |javassist:3.12.1.GA, weld-core:1.1.33.Final, cdi-api:1.0-SP1, javax.interceptor-api:3.1, jboss-interceptor-spi:2.0.0.Final, slf4j-api:1.7.21
Jdk7u21             |@frohoff|
Jython1             |@pwntester, @cschneider4711 |jython-standalone:2.5.2
MozillaRhino1       |@matthias_kaiser            |js:1.7R2
Myfaces1            |@mbechler|
Myfaces2            |@mbechler|
ROME                |@mbechler                   |rome:1.0
Spring1             |@frohoff                    |spring-core:4.1.4.RELEASE, spring-beans:4.1.4.RELEASE
Spring2             |@mbechler                   |spring-core:4.1.4.RELEASE, spring-aop:4.1.4.RELEASE, aopalliance:1.0, commons-logging:1.2
URLDNS              |@gebl| | jre only vuln detect
Wicket1             |@jacob-baines               |wicket-util:6.23.0, slf4j-api:1.6.4


Additional tools (detetection, integration ysoserial with Burp Suite):
- [Freddy](https://github.com/nccgroup/freddy)
- [JavaSerialKiller](https://github.com/NetSPI/JavaSerialKiller)
- [Java Deserialization Scanner](https://github.com/federicodotta/Java-Deserialization-Scanner)
- [Burp-ysoserial](https://github.com/summitt/burp-ysoserial)
- [SuperSerial](https://github.com/DirectDefense/SuperSerial)
- [SuperSerial-Active](https://github.com/DirectDefense/SuperSerial-Active)

Additional tool to test RMI:
- [BaRMIe](https://github.com/NickstaDB/BaRMIe)

Full shell (pipes, redirects and other stuff):
- [$@|sh – Or: Getting a shell environment from Runtime.exec](http://codewhitesec.blogspot.ru/2015/03/sh-or-getting-shell-environment-from.html)
- Set String[] for Runtime.exec (patch ysoserial's payloads)
- [Shell Commands Converter](http://jackson.thuraisamy.me/runtime-exec-payloads.html)

How it works:
- [https://blog.srcclr.com/commons-collections-deserialization-vulnerability-research-findings/](https://blog.srcclr.com/commons-collections-deserialization-vulnerability-research-findings/)
- [http://gursevkalra.blogspot.ro/2016/01/ysoserial-commonscollections1-exploit.html](http://gursevkalra.blogspot.ro/2016/01/ysoserial-commonscollections1-exploit.html)

##### JRE8u20_RCE_Gadget
[https://github.com/pwntester/JRE8u20_RCE_Gadget](https://github.com/pwntester/JRE8u20_RCE_Gadget)

Pure JRE 8 RCE Deserialization gadget

##### ACEDcup
[https://github.com/GrrrDog/ACEDcup](https://github.com/GrrrDog/ACEDcup)

File uploading via:
- Apache Commons FileUpload <= 1.3 (CVE-2013-2186) and Oracle JDK < 7u40

##### Universal billion-laughs DoS
[https://gist.github.com/coekie/a27cc406fc9f3dc7a70d](https://gist.github.com/coekie/a27cc406fc9f3dc7a70d)

Won't fix DoS via default Java classes (JRE)

##### Universal Heap overflows DoS using Arrays and HashMaps
[https://github.com/topolik/ois-dos/](https://github.com/topolik/ois-dos/)

How it works:
- [Java Deserialization DoS - payloads](http://topolik-at-work.blogspot.ru/2016/04/java-deserialization-dos-payloads.html)

Won't fix DoS using default Java classes (JRE)

##### DoS against Serialization Filtering (JEP-290)
- [CVE-2018-2677](https://www.waratek.com/waratek-identifies-two-new-deserialization-vulnerabilities-cve-2018-2677/)


##### Tool to search gadgets
- [Gadget Inspector](https://github.com/JackOfMostTrades/gadgetinspector)

### Exploits

no spec tool - You don't need a special tool (just Burp/ZAP + payload)

##### RMI
- *Protocol*
- *Default - 1099/tcp for rmiregistry*

[ysoserial](#ysoserial) (works only against a RMI registry service)

##### JMX
- *Protocol based on RMI*
- + [CVE-2016-3427](http://engineering.pivotal.io/post/java-deserialization-jmx/)
- partially patched in JRE

[ysoserial](#ysoserial)

[JexBoss](https://github.com/joaomatosf/jexboss)

##### JNDI/LDAP
- When we control an address for lookup of JNDI (context.lookup(address) and can have backconnect from a server
- [Full info](#a-journey-from-jndildap-manipulation-to-remote-code-execution-dream-land)
- [JNDI remote code injection](http://zerothoughts.tumblr.com/post/137769010389/fun-with-jndi-remote-code-injection)

[https://github.com/zerothoughts/jndipoc](https://github.com/zerothoughts/jndipoc)

##### JMS
- [Full info](#pwning-your-java-messaging-with-deserialization-vulnerabilities)

[JMET](https://github.com/matthiaskaiser/jmet)

##### JSF ViewState
- if no encryption or good mac

no spec tool

[JexBoss](https://github.com/joaomatosf/jexboss)

##### T3 of Oracle Weblogic
- *Protocol*
- *Default - 7001/tcp on localhost interface*
- [CVE-2015-4852](https://www.vulners.com/search?query=CVE-2015-4852)
- [Blacklist bypass - CVE-2017-3248](https://www.tenable.com/security/research/tra-2017-07)
- [Blacklist bypass - CVE-2017-3248 PoC](https://github.com/quentinhardy/scriptsAndExploits/blob/master/exploits/weblogic/exploit-CVE-2017-3248-bobsecq.py)
- [Blacklist bypass - CVE-2018-2628](https://github.com/brianwrf/CVE-2018-2628)
- [Blacklist bypass - CVE-2018-3245](https://blogs.projectmoon.pw/2018/10/19/Oracle-WebLogic-Two-RCE-Deserialization-Vulnerabilities/)
- [Blacklist bypass - CVE-2018-3191](https://mp.weixin.qq.com/s/ebKHjpbQcszAy_vPocW0Sg)

[loubia](https://github.com/metalnas/loubia) (tested on 11g and 12c, supports t3s)

[JavaUnserializeExploits](https://github.com/foxglovesec/JavaUnserializeExploits) (doesn't work for all Weblogic versions)

[WLT3Serial](https://github.com/Bort-Millipede/WLT3Serial) 

[CVE-2018-2628 sploit](https://github.com/brianwrf/CVE-2018-2628)

##### Oracle Weblogic
- auth required
- [How it works](https://blogs.projectmoon.pw/2018/10/19/Oracle-WebLogic-Two-RCE-Deserialization-Vulnerabilities/)
- [CVE-2018-3252](https://www.vulners.com/search?query=CVE-2018-3252)

##### IBM Websphere (1)
- *wsadmin*
- *Default port - 8880/tcp*
- [CVE-2015-7450](https://www.vulners.com/search?query=CVE-2015-7450)

[JavaUnserializeExploits](https://github.com/foxglovesec/JavaUnserializeExploits)

[serialator](https://github.com/roo7break/serialator)

[CoalfireLabs/java_deserialization_exploits](https://github.com/Coalfire-Research/java-deserialization-exploits/tree/master/WebSphere)

##### IBM Websphere (2)
- When using custom form authentication
- WASPostParam cookie
- [Full info](https://lab.mediaservice.net/advisory/2016-02-websphere.txt)

no spec tool

##### Red Hat JBoss (1)
- *http://jboss_server/invoker/JMXInvokerServlet*
- *Default port - 8080/tcp*
- [CVE-2015-7501](https://www.vulners.com/search?query=CVE-2015-7501)

[JavaUnserializeExploits](https://github.com/foxglovesec/JavaUnserializeExploits)

[https://github.com/njfox/Java-Deserialization-Exploit](https://github.com/njfox/Java-Deserialization-Exploit)

[serialator](https://github.com/roo7break/serialator)

[JexBoss](https://github.com/joaomatosf/jexboss)

##### Red Hat JBoss 6.X
- *http://jboss_server/invoker/readonly*
- *Default port - 8080/tcp*
- [CVE-2017-12149](https://www.vulners.com/search?query=CVE-2017-12149)
- JBoss 6.X and EAP 5.X 
- [Details](https://github.com/joaomatosf/JavaDeserH2HC)

no spec tool

##### Red Hat JBoss 4.x
- *http://jboss_server/jbossmq-httpil/HTTPServerILServlet/*
- <= 4.x
- [CVE-2017-7504](https://www.vulners.com/search?query=CVE-2017-7504)

no spec tool

##### Jenkins (1)
- *Jenkins CLI*
- *Default port - High number/tcp*
- [CVE-2015-8103](https://www.vulners.com/search?query=CVE-2015-8103)
- [CVE-2015-3253](https://www.vulners.com/search?query=CVE-2015-3253)

[JavaUnserializeExploits](https://github.com/foxglovesec/JavaUnserializeExploits)

[JexBoss](https://github.com/joaomatosf/jexboss)

##### Jenkins (2)
- patch "bypass" for [Jenkins](#jenkins)
- [CVE-2016-0788](https://www.vulners.com/search?query=CVE-2016-0788)
- [Details of exploit](https://www.insinuator.net/2016/07/jenkins-remoting-rce-ii-the-return-of-the-ysoserial/)

[ysoserial](#ysoserial)

##### Jenkins (s)
- *Jenkins CLI LDAP*
- *Default port - High number/tcp
- <= 2.32
- <= 2.19.3 (LTS)
- [CVE-2016-9299](https://www.vulners.com/search?query=CVE-2016-9299)

##### CloudBees Jenkins
- <= 2.32.1
- [CVE-2017-1000353](https://www.vulners.com/search?query=CVE-2017-1000353)
- [Details](https://blogs.securiteam.com/index.php/archives/3171)

[Sploit](https://blogs.securiteam.com/index.php/archives/3171)

##### Restlet
- *<= 2.1.2*
- *When Rest API accepts serialized objects (uses ObjectRepresentation)*

no spec tool

##### RESTEasy
- *When Rest API accepts serialized objects (uses @Consumes({"\*/\*"}) or "application/\*" )
- [Details and examples](https://0ang3el.blogspot.ru/2016/06/note-about-security-of-resteasy-services.html)

no spec tool

##### OpenNMS
- RMI

[ysoserial](#ysoserial)

##### Progress OpenEdge RDBMS
- all versions
- RMI

[ysoserial](#ysoserial)

#####  Commvault Edge Server
- [CVE-2015-7253](https://www.vulners.com/search?query=CVE-2015-7253)
- Serialized object in cookie

no spec tool

##### Symantec Endpoint Protection Manager
- */servlet/ConsoleServlet?ActionType=SendStatPing*
- [CVE-2015-6555](https://www.vulners.com/search?query=CVE-2015-6555)

[serialator](https://github.com/roo7break/serialator)

##### Oracle MySQL Enterprise Monitor
- *https://[target]:18443/v3/dataflow/0/0*
- [CVE-2016-3461](http://www.tenable.com/security/research/tra-2016-11)

no spec tool

[serialator](https://github.com/roo7break/serialator)

##### PowerFolder Business Enterprise Suite
- custom(?) protocol (1337/tcp)
- [MSA-2016-01](http://lab.mogwaisecurity.de/advisories/MSA-2016-01/)

[powerfolder-exploit-poc](https://github.com/h0ng10/powerfolder-exploit-poc)

##### Solarwinds Virtualization Manager
- <= 6.3.1
- RMI
- [CVE-2016-3642](https://www.vulners.com/search?query=CVE-2016-3642)

[ysoserial](#ysoserial)

##### Cisco Prime Infrastructure
- *https://[target]/xmp_data_handler_service/xmpDataOperationRequestServlet*
- <= 2.2.3 Update 4
- <= 3.0.2
- [CVE-2016-1291](https://www.vulners.com/search?query=CVE-2016-1291)

[CoalfireLabs/java_deserialization_exploits](https://github.com/Coalfire-Research/java-deserialization-exploits/tree/master/CiscoPrime)

##### Cisco ACS
- <= 5.8.0.32.2
- RMI (2020 tcp)
- [CSCux34781](https://quickview.cloudapps.cisco.com/quickview/bug/CSCux34781)

[ysoserial](#ysoserial)

##### Apache XML-RPC
- all version, no fix (the project is not supported)
- POST XML request with <ex:serializable> element
- [Details and examples](https://0ang3el.blogspot.ru/2016/07/beware-of-ws-xmlrpc-library-in-your.html)

no spec tool

##### Apache Archiva
- because it uses [Apache XML-RPC](#apache-xml-rpc)
- [CVE-2016-5004](https://www.vulners.com/search?query=CVE-2016-5004)
- [Details and examples](https://0ang3el.blogspot.ru/2016/07/beware-of-ws-xmlrpc-library-in-your.html)

no spec tool

##### SAP NetWeaver
- *https://[target]/developmentserver/metadatauploader*
- [CVE-2017-9844](https://erpscan.com/advisories/erpscan-17-014-sap-netweaver-java-deserialization-untrusted-user-value-metadatauploader/)

[PoC](https://github.com/vah13/SAP_vulnerabilities/tree/5995daf7bac2e01a63dc57dcf5bbab70489bf6bb/CVE-2017-9844)

#####  Sun Java Web Console
- admin panel for Solaris
- < v3.1.
- [old DoS sploit](https://www.ikkisoft.com/stuff/SJWC_DoS.java)

no spec tool

##### Apache MyFaces Trinidad
- 1.0.0 <= version < 1.0.13
- 1.2.1 <= version < 1.2.14
- 2.0.0 <= version < 2.0.1
- 2.1.0 <= version < 2.1.1
- it does not check MAC
- [CVE-2016-5019](https://www.vulners.com/search?query=CVE-2016-5019)

no spec tool

##### Apache Tomcat JMX
- JMX
- [Patch bypass](http://seclists.org/oss-sec/2016/q4/502)
- [CVE-2016-8735](https://www.vulners.com/search?query=CVE-2016-8735)

[JexBoss](https://github.com/joaomatosf/jexboss)

##### OpenText Documentum D2
- *version 4.x*
- [CVE-2017-5586](https://www.vulners.com/search?query=CVE-2017-5586)

[exploit](https://www.exploit-db.com/exploits/41366/)

##### Liferay
- */api/spring*
- */api/liferay*
- <= 7.0-ga3
- if IP check works incorrectly
- [Details](https://www.tenable.com/security/research/tra-2017-01)

no spec tool

##### ScrumWorks Pro
- */UFC*
- <= 6.7.0
- [Details](https://blogs.securiteam.com/index.php/archives/3387)

[PoC](https://blogs.securiteam.com/index.php/archives/3387)

##### ManageEngine Applications Manager
- version: 12, 13
- RMI
- [CVE-2016-9498](https://www.vulners.com/search?query=CVE-2016-9498)

[ysoserial](#ysoserial)

##### Apache Shiro
- [SHIRO-550](https://issues.apache.org/jira/browse/SHIRO-550)
- encrypted cookie (with the hardcoded key)
- [Exploitation (in Chinese)](http://blog.knownsec.com/2016/08/apache-shiro-java/)

##### Apache ActiveMQ - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### Redhat/Apache HornetQ - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### Oracle OpenMQ - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### IBM WebSphereMQ - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### Oracle Weblogic - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### Pivotal RabbitMQ - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### IBM MessageSight - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### IIT Software SwiftMQ - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### Apache ActiveMQ Artemis - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### Apache QPID JMS - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### Apache QPID - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### Amazon SQS Java Messaging - Client lib
- [JMS](#jms)

[JMET](https://github.com/matthiaskaiser/jmet)

##### Axis/Axis2 SOAPMonitor
- All version (this was deemed by design by project maintainer)
- Binary
- Default port : 5001
- Info : https://axis.apache.org/axis2/java/core/docs/soapmonitor-module.html

> java -jar ysoserial-*-all.jar CommonsCollections1  'COMMAND_HERE' | nc TARGET_SERVER 5001

[ysoserial](#ysoserial)

##### Apache Synapse
- <= 3.0.1
- RMI 
- [Exploit](https://github.com/iBearcat/CVE-2017-15708)

[ysoserial](#ysoserial)

##### Apache Jmeter
- <= 3.0.1
- RMI 
- When using Distributed Test only 
- [Exploit](https://github.com/iBearcat/CVE-2018-1297)

[ysoserial](#ysoserial)

##### Jolokia
- <= 1.4.0
- JNDI injection 
- /jolokia/ 
- [Exploit](https://blog.gdssecurity.com/labs/2018/4/18/jolokia-vulnerabilities-rce-xss.html)

##### RichFaces
- all versions
- [Poor RichFaces](https://codewhitesec.blogspot.com/2018/05/poor-richfaces.html)
- [When EL Injection meets Java Deserialization](https://tint0.com/when-el-injection-meets-java-deserialization/)
 
##### Apache James 
- < 3.0.1 
- [Analysis of CVE-2017-12628](https://nickbloor.co.uk/2017/10/22/analysis-of-cve-2017-12628/)
 
[ysoserial](#ysoserial)
 
### Detect
##### Code review
- *ObjectInputStream.readObject*
- *ObjectInputStream.readUnshared*
- Tool: [Find Security Bugs](http://find-sec-bugs.github.io/)
- Tool: [Serianalyzer](https://github.com/mbechler/serianalyzer)

##### Traffic
- *Magic bytes 'ac ed 00 05' bytes*
- *'rO0' for Base64*
- *'application/x-java-serialized-object' for Content-Type header*

##### Network
- Nmap >=7.10 has more java-related probes
- use nmap --all-version to find JMX/RMI on non-standart ports

##### Burp plugins
- [JavaSerialKiller](https://github.com/NetSPI/JavaSerialKiller)
- [Java Deserialization Scanner](https://github.com/federicodotta/Java-Deserialization-Scanner)
- [Burp-ysoserial](https://github.com/summitt/burp-ysoserial)
- [SuperSerial](https://github.com/DirectDefense/SuperSerial)
- [SuperSerial-Active](https://github.com/DirectDefense/SuperSerial-Active)
- [Freddy](https://github.com/nccgroup/freddy)

### Vulnerable apps (without public sploits/need more info)

##### Spring Service Invokers (HTTP, JMS, RMI...)
- [Details](https://www.tenable.com/security/research/tra-2016-20)

##### SAP P4
- [info from slides](#java-deserialization-vulnerabilities---the-forgotten-bug-class)

##### Apache SOLR
- [SOLR-8262](https://issues.apache.org/jira/browse/SOLR-8262)
- 5.1 <= version <=5.4
- /stream handler uses Java serialization for RPC

##### Apache ActiveMQ (2)
- [*CVE-2015-5254*](http://activemq.apache.org/security-advisories.data/CVE-2015-5254-announcement.txt)
- *<= 5.12.1*
- [*Explanation of the vuln*](https://srcclr.com/security/deserialization-untrusted-data/java/s-1893)
- [CVE-2015-7253](https://www.vulners.com/search?query=CVE-2015-7253)

##### Atlassian Bamboo (1)
- [CVE-2015-6576](https://confluence.atlassian.com/x/Hw7RLg)
-  *2.2 <= version < 5.8.5*
- *5.9.0 <= version < 5.9.7*

##### Atlassian Bamboo (2)
- [*CVE-2015-8360*](https://confluence.atlassian.com/bamboo/bamboo-security-advisory-2016-01-20-794376535.html)
- *2.3.1 <= version < 5.9.9*
- Bamboo JMS port (port 54663 by default)

##### Atlassian Jira
- only Jira with a Data Center license
- RMI (port 40001 by default)
- [*JRA-46203*](https://jira.atlassian.com/browse/JRA-46203)

##### Akka
- *version < 2.4.17*
- "an ActorSystem exposed via Akka Remote over TCP"
- [Official description](http://doc.akka.io/docs/akka/2.4/security/2017-02-10-java-serialization.html)

##### Spring AMPQ
- [CVE-2016-2173](http://pivotal.io/security/cve-2016-2173)
- *1.0.0 <= version < 1.5.5*

##### Apache Tika
- [CVE-2016-6809](https://lists.apache.org/thread.html/93618b15cdf3b38fa1f0bfc0c8c7cf384607e552935bd3db2e322e07@%3Cdev.tika.apache.org%3E)
- *1.6 <= version < 1.14*
- Apache Tika’s MATLAB Parser

##### Apache HBase
- [HBASE-14799](https://issues.apache.org/jira/browse/HBASE-14799)

##### Apache Camel
- [CVE-2015-5348](https://www.vulners.com/search?query=CVE-2015-5348)

##### Apache Log4j
- as server
- [CVE-2017-5645](https://vulners.com/search?query=CVE-2017-5645)

##### Gradle (gui)
- custom(?) protocol(60024/tcp)
- [article](http://philwantsfish.github.io/security/java-deserialization-github)

##### Oracle Hyperion
- [from slides](#java-deserialization-vulnerabilities---the-forgotten-bug-class)

##### Oracle Application Testing Suite
- [CVE-2015-7501](http://www.tenable.com/plugins/index.php?view=single&id=90859)

##### Red Hat JBoss BPM Suite
- [RHSA-2016-0539](http://rhn.redhat.com/errata/RHSA-2016-0539.html)
- [CVE-2016-2510](https://www.vulners.com/search?query=CVE-2016-2510)


##### VMWare vRealize Operations
- 6.0 <= version < 6.4.0
- REST API
- [VMSA-2016-0020](http://www.vmware.com/security/advisories/VMSA-2016-0020.html)
- [CVE-2016-7462](https://www.vulners.com/search?query=CVE-2016-7462)

##### VMWare vCenter/vRealize (various)
- [CVE-2015-6934](https://www.vulners.com/search?query=CVE-2015-6934)
- [VMSA-2016-0005](http://www.vmware.com/security/advisories/VMSA-2016-0005.html)
- JMX

##### Cisco (various)
- [List of vulnerable products](https://tools.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20151209-java-deserialization)
- [CVE-2015-6420](https://www.vulners.com/search?query=CVE-2015-6420)

##### Lexmark Markvision Enterprise
- [CVE-2016-1487](http://support.lexmark.com/index?page=content&id=TE747&locale=en&userlocale=EN_US)

#####  McAfee ePolicy Orchestrator
- [CVE-2015-8765](https://www.vulners.com/search?query=CVE-2015-8765)

#####  HP iMC
- [CVE-2016-4372](https://www.vulners.com/search?query=CVE-2016-4372)

#####  HP Operations Orchestration
- [CVE-2016-1997](https://www.vulners.com/search?query=CVE-2016-1997)

#####  HP Asset Manager
- [CVE-2016-2000](https://www.vulners.com/search?query=CVE-2016-2000)

##### HP Service Manager
- [CVE-2016-1998](https://h20564.www2.hpe.com/hpsc/doc/public/display?docId=emr_na-c05054565)

##### HP Operations Manager
- [CVE-2016-1985](https://h20565.www2.hpe.com/hpsc/doc/public/display?calledBy=Search_Result&docId=emr_na-c04953244&docLocale=en_US)

##### HP Release Control
- [CVE-2016-1999](https://h20565.www2.hpe.com/hpsc/doc/public/display?calledBy=Search_Result&docId=emr_na-c05063986&docLocale=en_US)

##### HP Continuous Delivery Automation
- [CVE-2016-1986](https://h20565.www2.hpe.com/hpsc/doc/public/display?calledBy=Search_Result&docId=emr_na-c04958567&docLocale=en_US)

##### HP P9000, XP7 Command View Advanced Edition (CVAE) Suite
- [CVE-2016-2003](https://h20564.www2.hpe.com/hpsc/doc/public/display?docId=emr_na-c05085438)

##### HP Network Automation
- [CVE-2016-4385](https://www.vulners.com/search?query=CVE-2016-4385)

##### Adobe Experience Manager
- [CVE-2016-0958](https://www.vulners.com/search?query=CVE-2016-0958)

#####  Unify OpenScape (various)
- [CVE-2015-8237](https://www.vulners.com/search?query=CVE-2015-8237)
- RMI (30xx/tcp)
- [CVE-2015-8238](https://www.vulners.com/search?query=CVE-2015-8238)
- js-soc protocol (4711/tcp)

##### Apache OFBiz
- [CVE-2016-2170](https://blogs.apache.org/ofbiz/date/20160405)

##### Apache Tomcat
- requires local access
- [CVE-2016-0714](https://www.vulners.com/search?query=CVE-2016-0714)
- [Article](http://engineering.pivotal.io/post/java-deserialization-jmx/)

##### Apache TomEE
- [CVE-2015-8581](https://www.vulners.com/search?query=CVE-2015-8581)
- [CVE-2016-0779](https://www.vulners.com/search?query=CVE-2016-0779)

##### IBM Congnos BI
- [CVE-2012-4858](https://www.vulners.com/search?query=CVE-2012-4858)

##### Novell NetIQ Sentinel
- [?](https://www.tenable.com/plugins/index.php?view=single&id=90602)

##### ForgeRock OpenAM
- *9-9.5.5, 10.0.0-10.0.2, 10.1.0-Xpress, 11.0.0-11.0.3 and 12.0.0*
- [201505-01](https://forgerock.org/2015/07/openam-security-advisory-201505/)

##### F5 (various)
- [sol30518307](https://support.f5.com/kb/en-us/solutions/public/k/30/sol30518307.html)

##### Hitachi (various)
- [HS16-010](http://www.hitachi.co.jp/Prod/comp/soft1/global/security/info/vuls/HS16-010/index.html)
- [0328_acc](http://www.hitachi.co.jp/products/it/storage-solutions/global/sec_info/2016/0328_acc.html)

##### NetApp (various)
- [CVE-2015-8545](https://kb.netapp.com/support/index?page=content&id=9010052)

##### Zimbra Collaboration
- < 8.7.0
- [CVE-2016-3415](https://www.vulners.com/search?query=CVE-2016-3415)

##### Adobe ColdFusion (1)
- <= 2016 Update 4
- <= 11 update 12
- [CVE-2017-11283](https://www.vulners.com/search?query=CVE-2017-11283)
- [CVE-2017-11284](https://www.vulners.com/search?query=CVE-2017-11284)

##### Adobe ColdFusion (2)
- RMI
- <= 2016 Update 5
- <= 11 update 13
- [Another ColdFusion RCE – CVE-2018-4939](https://nickbloor.co.uk/2018/06/18/another-coldfusion-rce-cve-2018-4939/)
- [CVE-2018-4939](https://www.vulners.com/search?query=CVE-2018-4939)

##### Code42 CrashPlan
- *TCP port 4282*
- RMI (?)
- 5.4.x
- [CVE-2017-9830](https://www.vulners.com/search?query=CVE-2017-9830)
- [Details](https://blog.radicallyopensecurity.com/CVE-2017-9830.html)

##### Apache OpenJPA
- [CVE-2013-1768](http://seclists.org/fulldisclosure/2013/Jun/98)

##### Apache Batchee
##### Apache JCS
##### Apache OpenWebBeans

### Protection
- [Look-ahead Java deserialization](http://www.ibm.com/developerworks/library/se-lookahead/ )
- [NotSoSerial](https://github.com/kantega/notsoserial)
- [SerialKiller](https://github.com/ikkisoft/SerialKiller)
- [ValidatingObjectInputStream](https://issues.apache.org/jira/browse/IO-487)
- [Name Space Layout Randomization](http://www.waratek.com/warateks-name-space-layout-randomization-nslr/)
- [Some protection bypasses](https://github.com/GrrrDog/Java-Deserialization-Cheat-Sheet/blob/master/README.md#serial-killer-silently-pwning-your-java-endpoints)
- Tool: [Serial Whitelist Application Trainer](https://github.com/cschneider4711/SWAT)
- [JEP 290: Filter Incoming Serialization Data](http://openjdk.java.net/jeps/290) in JDK 6u141, 7u131, 8u121
  - [A First Look Into Java's New Serialization Filtering](https://dzone.com/articles/a-first-look-into-javas-new-serialization-filterin)

### For Android
#### Main talks & presentations & docs
- [One Class to Rule Them All: 0-Day Deserialization Vulnerabilities in Android](https://www.usenix.org/conference/woot15/workshop-program/presentation/peles)
- [Android Serialization Vulnerabilities Revisited](https://www.rsaconference.com/events/us16/agenda/sessions/2455/android-serialization-vulnerabilities-revisited)
- [A brief history of Android deserialization vulnerabilities](https://lgtm.com/blog/android_deserialization)

#### Tools
- [Android Java Deserialization Vulnerability Tester](https://github.com/modzero/modjoda)

## XMLEncoder (XML)
How it works:

- [http://blog.diniscruz.com/2013/08/using-xmldecoder-to-execute-server-side.html](http://blog.diniscruz.com/2013/08/using-xmldecoder-to-execute-server-side.html)
- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)

### Detect
##### Code review
- java.beans.XMLDecoder
- readObject
 
##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

### Exploits
##### Oracle Weblogic
- <= 10.3.6.0.0
- <= 12.1.3.0.0
- <= 12.2.1.2.0
- <= 12.2.1.1.0
- *http://weblogic_server/wls-wsat/CoordinatorPortType*
- [CVE-2017-3506](https://www.vulners.com/search?query=CVE-2017-3506)
- [CVE-2017-10271](https://www.vulners.com/search?query=CVE-2017-10271)
- [Details](https://blog.nsfocusglobal.com/threats/vulnerability-analysis/technical-analysis-and-solution-of-weblogic-server-wls-component-vulnerability/)

[Exploit](https://github.com/1337g/CVE-2017-10271/blob/master/CVE-2017-10271.py)

##### Oracle RDBMS
- priv escalation
- [Oracle Privilege Escalation via Deserialization](http://obtruse.syfrtext.com/2018/07/oracle-privilege-escalation-via.html)

## XStream (XML/JSON/various)
How it works:

- [http://www.pwntester.com/blog/2013/12/23/rce-via-xstream-object-deserialization38/](http://www.pwntester.com/blog/2013/12/23/rce-via-xstream-object-deserialization38/)
- [http://blog.diniscruz.com/2013/12/xstream-remote-code-execution-exploit.html](http://blog.diniscruz.com/2013/12/xstream-remote-code-execution-exploit.html)
- [https://www.contrastsecurity.com/security-influencers/serialization-must-die-act-2-xstream](https://www.contrastsecurity.com/security-influencers/serialization-must-die-act-2-xstream)
- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)

### Payload generators

- [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)

### Exploits
##### Apache Struts (S2-052)
- <= 2.3.34
- <= 2.5.13
- REST plugin
- [CVE-2017-9805](https://www.vulners.com/search?query=CVE-2017-9805)

[Exploit](https://www.exploit-db.com/exploits/42627/)

### Detect
##### Code review
- com.thoughtworks.xstream.XStream
- xs.fromXML(data)

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

### Vulnerable apps (without public sploits/need more info):
##### Atlassian Bamboo
- [CVE-2016-5229](https://www.vulners.com/search?query=CVE-2016-5229)

##### Jenkins
- [CVE-2017-2608](https://www.vulners.com/search?query=CVE-2017-2608)

## Kryo (binary)

How it works:

- [https://www.contrastsecurity.com/security-influencers/serialization-must-die-act-1-kryo](https://www.contrastsecurity.com/security-influencers/serialization-must-die-act-1-kryo)
- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)

### Payload generators

- [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)

### Detect
##### Code review
- com.esotericsoftware.kryo.io.Input
- SomeClass object = (SomeClass)kryo.readClassAndObject(input);
- SomeClass someObject = kryo.readObjectOrNull(input, SomeClass.class);
- SomeClass someObject = kryo.readObject(input, SomeClass.class);

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

## Hessian/Burlap (binary/XML)
How it works:

- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)

### Payload generators

- [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)

### Detect
##### Code review
- com.caucho.hessian.io
- AbstractHessianInput
- com.caucho.burlap.io.BurlapInput;
- com.caucho.burlap.io.BurlapOutput;
- BurlapInput in = new BurlapInput(is);
- Person2 p1 = (Person2) in.readObject();

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

## Castor (XML)
How it works:

- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)

### Payload generators

- [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)

### Detect
##### Code review
- org.codehaus.castor
- org.exolab.castor.xml.Unmarshaller 
- org.springframework.oxm.Unmarshaller
- Unmarshaller.unmarshal(Person.class, reader)
- unmarshaller = context.createUnmarshaller();
- unmarshaller.unmarshal(new StringReader(data));

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

### Vulnerable apps (without public sploits/need more info):

##### OpenNMS
- [NMS-9100](https://issues.opennms.org/browse/NMS-9100)

## json-io (JSON)
How it works:

- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)

Exploitation examples:

- [Experiments with JSON-IO, Serialization, Mass Assignment, and General Java Object Wizardry](https://versprite.com/blog/application-security/experiments-with-json-io-serialization-mass-assignment-and-general-java-object-wizardry/)
- [JSON Deserialization Memory Corruption Vulnerabilities on Android](https://versprite.com/blog/json-deserialization-memory-corruption-vulnerabilities/)

### Payload generators

- [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)

### Detect
##### Code review
- com.cedarsoftware.util.io.JsonReader
- JsonReader.jsonToJava

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

## Jackson (JSON)
*vulnerable in specific configuration*

How it works:

- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)
- [On Jackson CVEs: Don’t Panic — Here is what you need to know](https://medium.com/@cowtowncoder/on-jackson-cves-dont-panic-here-is-what-you-need-to-know-54cd0d6e8062)
- [Jackson Deserialization Vulnerabilities](https://www.nccgroup.trust/globalassets/our-research/us/whitepapers/2018/jackson_deserialization.pdf)

### Payload generators

- [https://adamcaudill.com/2017/10/04/exploiting-jackson-rce-cve-2017-7525/](https://adamcaudill.com/2017/10/04/exploiting-jackson-rce-cve-2017-7525/)
- [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)
- [blacklist bypass - CVE-2017-17485](https://github.com/irsl/jackson-rce-via-spel)
- [blacklist bypass - CVE-2017-15095](https://github.com/SecureSkyTechnology/study-struts2-s2-054_055-jackson-cve-2017-7525_cve-2017-15095) 

### Detect
##### Code review
- com.fasterxml.jackson.databind.ObjectMapper
- ObjectMapper mapper = new ObjectMapper();  
- objectMapper.enableDefaultTyping();
- @JsonTypeInfo(use=JsonTypeInfo.Id.CLASS, include=JsonTypeInfo.As.PROPERTY, property="@class") 
- public Object message; 
- mapper.readValue(data, Object.class); 

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

### Vulnerable apps (without public sploits/need more info):
##### Apache Camel
- [CVE-2016-8749](https://www.vulners.com/search?query=CVE-2016-8749)

## Fastjson (JSON)

How it works (in Chinese):

- [https://www.secfree.com/article-590.html](https://www.secfree.com/article-590.html) 
- [Official advisory](https://github.com/alibaba/fastjson/wiki/security_update_20170315)

### Detect
##### Code review
- com.alibaba.fastjson.JSON
- JSON.parseObject

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

### Payload generators

- [https://github.com/iBearcat/Fastjson-Payload](https://github.com/iBearcat/Fastjson-Payload)

## Genson (JSON)

How it works:

- [Friday the 13th JSON Attacks](https://www.blackhat.com/docs/us-17/thursday/us-17-Munoz-Friday-The-13th-JSON-Attacks-wp.pdf) 

### Detect
##### Code review
- com.owlike.genson.Genson
- useRuntimeType
- genson.deserialize

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

## Red5 IO AMF (AMF)
How it works:

- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)

### Payload generators

- [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)

### Detect
##### Code review
- org.red5.io
- Deserializer.deserialize(i, Object.class);

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

### Vulnerable apps (without public sploits/need more info):
##### Apache OpenMeetings
- [CVE-2017-5878](https://www.vulners.com/search?query=CVE-2017-5878)

## Apache Flex BlazeDS (AMF)
How it works:

- [AMF – Another Malicious Format](http://codewhitesec.blogspot.ru/2017/04/amf.html)
- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)

### Payload generators

- [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)

### Detect
##### Code review

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

### Vulnerable apps (without public sploits/need more info):

##### Adobe ColdFusion
- [CVE-2017-3066](https://www.vulners.com/search?query=CVE-2017-3066)
- *<= 2016 Update 3*
- *<= 11 update 11*
- *<= 10 Update 22*

- [Exploiting Adobe ColdFusion before CVE-2017-3066](http://codewhitesec.blogspot.ru/2018/03/exploiting-adobe-coldfusion.html)
- [PoC](https://github.com/depthsecurity/coldfusion_blazeds_des)

##### Apache BlazeDS
- [CVE-2017-5641](https://www.vulners.com/search?query=CVE-2017-5641)

##### VMWare VCenter
- [CVE-2017-5641](https://www.vulners.com/search?query=CVE-2017-5641)

## Flamingo AMF  (AMF)
How it works:

- [AMF – Another Malicious Format](http://codewhitesec.blogspot.ru/2017/04/amf.html)

### Detect
##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

## GraniteDS  (AMF)
How it works:

- [AMF – Another Malicious Format](http://codewhitesec.blogspot.ru/2017/04/amf.html)

### Detect
##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

## WebORB for Java  (AMF)
How it works:

- [AMF – Another Malicious Format](http://codewhitesec.blogspot.ru/2017/04/amf.html)

### Detect
##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

## SnakeYAML (YAML)
How it works:

- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)

### Payload generators

- [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)

### Detect
##### Code review
- org.yaml.snakeyaml.Yaml
- yaml.load

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

### Vulnerable apps (without public sploits/need more info):
##### Resteasy
- [CVE-2016-9606](https://www.vulners.com/search?query=CVE-2016-9606)

##### Apache Camel
- [CVE-2017-3159](https://www.vulners.com/search?query=CVE-2017-3159)

##### Apache Brooklyn
- [CVE-2016-8744](https://www.vulners.com/search?query=CVE-CVE-2016-8744)

## jYAML (YAML)
How it works:

- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)

### Payload generators

- [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)

### Detect
- org.ho.yaml.Yaml
- Yaml.loadType(data, Object.class);

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

## YamlBeans (YAML)
How it works:

- [Java Unmarshaller Security](https://www.github.com/mbechler/marshalsec/blob/master/marshalsec.pdf)

### Payload generators

- [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)

### Detect
- com.esotericsoftware.yamlbeans
- YamlReader r = new YamlReader(data, yc);

##### Burp plugins
- [Freddy](https://github.com/nccgroup/freddy)

## "Safe" deserialization

Some serialization libs are safe (or almost safe) [https://github.com/mbechler/marshalsec](https://github.com/mbechler/marshalsec)

However, it's not a recommendation, but just a list of other libs that has been researched by someone:

- JAXB
- XmlBeans
- Jibx
- Protobuf
- GSON
- GWT-RPC
