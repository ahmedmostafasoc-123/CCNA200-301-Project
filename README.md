# CCNA200-301-Project
**project Name : Full CCNA 200-301 Finish project **
due date : from 1 July 2026 to 20 July 2026 
by : Ahmed Mostafa  ( cyber security analyst)




## 1. Subnets ( main network)

| Subnet Name    | Subnet mask     | Network IP    | Broadcast IP   | Number of hosts |
| -------------- | --------------- | ------------- | -------------- | --------------- |
| **IT**         | 255.255.255.192 | 192.168.10.0  | 192.168.10.63  | 62              |
| **Sales**      | 255.255.255.224 | 192.168.10.64 | 192.168.10.95  | 30              |
| **Management** | 255.255.255.240 | 192.168.10.96 | 192.168.10.111 | 14              |
## 2. Subnets ( branch network)
| Subnet Name | Subnet mask     | Network IP    | Broadcast IP  | Number of hosts |
| ----------- | --------------- | ------------- | ------------- | --------------- |
| **Support** | 255.255.255.224 | 192.168.20.0  | 192.168.20.31 | 30              |
| **Finance** | 255.255.255.240 | 192.168.20.32 | 192.168.20.47 | 14              |
## 3. Routers 

| Name              | Se0/3/1            | Gig0/0                     | Gig0/1                         | Gig0/2                    |
| ----------------- | ------------------ | -------------------------- | ------------------------------ | ------------------------- |
| **Main Router**   | 172.16.1.5 (WAN 1) | 192.168.10.1 (IT-LAN)      | 192.168.10.98 (Management-LAN) | 192.168.10.65 (Sales-LAN) |
| **Branch Router** | 172.16.1.6 (WAN 1) | 192.168.20.1 (Support-LAN) | 192.168.20.33 (Finance-LAN )   | --------                  |
## Steps :
1. implement a Design for topology and specify branches 
2. Divide each network to branches ( subnets ) using VLSM to Save ip addresses 
3. change routers hostnames 

`Router 1 :hostname main-Router`
`Router 2: hostname branch-Router`

4. set interfaces IP address and open interfaces with the command `no shutdown`
5. Enable DHCP on routers to get IP addresses dynamically 
6. enable OSPF Routing for each router
`main-router` 
`O       192.168.20.0/27 [110/65] via 172.16.1.6, 00:17:11, Serial0/3/1`
`O       192.168.20.32/28 [110/65] via 172.16.1.6, 00:17:11, Serial0/3/1`

`branch router
`O 192.168.10.0/26 [110/65] via 172.16.1.5, 00:19:23, Serial0/3/1`
`O 192.168.10.64/27 [110/65] via 172.16.1.5, 00:19:23, Serial0/3/1`
`O 192.168.10.96/28 [110/65] via 172.16.1.5, 00:19:23, Serial0/3/1`

7. enable Extended access control list block pc 3 on finance subnet from access http on web server in management subnet on interface Gig0/0 on branch router 

`access-list 101 deny tcp host 192.168.20.36 host 192.168.10.101 eq 80`
`access-list 101 IP permit any`
`int Gig0/0 `
`IP access group 101 in`

8. set main router as NTP server for branch router 

configuration on main router :

`set clock <Time><Date> `
`NTP master `

configuration on branch router 

`ntp server 172.16.1.5`
9. Set backup server for main network and backup server for branch 

main router server IP : 192.168.10.5 
`configuration file backup`
	`Copy run tftp `
		`192.168.10.5`
		`backup-main`
`IOS file backup`
	`copy flash tftp`
	`192.168.1.5 `
	`IOS file name .bin `
	`main-IOS-backup`


main router server IP : 192.168.20.40

`configuration file backup`
	`Copy run tftp `
		`192.168.20.40 
		`backup-main`
`IOS file backup`
	`copy flash tftp`
	`192.168.20.40 `
	`IOS file name .bin `
	`office-IOS

## Verification and Testing 

1. Success Ping between all subnets 
2. Trace Route 
3. make sure all the interface is UP/UP using command 
`show ip interface brief`

**main-router** 
**Interface                            IP-Address           OK?        Method     Status     Protocol**
GigabitEthernet0/0           192.168.10.1        YES          NVRAM      up           up
GigabitEthernet0/1           192.168.10.98       YES          NVRAM      up           up
GigabitEthernet0/2           192.168.10.65       YES         NVRAM      up           up
Serial0/3/1                        172.16.1.5             YES         manual       up           up

**branch-router** 
**Interface                           IP-Address          OK?            Method       Status      Protocol**
GigabitEthernet0/0          192.168.20.33      YES             manual         up             up
GigabitEthernet0/1           192.168.20.1       YES             manual          up            up
Serial0/3/1                         172.16.1.6          YES             manual          up            up
