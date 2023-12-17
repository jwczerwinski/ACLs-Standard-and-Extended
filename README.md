# ACLs-Standard-and-Extended

<h2>Description</h2>
On R1 and R2, configure OSPF, then standard and extended ACLs. Verify results.

<h2>Environments Used </h2>

- <b>Cisco Packet Tracer</b> (2.2.43) <br />

- <b>Cisco IOS C2900 Software Version 15.1(4)M5<br />

<h2>Diagram </h2>
<img src="https://i.imgur.com/fgEx8D3.png" height="80%" width="80%" />

<h2>Walk-through:</h2>
<h3> Standard ACLs</h3>
Configure R1 and R2 with OSPF.<br />
R1(config)#router ospf 1<br />
R1(config-router)#network 0.0.0.0 255.255.255.255 area 0<br />
R2(config)#router ospf 1<br />
R2(config-router)#network 0.0.0.0 255.255.255.255 area 0<br />
<img src="https://i.imgur.com/wadLJYy.png" height="80%" width="80%" /> <br />
<img src="https://i.imgur.com/2IceWyP.png" height="80%" width="80%" /> <br />
<br />
<br />
Configure R1 and R2 with the appropriate named ACLs in diagram. <br />
R2(config-if)#ip access-list standard TO_192.168.1.0/24<br />
R2(config-std-nacl)#permit 172.16.1.1<br />
R2(config-std-nacl)#permit 172.16.2.1<br />
R2(config-std-nacl)#deny any<br />
R2(config-std-nacl)#int g0/0<br />
R2(config-if)#ip access-group TO_192.168.1.0/24 out<br />
<img src="https://i.imgur.com/rEQ2XXO.png" height="80%" width="80%" /> <br />
<img src="https://i.imgur.com/9vjoHZF.png" height="80%" width="80%" /> <br />
<img src="https://i.imgur.com/e7l7VMi.png" height="80%" width="80%" /> <br />
R2(config)#ip access-list standard TO_192.168.2.0/24<br />
R2(config-std-nacl)#deny 172.16.2.0 0.0.0.255<br />
R2(config-std-nacl)#int g0/1<br />
R2(config-if)#ip access-group TO_192.168.2.0/24 out<br />
R2(config-if)#do show run<br />
<img src="https://i.imgur.com/Fgnpu5w.png" height="80%" width="80%" /> <br />
<img src="https://i.imgur.com/i5Jf71p.png" height="80%" width="80%" /> <br />
<br />
<br />
Configure R1 with appropriate numbered ACLs from diagram.<br />
R1(config)#access-list 1 deny 172.16.1.0 0.0.0.255<br />
R1(config)#access-list 1 permit any<br />
R1(config)#access-list 2 deny 172.16.2.0 0.0.0.255<br />
R1(config)#access-list 2 permit any<br />
R1(config)#int g0/0<br />
R1(config-if)#ip access-group 2 out<br />
R1(config-if)#int g0/1<br />
R1(config-if)#ip access-group 1 out<br />
<img src="https://i.imgur.com/13cOFMK.png" height="80%" width="80%" /> <br />
<img src="https://i.imgur.com/xXByX6p.png" height="80%" width="80%" /> <br />
<img src="https://i.imgur.com/yvSt919.png" height="80%" width="80%" /> <br />
<br />
<br />
<br />
<br />
<h3> Extended ACLs</h3>
Configure R1 to restrict access from 172.16.2.0/24 to 172.16.1.1.<br />
R1(config)#ip access-list extended 100<br />
R1(config-ext-nacl)#deny ip 172.16.2.0 0.0.0.255 host 172.16.1.1<br />
R1(config-ext-nacl)#permit ip any any<br />
R1(config-ext-nacl)#int g0/0<br />
R1(config-if)#ip access-group 100 out<br />
<img src="https://i.imgur.com/OSc28ig.png" height="80%" width="80%" /> <br />
<img src="https://i.imgur.com/63NsyHW.png" height="80%" width="80%" /> <br />
<img src="https://i.imgur.com/JwyfMlx.png" height="80%" width="80%" /> <br />
<br />
<br />
Configure R1 to restrict hosts in 172.16.1.0/24 from accessing DNS service on SRV1.<br />
R1(config-if)#ip access-list extended 101<br />
R1(config-ext-nacl)#deny udp 172.16.1.0 0.0.0.255 host 192.168.1.100 eq 53<br />
R1(config-ext-nacl)#deny tcp 172.16.1.0 0.0.0.255 host 192.168.1.100 eq 53<br />
R1(config-ext-nacl)#permit ip any any<br />
R1(config-ext-nacl)#int g0/0<br />
R1(config-if)#ip access-group 101 in<br />
<img src="https://i.imgur.com/RVDQaTW.png" height="80%" width="80%" /> <br />
<img src="https://i.imgur.com/M1dpfVj.png" height="80%" width="80%" /> <br />
<br />
<br />
Configure R1 to restrict hosts in 172.16.2.0/24 from accessing HTTP or HTTPS services on SRV2.<br />
R1(config-ext-nacl)#deny tcp 172.16.2.0 0.0.0.255 host 192.168.2.100 eq 80<br />
R1(config-ext-nacl)#deny tcp 172.16.2.0 0.0.0.255 host 192.168.2.100 eq 443<br />
R1(config-ext-nacl)#permit ip any any<br />
R1(config-ext-nacl)#int g0/1<br />
R1(config-if)#ip access-group 102 in<br />
<img src="https://i.imgur.com/zdfnLUE.png" height="80%" width="80%" /> <br />
<img src="https://i.imgur.com/Nu41fPb.png" height="80%" width="80%" /> <br />
<img src="https://i.imgur.com/kvJZJOd.png" height="80%" width="80%" /> <br />
