# Firewall Management



### Debian Firewall



that firewalling functionality is already built into the Linux kernel to a certain extent; e.g. `nftables` and `netfilter`. Debian and other Linux distributions do provide user space tools like `iptables` to manage that functionality. But what you do with them is up to you.

nftables is configured via the user-space utility nft while netfilter is configured via the utilities iptables, ip6tables, arptables and ebtables frameworks.



### CentOS//RHEL Firewall

CentOS 7 firewall-cmd

```shell
# To open up a new port (e.g., TCP/80) permanently
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd --reload
#Check the updated rules with
firewall-cmd --list-all
```



### Common Firewall Management

#### Iptables

**iptables** is an application that allows users to configure specific rules that will be enforced by the kernel’s `netfilter` framework. It acts as a packet filter and firewall that examines and directs traffic based on port, protocol and other criteria. This guide will focus on the configuration and application of iptables rulesets and will provide examples of ways they are commonly used.

```
## set default policies to let everything in
ip6tables --policy INPUT   ACCEPT;
ip6tables --policy OUTPUT  ACCEPT;
ip6tables --policy FORWARD ACCEPT;
## set default policies to let everything in
iptables --policy INPUT   ACCEPT;
iptables --policy OUTPUT  ACCEPT;
iptables --policy FORWARD ACCEPT;
## start fresh
ip6tables -Z; # zero counters
ip6tables -F; # flush (delete) rules
ip6tables -X; # delete all extra chains
```

##### UFW

Uncomplicated Firewall (ufw) is a frontend for iptables and is particularly well-suited for host-based firewalls.

1

UFW is just a tool that manipulates iptables rules. This means if you apply an UFW rule (in this case port 22) and run `iptables -F` after that, you will flush the rule you've just entered with UFW.

You can check your rule by doing `iptables -L` after applying the ufw rule.

2

ufw is a front-end for netfilter/iptables, the Linux mechanism for routing and filtering internet traffic.

ufw is completely optional and it's possible to create firewall and routing tables directly using the iptables commands. Some people prefer the syntax of ufw, which is supposed to make it a bit easier.

ufw itself is not the firewall, it's a tool for setting the configuration of netfilter/iptables. It is registered as a service because it needs to be run every time your machine starts up. I don't believe it stays resident in memory after it has done this configuration. "restarting" the service simply re-runs its scripts. It is not unusual for things in Linux distributions to be registered as services even though they are merely scripts that run at boot or shutdown without staying resident in between.

If you use ufw you can't set your own iptables rules using your own scripts, as they will be overwritten when ufw sets its own. This also means that ufw can conflict with other tools that set firewall rules.

If `sudo ufw status` returns "inactive", it means ufw has been disabled (and won't, for example, re-apply any rules at startup). You need to ensure ufw is enabled with `sudo ufw enable`, though you also need to configure rules for this to have meaning. If you are sure you have enabled ufw but it returns "inactive" there could be some other issue.

Note that "starting" the service doesn't enable ufw, it just activates the start-up script. If ufw is disabled when the start-up script runs it won't do anything.

You can tell if your firewall rules have been applied at any given time using iptables directly:

```
sudo iptables -L
sudo ip6tables -L
```



#### Using UFW

- Install UFW
- Check UFW status `ufw status`
- Allow Connections `ufw allow`
- Deny Connections `ufw deny`
- Enable UFW `ufw enable`
- Disable/reload/restart UFW `ufw diable/reload/enable`
- Removing rules `ufw delete`
- Back to default Settings `ufw reset`

ran 'iptables -F' (to remove previous iptables rules that were still active), then reset `ufw`, configuring `ufw` and enable `ufw`.

Common Operation

```shell
# 0. install ufw
sudo apt install -y ufw
ufw status # inactive

# 1. config ufw (need restart system)
# Flush the tables to apply changes
iptables -F
ufw reset
ufw default deny incoming
ufw default allow outgoing
ufw allow OpenSSH
ufw allow http
ufw allow https
ufw enable
# restart system make it work
reboot


# 2. verify the ufw firewall is work 
# 2.1 command $ iptables -L
# Chain INPUT (policy DROP), Chain FORWARD (policy DROP), Chain OUTPUT (policy DROP) all target is ufw-xxx
# 2.2 Port Scan tool. 
# the result of the port scan is find all opened firewall ports. some ports running process is display open, some ports not running process is close. But it can find all your opened ports.


# 3. view open port
ufw status

# 4. remove allow port (need restart system)
ufw delete allow 8080/tcp
# or
ufw status numbered
ufw delete <number>
# restart make it work
reboot

# 5. allow port (not need restart system)
ufw allow 8080/tcp
```



Install UFW

```shell
sudo apt install ufw
```


Check UFW Status

```shell
$ sudo ufw status
# or
$ sudo ufw status verbose
Output
Status: inactive
# UFW is disabled by default. The installation will not activate the firewall automatically to avoid a lockout from the server.
```

Application Profiles

```shell
# To list all application profiles available on your system type
$ sudo ufw app list
Output
Available applications:
  DNS
  IMAP
  IMAPS
  OpenSSH
  POP3
  POP3S
  Postfix
  Postfix SMTPS
  Postfix Submission

# To find more information about a specific profile and included rules
$ sudo ufw app info OpenSSH
Output
Profile: OpenSSH
Title: Secure shell server, an rshd replacement
Description: OpenSSH is a free implementation of the Secure Shell protocol.

Port:
  22/tcp
```

Set Up Defaults

```shell
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Allow SSH Connections

```shell
sudo ufw allow OpenSSH
# or
sudo ufw allow 22/tcp
```

Enable UFW

```shell
$ sudo ufw enable
# veify status
$ sudo ufw status
active
```

Allow Connections on other ports

```shell
# Open port 80 - HTTP
sudo ufw allow http
# or 
sudo ufw allow 80/tcp

# Open port 443 - HTTPS
sudo ufw allow https
# or
sudo ufw allow 443/tcp

# Open port 8080
sudo ufw allow 8080/tcp

# Allow Port Ranges
sudo ufw allow 7100:7200/tcp
sudo ufw allow 7100:7200/udp

# Allow Specific IP Addresses(to allow access on all ports from a specific IP address)
sudo ufw allow from 64.63.62.61

# Allow Specific IP Addresses on Specific port
sudo ufw allow from 64.63.62.61 to any port 22

# Allow Subnets
# For example, if you want to allow access for IP addresses ranging from 192.168.1.1 to 192.168.1.254 to port 3360 (MySQL) 
sudo ufw allow from 192.168.1.0/24 to any port 3306

# Allow Connections to a Specific Network Interface
sudo ufw allow in on eth2 to any port 3306
```

Deny Connections

```shell
# sudo ufw deny <port>/<optional: protocol>
sudo ufw deny 3306
# or 
sudo ufw deny mysql

# To deny all connections from 23.24.25.0/24
sudo ufw deny from 23.24.25.0/24
# deny access to ports 80 and 443 from 23.24.25.0/24 use
sudo ufw deny from 23.24.25.0/24 to any port 80
sudo ufw deny from 23.24.25.0/24 to any port 443
```

Removing rules

```shell
# find the number of the rule
sudo ufw status numbered
# To delete rule number 3
sudo ufw delete 3
# To delete a rule by specifying the actual rule. (For example, if you added a rule to open port 8069 you can delete it)
sudo ufw delete allow 8069
```

Disable UFW

```shell
# If for any reason you want to stop UFW and deactivate all rules 
sudo ufw disable
# if you want to re-enable UTF and activate all rules 
sudo ufw enable
```

Reset UFW

```shell
# Resetting UFW will disable UFW, and delete all active rules. This is helpful if you want to revert all of your changes and start fresh.
sudo ufw reset
```

only using UFW 

```shell
$ iptables -L
# ufw rules are in the INPUT, OUTPUT, and FORWARD chains.
Chain INPUT (policy DROP)
target     prot opt source               destination         
ufw-before-logging-input  all  --  anywhere             anywhere            
ufw-before-input  all  --  anywhere             anywhere            
ufw-after-input  all  --  anywhere             anywhere            
ufw-after-logging-input  all  --  anywhere             anywhere            
ufw-reject-input  all  --  anywhere             anywhere            
ufw-track-input  all  --  anywhere             anywhere            

Chain FORWARD (policy DROP)
target     prot opt source               destination         
ufw-before-logging-forward  all  --  anywhere             anywhere            
ufw-before-forward  all  --  anywhere             anywhere            
ufw-after-forward  all  --  anywhere             anywhere            
ufw-after-logging-forward  all  --  anywhere             anywhere            
ufw-reject-forward  all  --  anywhere             anywhere            
ufw-track-forward  all  --  anywhere             anywhere            

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
ufw-before-logging-output  all  --  anywhere             anywhere            
ufw-before-output  all  --  anywhere             anywhere            
ufw-after-output  all  --  anywhere             anywhere            
ufw-after-logging-output  all  --  anywhere             anywhere            
ufw-reject-output  all  --  anywhere             anywhere            
ufw-track-output  all  --  anywhere             anywhere            

Chain ufw-after-forward (1 references)
target     prot opt source               destination         

Chain ufw-after-input (1 references)
target     prot opt source               destination         
ufw-skip-to-policy-input  udp  --  anywhere             anywhere             udp dpt:netbios-ns
ufw-skip-to-policy-input  udp  --  anywhere             anywhere             udp dpt:netbios-dgm
ufw-skip-to-policy-input  tcp  --  anywhere             anywhere             tcp dpt:netbios-ssn
ufw-skip-to-policy-input  tcp  --  anywhere             anywhere             tcp dpt:microsoft-ds
ufw-skip-to-policy-input  udp  --  anywhere             anywhere             udp dpt:bootps
ufw-skip-to-policy-input  udp  --  anywhere             anywhere             udp dpt:bootpc
ufw-skip-to-policy-input  all  --  anywhere             anywhere             ADDRTYPE match dst-type BROADCAST

Chain ufw-after-logging-forward (1 references)
target     prot opt source               destination         
LOG        all  --  anywhere             anywhere             limit: avg 3/min burst 10 LOG level warning prefix "[UFW BLOCK] "

Chain ufw-after-logging-input (1 references)
target     prot opt source               destination         
LOG        all  --  anywhere             anywhere             limit: avg 3/min burst 10 LOG level warning prefix "[UFW BLOCK] "

Chain ufw-after-logging-output (1 references)
target     prot opt source               destination         

Chain ufw-after-output (1 references)
target     prot opt source               destination         

Chain ufw-before-forward (1 references)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ACCEPT     icmp --  anywhere             anywhere             icmp destination-unreachable
ACCEPT     icmp --  anywhere             anywhere             icmp source-quench
ACCEPT     icmp --  anywhere             anywhere             icmp time-exceeded
ACCEPT     icmp --  anywhere             anywhere             icmp parameter-problem
ACCEPT     icmp --  anywhere             anywhere             icmp echo-request
ufw-user-forward  all  --  anywhere             anywhere            

Chain ufw-before-input (1 references)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ufw-logging-deny  all  --  anywhere             anywhere             ctstate INVALID
DROP       all  --  anywhere             anywhere             ctstate INVALID
ACCEPT     icmp --  anywhere             anywhere             icmp destination-unreachable
ACCEPT     icmp --  anywhere             anywhere             icmp source-quench
ACCEPT     icmp --  anywhere             anywhere             icmp time-exceeded
ACCEPT     icmp --  anywhere             anywhere             icmp parameter-problem
ACCEPT     icmp --  anywhere             anywhere             icmp echo-request
ACCEPT     udp  --  anywhere             anywhere             udp spt:bootps dpt:bootpc
ufw-not-local  all  --  anywhere             anywhere            
ACCEPT     udp  --  anywhere             224.0.0.251          udp dpt:mdns
ACCEPT     udp  --  anywhere             239.255.255.250      udp dpt:1900
ufw-user-input  all  --  anywhere             anywhere            

Chain ufw-before-logging-forward (1 references)
target     prot opt source               destination         

Chain ufw-before-logging-input (1 references)
target     prot opt source               destination         

Chain ufw-before-logging-output (1 references)
target     prot opt source               destination         

Chain ufw-before-output (1 references)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ufw-user-output  all  --  anywhere             anywhere            

Chain ufw-logging-allow (0 references)
target     prot opt source               destination         
LOG        all  --  anywhere             anywhere             limit: avg 3/min burst 10 LOG level warning prefix "[UFW ALLOW] "

Chain ufw-logging-deny (2 references)
target     prot opt source               destination         
RETURN     all  --  anywhere             anywhere             ctstate INVALID limit: avg 3/min burst 10
LOG        all  --  anywhere             anywhere             limit: avg 3/min burst 10 LOG level warning prefix "[UFW BLOCK] "

Chain ufw-not-local (1 references)
target     prot opt source               destination         
RETURN     all  --  anywhere             anywhere             ADDRTYPE match dst-type LOCAL
RETURN     all  --  anywhere             anywhere             ADDRTYPE match dst-type MULTICAST
RETURN     all  --  anywhere             anywhere             ADDRTYPE match dst-type BROADCAST
ufw-logging-deny  all  --  anywhere             anywhere             limit: avg 3/min burst 10
DROP       all  --  anywhere             anywhere            

Chain ufw-reject-forward (1 references)
target     prot opt source               destination         

Chain ufw-reject-input (1 references)
target     prot opt source               destination         

Chain ufw-reject-output (1 references)
target     prot opt source               destination         

Chain ufw-skip-to-policy-forward (0 references)
target     prot opt source               destination         
DROP       all  --  anywhere             anywhere            

Chain ufw-skip-to-policy-input (7 references)
target     prot opt source               destination         
DROP       all  --  anywhere             anywhere            

Chain ufw-skip-to-policy-output (0 references)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             anywhere            

Chain ufw-track-forward (1 references)
target     prot opt source               destination         

Chain ufw-track-input (1 references)
target     prot opt source               destination         

Chain ufw-track-output (1 references)
target     prot opt source               destination         
ACCEPT     tcp  --  anywhere             anywhere             ctstate NEW
ACCEPT     udp  --  anywhere             anywhere             ctstate NEW

Chain ufw-user-forward (1 references)
target     prot opt source               destination         

Chain ufw-user-input (1 references)
target     prot opt source               destination         
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:ssh
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:http
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:https

Chain ufw-user-limit (0 references)
target     prot opt source               destination         
LOG        all  --  anywhere             anywhere             limit: avg 3/min burst 5 LOG level warning prefix "[UFW LIMIT BLOCK] "
REJECT     all  --  anywhere             anywhere             reject-with icmp-port-unreachable

Chain ufw-user-limit-accept (0 references)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             anywhere            

Chain ufw-user-logging-forward (0 references)
target     prot opt source               destination         

Chain ufw-user-logging-input (0 references)
target     prot opt source               destination         

Chain ufw-user-logging-output (0 references)
target     prot opt source               destination         

Chain ufw-user-output (1 references)
target     prot opt source               destination 
```





### References

Firewall

- [Debian Firewall - debian wiki](https://wiki.debian.org/DebianFirewall)

UFW

- [How To Set Up a Firewall with UFW on Debian 9](https://linuxize.com/post/how-to-setup-a-firewall-with-ufw-on-debian-9)
- [Configure Uncomplicated Firewall (UFW) on Ubuntu 14.04](https://www.vultr.com/docs/how-to-configure-ufw-firewall-on-ubuntu-14-04)
- [UFW is blocking all even when I set rules to allow - ask ubuntu](https://askubuntu.com/questions/541675/ufw-is-blocking-all-even-when-i-set-rules-to-allow)