# SNMP Enumeration Tools

## SNMP Walk: 

```
$ snmpwalk -c public -v1 ipaddress 1
$ snmpwalk -c private -v1 ipaddress 1
$ snmpwalk -c manager -v1 ipaddress 1
```

## Nmap Enumeration

`nmap 172.21.0.0 -Pn -sU -p 161 --script=`

```
$ ls -lh /usr/share/nmap/scripts/ | grep snmp
-rw-r--r-- 1 root root  7814 Oct 12 09:29 snmp-brute.nse
-rw-r--r-- 1 root root  4388 Oct 12 09:29 snmp-hh3c-logins.nse
-rw-r--r-- 1 root root  5216 Oct 12 09:29 snmp-info.nse
-rw-r--r-- 1 root root 28644 Oct 12 09:29 snmp-interfaces.nse
-rw-r--r-- 1 root root  5978 Oct 12 09:29 snmp-ios-config.nse
-rw-r--r-- 1 root root  4156 Oct 12 09:29 snmp-netstat.nse
-rw-r--r-- 1 root root  4431 Oct 12 09:29 snmp-processes.nse
-rw-r--r-- 1 root root  1867 Oct 12 09:29 snmp-sysdescr.nse
-rw-r--r-- 1 root root  2570 Oct 12 09:29 snmp-win32-services.nse
-rw-r--r-- 1 root root  2739 Oct 12 09:29 snmp-win32-shares.nse
-rw-r--r-- 1 root root  4713 Oct 12 09:29 snmp-win32-software.nse
-rw-r--r-- 1 root root  2016 Oct 12 09:29 snmp-win32-users.nse
$ nmap x.x.x.x -p 161,162 -sV --script=exampleScript1.nse,exampleScript2.nse
```

## Metasploit aux modules

```
 auxiliary/scanner/misc/oki_scanner                                    
 auxiliary/scanner/snmp/aix_version                                   
 auxiliary/scanner/snmp/arris_dg950                                   
 auxiliary/scanner/snmp/brocade_enumhash                               
 auxiliary/scanner/snmp/cisco_config_tftp                               
 auxiliary/scanner/snmp/cisco_upload_file                              
 auxiliary/scanner/snmp/cnpilot_r_snmp_loot                             
 auxiliary/scanner/snmp/epmp1000_snmp_loot                             
 auxiliary/scanner/snmp/netopia_enum                                    
 auxiliary/scanner/snmp/sbg6580_enum                                 
 auxiliary/scanner/snmp/snmp_enum                                 
 auxiliary/scanner/snmp/snmp_enum_hp_laserjet                           
 auxiliary/scanner/snmp/snmp_enumshares                                
 auxiliary/scanner/snmp/snmp_enumusers                                 
 auxiliary/scanner/snmp/snmp_login                                     
```

## Onesixtyone

```
$ onesixtyone -c /usr/share/doc/onesixtyone/dict.txt x.x.x.x
```

## Snmp-check

```
$ snmp-check x.x.x.x -c public
```

## Impacket

```
python3 samdump.py SNMP x.x.x.x
```
