# Zabbix_wildfly_eap_jboss_monitoring

This is auto Discovery template for monitoring Wildfly, EAP, Jboss servers (domain, standalone not tested)

Minimal requirements and installation:
1) Zabbix 3.4+
2) Installed fixed Zabbix Java Gateway. From this repo. In future zabbix maybe have own fix. I send me fix in to zabbix.
	If you don't have CentOS 7 or RHEL 7 I fix in source, you need make own rpm package.
	I change in this file is: /root/rpmbuild/SOURCES/zabbix-3.4.9/src/zabbix_java/src/com/zabbix/gateway/JMXItemChecker.java
	
	We started change on the line 274:
	274                                 // This is changed for replace special characters
	275                                 //String key = property.getKey().toUpperCase();
	276                                 String key = property.getKey().toUpperCase().replace('-', '_’);
	
3) Setup JMX Access to Server Node. For example can use this howto https://kb.novaordis.com/index.php/JMX_Access_to_Domain_Mode_EAP_7_Server_Node.
4) Imported template.
5) Set Regular expression with name "Disable datasource discovery" for exclude some datasources from discovery For example: Disable datasource discovery ^(ExampleDS)$	[Result is FALSE]

