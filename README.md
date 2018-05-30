# Zabbix_wildfly_eap_jboss_monitoring

This is auto Discovery template for monitoring Wildfly, EAP, Jboss servers (standalone not tested)
Tested with Wildfy 12

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
```
rpm -i https://github.com/hermanekt/Zabbix_wildfly_eap_jboss_monitoring/raw/master/zabbix-java-gateway-3.4.9-1.el7.x86_64.rpm
```
 * In future zabbix maybe have own fix. I sent the info to the zabbix.
	* If you don't have CentOS 7 or RHEL 7, you need make own rpm package.
	* I change in this file is: 
	```
	/root/rpmbuild/SOURCES/zabbix-3.4.9/src/zabbix_java/src/com/zabbix/gateway/JMXItemChecker.java
	```
	* We started change on the line 274:
	```
	274	// This is changed for replace special characters
	275	//String key = property.getKey().toUpperCase();
	276	String key = property.getKey().toUpperCase().replace('-', '_’);
	```
3) **Copy jboss-client.jar from /wildfly-12/bin/client in to directory /usr/share/zabbix-java-gateway/lib**
	
	Restart Zabbix Java gateway
	```
	systemctl restart zabbix-java-gateway.service
	```
	Check if jar file is loaded
	```
	ps xauf | grep jboss-client.jar
	```

4) **Setup JMX Access to Server Node. For example can use this howto:** 
* [JMX_Access_to_Domain_Mode_EAP_7_Server_Node](https://kb.novaordis.com/index.php/JMX_Access_to_Domain_Mode_EAP_7_Server_Node)
* Me setup work only with ManagementRealm.
5) **Import template.**
6) **Set Regular expression with name "Disable datasource discovery" for exclude some datasources from discovery For example:** 
```
Disable datasource discovery ^(ExampleDS)$  [Result is FALSE]
```
7) **You need change macros in Template {$WILDFLY.PASS}, {$WILDFLY.USER} or you can overload in hosts.**

## Authors

* **Tomas Hermanek** - *Initial work* - [hermanekt](https://github.com/hermanekt)

## Acknowledgments

### This is Alfa version, please give me feadback if you find bug or need some another check. Email is info"@"tomashermanek.cz, twitter is: hermanekt.
