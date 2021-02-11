## Aker Installation
#### Prerequisite
- Aker component are on /Source 

### 1.) Install dependencies offline 
user: root
```
cp -r /Source/aker-file /usr/bin/aker
cd /source 
yum install python2-redis-2.10.6-1.el7.noarch.rpm 
yum install python2-wcwidth-0.1.9-1.el7.noarch.rpm 
yum install python-configparser-4.0.2-2.el7.noarch.rpm 
yum install python-paramiko-2.1.1-9.el7.noarch.rpm 
yum install redis-6.0.4-2.el7.remi.x86_64.rpm 
yum install python-urwid 
```

### 2.) Set files executable perms 
user: root
```
chmod 755 /usr/bin/aker/*.py 
Setup logdir and perms 
mkdir /var/log/aker 
chmod 777 /var/log/aker 
touch /var/log/aker/aker.log 
chmod 777 /var/log/aker/aker.log  
chmod a+rx /usr/bin/aker/pyte 
chmod a+rx /usr/bin/aker/idp 
chmod a+rx /usr/bin/aker 
mkdir /etc/aker 
cp /usr/bin/aker/hosts.json /etc/aker/hosts.json 
chmod a+x /etc/aker/ 
```

### 3.)Enforce aker on all users but root, edit sshd_config 
user: root
```
vi /etc/ssh/sshd_config 

Match Group *,!support,!root,!redhat 
ForceCommand /usr/bin/aker/aker.py 
```
### 4.) Setup host who will connect using aker 

```
vi /etc/aker/aker.ini

[General]
log_level = INFO
ssh_port = 22

# Identity Provider to determine the list of available hosts
# options shipped are IPA, Json. Default is IPA
idp = IPA
hosts_file = /etc/aker/hosts.json

# FreeIPA hostgroup name contatining Aker gateways
# to be excluded from hosts presented to user
gateway_group = gateways
```
### 5.) Restart SSHD and Redis 
```
service sshd restart 
service redis restart
```
