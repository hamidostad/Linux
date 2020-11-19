# reset password zabbix users in mysql:
update zabbix.users set passwd=md5('zabbix') where alias='Admin';
