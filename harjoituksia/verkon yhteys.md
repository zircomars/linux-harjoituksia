# verkon diagnostiikka
Verkon yhteys koskien on mm. DNS / yhteys ja ARP - usein esim. päästäkseen kohteen nettisivustolle

# IP-osoitteet
Tarkistuksena tarkistaa oman IP-osoitteen mikä on oma yhteys
```
┌──(kali㉿kali)-[~]
└─$ ip a
1: lo: 
2: eth0:
```

# muita IP vaihtoehtoisia termistöjä
```
┌──(kali㉿kali)-[~]
└─$ ip  
Usage: ip [ OPTIONS ] OBJECT { COMMAND | help }
       ip [ -force ] -batch filename
where  OBJECT := { address | addrlabel | fou | help | ila | ioam | l2tp | link |
                   macsec | maddress | monitor | mptcp | mroute | mrule |
                   neighbor | neighbour | netconf | netns | nexthop | ntable |
                   ntbl | route | rule | sr | stats | tap | tcpmetrics |
                   token | tunnel | tuntap | vrf | xfrm }
       OPTIONS := { -V[ersion] | -s[tatistics] | -d[etails] | -r[esolve] |
                    -h[uman-readable] | -iec | -j[son] | -p[retty] |
                    -f[amily] { inet | inet6 | mpls | bridge | link } |
                    -4 | -6 | -M | -B | -0 |
                    -l[oops] { maximum-addr-flush-attempts } | -echo | -br[ief] |
                    -o[neline] | -t[imestamp] | -ts[hort] | -b[atch] [filename] |
                    -rc[vbuf] [size] | -n[etns] name | -N[umeric] | -a[ll] |
                    -c[olor]}
```

## reitit
sama idea kuin tarkistaisi oman IP-osoitteen yhteyden, että tarkistuksena tarkistaa reitit
```
┌──(kali㉿kali)-[~]
└─$ ip r
default via
```

# Portit ja kuuntelevat palvelut
```
┌──(kali㉿kali)-[~]
└─$ ss -tulpn
Netid     State     Recv-Q     Send-Q         Local Address:Port          Peer Address:Port     Process
```

                                                                                                          
# DNS tarkistus
```
┌──(kali㉿kali)-[~]
└─$ dig google.com

; <<>> DiG 9.20.15-2-Debian <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 53231
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; MBZ: 0x0005, udp: 4096
; COOKIE: d8567a1b394863e3010000006970d98dcbd45143df1f225f (good)
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             5       IN      A       216.58.210.142

;; Query time: 4 msec
;; SERVER: 192.168.195.2#53(192.168.195.2) (UDP)
;; WHEN: Wed Jan 21 08:50:05 EST 2026
;; MSG SIZE  rcvd: 83
                                                                                                        
┌──(kali㉿kali)-[~]
└─$ nslookup google.com
Server:         192.168.195.2
Address:        192.168.195.2#53

Non-authoritative answer:
Name:   google.com
Address: 216.58.210.142
Name:   google.com
Address: 2a00:1450:4026:804::200e
                                                                                                           
┌──(kali㉿kali)-[~]
└─$ host google.com
google.com has address 216.58.210.142
google.com has IPv6 address 2a00:1450:4026:804::200e
google.com mail is handled by 10 smtp.google.com.
google.com has HTTP service bindings 1 . alpn="h2,h3"
```

# HTTP yhteys
```                                                                                                            
┌──(kali㉿kali)-[~]
└─$ curl http://example.com
<!doctype html><html lang="en"><head><title>Example Domain</title><meta name="viewport" content="width=device-width, initial-scale=1"><style>body{background:#eee;width:60vw;margin:15vh auto;font-family:system-ui,sans-serif}h1{font-size:1.5em}div{opacity:0.8}a:link,a:visited{color:#348}</style><body><div><h1>Example Domain</h1><p>This domain is for use in documentation examples without needing permission. Avoid use in operations.<p><a href="https://iana.org/domains/example">Learn more</a></div></body></html>
```


# ARP-taulukko
paras sanontana lähiverkko alueet esim. lähistöllä on mm. tulostin ja muita laiteitta yhteydessä:
```
┌──(kali㉿kali)-[~]
└─$ arp -a
```


# Yhteyden testausta
Testataan ja tarkistellaan pelittääkö yhteys, että vastaanottaja saa yhteyden
```
┌──(kali㉿kali)-[~]
└─$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=128 time=7.39 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=128 time=6.55 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=128 time=8.01 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=128 time=5.43 ms
^C
--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3006ms
rtt min/avg/max/mdev = 5.430/6.843/8.006/0.965 ms
                                                                                                            
┌──(kali㉿kali)-[~]
└─$ ping google.com
PING google.com (216.58.210.142) 56(84) bytes of data.
64 bytes from mad06s09-in-f142.1e100.net (216.58.210.142): icmp_seq=1 ttl=128 time=6.60 ms
64 bytes from mad06s09-in-f142.1e100.net (216.58.210.142): icmp_seq=2 ttl=128 time=134 ms
64 bytes from mad06s09-in-f142.1e100.net (216.58.210.142): icmp_seq=3 ttl=128 time=7.00 ms
64 bytes from mad06s09-in-f142.1e100.net (216.58.210.142): icmp_seq=4 ttl=128 time=131 ms
64 bytes from mad06s09-in-f142.1e100.net (216.58.210.142): icmp_seq=5 ttl=128 time=152 ms
^C
--- google.com ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4008ms
rtt min/avg/max/mdev = 6.602/86.245/151.962/65.247 ms
```






