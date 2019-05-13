# Zabbix_wildfly_eap_jboss_monitoring

* Please give me feadback if you find bug or need some another check. Email is info"@"tomashermanek.cz, twitter is: hermanekt.

* Known zabbix java gateway BUG with discovery: https://support.zabbix.com/browse/ZBXNEXT-4209

* Debian, Ubuntu, CentOS, RHEL packages

This is auto Discovery template for monitoring Wildfly, EAP, Jboss servers (Domain,Standalone)
Tested with Wildfy 12,13,14,15,16

Static item:
```
JMX Wildfly Memory
JMX Wildfly Runtime
JMX Wildfly Threading
JMX Wildlfy Classes
JMX Wildlfy Server
```
Discover:
```
Datasource JDBC
Datasource POOL
Garbage Collector
Memory Pool
Server
```
### Minimal requirements and installation:
1) **Zabbix 3.4+**
2) **Install fixed Zabbix Java Gateway. From this repo.**

* Centos, RHEL
```
rpm -i https://github.com/hermanekt/Zabbix_wildfly_eap_jboss_monitoring/raw/master/zabbix-java-gateway-4.2.1-1.el7.x86_64.rpm
```

* Debian, Ubuntu
```
curl -sLO https://github.com/hermanekt/Zabbix_wildfly_eap_jboss_monitoring/raw/master/zabbix-java-gateway_4.2.1-1+stretch_all.deb && dpkg -i zabbix-java-gateway_4.2.1-1+stretch_all.deb
```

 * In future zabbix maybe have own fix. I sent the info to the zabbix.
	* If you don't have Debian, Ubuntu, CentOS 7 or RHEL 7, you need make own package.
	* I change in this file is: 
	```
	/root/rpmbuild/SOURCES/zabbix-4.2.1/src/zabbix_java/src/com/zabbix/gateway/JMXItemChecker.java
	```
	* We started change on the line 274:
	```
	274	// This is changed for replace special characters
	275	//String key = property.getKey().toUpperCase();
	276	String key = property.getKey().toUpperCase().replace('-', '_’);
	```
3) **Copy jboss-client.jar from /(wildfly,EAP,Jboss,AS)/bin/client in to directory /usr/share/zabbix-java-gateway/lib**
	
	Restart Zabbix Java gateway
	```
	systemctl restart zabbix-java-gateway
	```
	Check if jar file is loaded
	```
	ps xauf | grep jboss-client.jar
	```

## DOMAIN
4) **Setup JMX Access to Server Node. For example can use this howto:** 
* [JMX_Access_to_Domain_Mode_EAP_7_Server_Node](https://kb.novaordis.com/index.php/JMX_Access_to_Domain_Mode_EAP_7_Server_Node)
* Me setup work only with ManagementRealm.
* Maybe you need change this macro in template: {$JMX.PROTOCOL}

## STANDALONE
* [JMX_Access_standalone](https://dzone.com/articles/remote-jmx-access-wildfly-or)
* Port is 9990, test connect with jconsole
* Maybe you need change this {$JMX.PROTOCOL} macro in template

5) **Import template. In repo is 2 remplates for 3.4 and 4.+**
6) **If you have zabbix 3.4. Set Regular expression with name "Disable datasource discovery" for exclude some datasources from discovery For example:**
```
Disable datasource discovery ^(ExampleDS)$  [Result is FALSE]
```
**If you have version 4.+ and you want exclude some datasources you can use filters in discovery rule inside template is for example added ExampleDS datasource.**

7) **You need change macros in Template {$WILDFLY.PASS}, {$WILDFLY.USER} or you can overload in hosts.**

## Authors

* **Tomas Hermanek** - *Initial work* - [hermanekt](https://github.com/hermanekt)

## Acknowledgments

### Please give me feadback if you find bug or need some another check. Email is info"@"tomashermanek.cz, twitter is: hermanekt.
