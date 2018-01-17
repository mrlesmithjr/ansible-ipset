# ansible-ipset

An [Ansible](https://www.ansible.com) role to install/configure [ipset](http://ipset.netfilter.org/)

> NOTE: This role will also manage [IPTables](http://netfilter.org/projects/iptables/index.html)
> rules as part of configuring ipset. Any existing IPTables rules **WILL** be
> removed.

## Requirements

## Role Variables

[defaults/main.yml](defaults/main.yml)

## Dependencies

## Example Playbook

[playbook.yml](./playbook.yml)

## Examples

### Example ipset list

```bash
vagrant@node0:~$ sudo ipset list
Name: safe_input
Type: hash:net
Revision: 6
Header: family inet hashsize 1024 maxelem 1000111222
Size in memory: 448
References: 1
Members:
10.0.0.0/8

Name: dshield_block_list
Type: hash:net
Revision: 6
Header: family inet hashsize 1024 maxelem 1000111222
Size in memory: 1664
References: 4
Members:
85.93.20.0/24
5.188.203.0/24
104.236.178.0/24
77.72.85.0/24
77.72.82.0/24
181.214.87.0/24
5.188.11.0/24
46.29.162.0/24
141.212.122.0/24
80.82.77.0/24
180.97.106.0/24
185.35.62.0/24
216.158.238.0/24
5.188.86.0/24
191.101.167.0/24
93.174.93.0/24
109.248.9.0/24
5.188.10.0/24
80.82.70.0/24
196.52.43.0/24
```

### Example iptables list

```bash
[vagrant@node0 ~]$ sudo iptables -L -n -v
Chain INPUT (policy DROP 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0
   65  5541 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            ctstate RELATED,ESTABLISHED
    1    44 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            multiport dports 22,2202,2222 ctstate NEW match-set safe_input src
    0     0 DROP       all  --  *      *       0.0.0.0/0            0.0.0.0/0            match-set dshield_block_list src

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy DROP 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 ACCEPT     all  --  *      lo      0.0.0.0/0            0.0.0.0/0
   22  4257 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            ctstate ESTABLISHED
   33  1988 ACCEPT     udp  --  *      *       0.0.0.0/0            0.0.0.0/0            multiport dports 53,123 ctstate NEW
    0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            multiport dports 22,80 ctstate NEW
    0     0 DROP       all  --  *      *       0.0.0.0/0            0.0.0.0/0            match-set dshield_block_list dst
```

## License

MIT

## Author Information

Larry Smith Jr.

-   [EverythingShouldBeVirtual](http://everythingshouldbevirtual.com)
-   [@mrlesmithjr](https://www.twitter.com/mrlesmithjr)
-   <mailto:mrlesmithjr@gmail.com>
