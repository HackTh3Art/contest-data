
Ce trebuie sa apara in cerinte:
(storyline-ul:
Operation Parasite Network
In a world where the Internet has been gradually banned — starting from planet Earth and spreading across every other inhabited world — six colonies managed to escape the purge… but only for a while.

These AI sentinels, known as the Virex, have taken on the task of eradicating any trace of the Internet’s existence . They’re not just shutting down networks — they’re erasing all records, all memories, everything that came before their rise. Now, they’re coming for the last remaining networks.

And so, it falls to you, my friend. You are tasked with configuring — and more importantly, securing — the final cores that keep these networks alive. You must act fast. In four hours, the Virex will reach you, and once they do, they’ll unleash a new-generation virus we've never seen before… and don’t yet know how to defeat.

I’ve left you a few instructions below — but the rest is up to you. Will you save the last fragment of the Internet?)

---------------------------------------------------------------------------------------------------------------------------------------------------------------------
1. Config de baza: hostname, password pe line con 0 service pass-encryp, no ip domain-lookup.
2. Subnetare
3. Ip routing
4. Vlans + ROAS
5. Layer 3 routing - ip pe switch uri de mngmt
6. DHCP
7. Etherchannel - pe langa chestia de cine initiaza sa se puna si speed ul de 1000 mb/s
8. Security layer: DHCP snooping, DAI, port security, sh porturi nefolosite, portfast, vlan nativ + trecerea porturilor nefolosite in el, aging time
9. Debug: ghost vlan 666, duplex problems, stp problems, speed problems, vlan uri care nu au fost create, parola uitata, probleme enervante de etherchannel ... to be continued.
10. Wireless
11. Config server NTP + celelalte device-uri
12. OSPF + autentificare ospf
13. ACLs
14. SSH 
-intre timp ai fost rapit si trebuie sa set up network ul de la Virex, nu te lasa la cafea daca nu ii ajuti-
15. Subnetare ipv6 guysss, vlsm, also asignare ipv6 and dhcpv6 
16. OSPFv6 pe routerele Virex

---------------------------------------------------------------------------------------------------------------------------------------------------------------------
the ideas for each colony:
basic configs in toate coloniile
done 
1. SOLARA - subnetting, vlans, roas, ip on hosts, server ul de dhcp
done 
2. LYXIS - layer 3 routing, vlans, dhcp(in alt cluster, solara) + ip dhcp helper-address, QOS pt cele trei vlan uri, 
done 
3. OBSIDIA - !cea mai incurcata parte trebuie sa fie! va avea config deja niste vlan uri si cv dhcp **, etherchannel, port security, shutdown unused ports, bpdu guard, portfast, stp config, ip dhcp snooping (aici sa fie dhcp ul sa existe unul deja configurat + vlan uri ei sa faca doar ce trebuie pe securitate), DAI,  la etherchannel ala care initiaza are index ul mai mare, zonele marcate cu rosu vor lasa doar vlan ul respectiv zonei sa treaca pe leg trunk
   
4. NIXUS - debug: stp ul sa fie messed up (dau indicatii ce sa i faca a bit doar, pot sa calculeze ei care sa fie root ul), bpdu guard si portfast puse gresit => porturi inchise, duplex and speed mismatch, wireless shit, vlans,

5. SKYLON - chestii wireless, de configurat un ap si wireless router, wpa2 security, 2,4 ghz and 5 ghz channels

6. DARKWELL - virex a corrupt partea aia, acl ca sa nu ajunga in anumite parti din retea + debug la ospf ul de acolo ptc exista un rogue router care advertises rute gresite din cauza la metrica => toate pachetele catre hex core trec prin el , sw mo tr la etherchannel intr o parte nu e

---------------------------------------------------------------------------------------------------------------------------------------------------------------------

1. SOLARA - Where the network’s first light was born — the spark that keeps the rebellion alive - 
2. LYXIS - The heartbeat of connection, weaving lines that bind distant worlds together- 
3. OBSIDIA - A fortress forged in shadow, unyielding against every digital assault -
4. NIXUS - A fractured land of ghosts and glitches, where chaos must be tamed -
5. SKYLON - Silent signals drift through the ether, carrying hope on unseen waves -
6. DARKWELL - The hidden but corrupt mind behind the war — pulling strings and guarding secrets -

----------------------------------------------------------------------------------------------------------------------------------------------------------------------

de tinut minte: 
**in obsidia la sw port sec: Secure aging - gen pe langa cele basic
**ssh pe switch urile din zona lyxis marcate, primesc ip din zona in care sunt asignate
* idee pt 6, sa vada unde e facut un man in the middle si sa l intercepteze
*Backdoors into devices (e.g., SSH tunnels or hardcoded static routes pointing to it)
Students must:Discover that traffic from routers is being mirrored to this server. Disable a username MerakiAdmin with privilege 15. Remove a route like ip route 0.0.0.0 0.0.0.0 [MerakiIP]*

---------------------------------------------------------------------------------------------------------------------------------------------------------------------

solara - 4 vlans : NOVA(2), FROSTLINE(4), ZEPHYRIA(6), CIPHER(8)
lyxis - 3 vlans : NEXZONE(11), OBSCURON(22), PULSE(33) (+mngmt vlan pt ssh)

---------------------------------------------------------------------------------------------------------------------------------------------------------------------

Subiectul 1 - Configurații de baza

Realizează pe toate echipamentele, mai puțin pe cele Virex, următoarele configurații de bază:
Hostname-ul corespunzător numelui/etichetei din topologie.
Un banner cu mesajul: „IF VIREX ASKS, WE ARE ON THEIR SIDE... FOR NOW”.
Parola către modul Privileged EXEC: NeverCATCHme.
Dezactivează căutarea numelui de domeniu.
La final, activează serviciul de criptare al parolelor.

Pentru router-ele din zona HEX CORE seteaza parola pe linia de consolă 0: GO_aWAY_Virex.

Subiectul 2 - Adresare IPv4/Subnetare optima
Pentru zonele: SOLARA, LYXIS si HEX CORE, sa se subneteze VLSM spatiul de adrese in care se regaseste urmatoarea adresa ip: 101.36.222.166/21
Districtele din SOLARA vor avea nevoie de:
NOVA: 401 hosturi
FROSTLINE: 127 hosturi
ZEPHYRIA: 511 hosturi
CIPHER: 15 hosturi

Districtele din LYXIS vor avea nevoie de:
NEXZONE: 56 hosturi
OBSCURON: 166 hosturi
PULSE: 63 hosturi
MNGMT: 3 hosturi

OBSERVATIE! 
In cele doua clustere nu configurati inca default gateway-urile, vor fi configurate in alt subiect;
In cluster-ul LYXIS nu se vor atribui adrese ip end device-urilor intrucat se vor atribui dinamic cu ajutorul dhcp
EXEMPLU pentru SOLARA:
Spatiu de adrese: 10.0.0.0/24
Laptop admin: 10.0.0.1/24
SOL-endDevice0: 10.0.0.2/24
SOL-endDevice1: 10.0.0.3/24
etc.
Default Gateway: 10.0.0.254/24

EXEMPLU pentru LYXIS:
Spatiu de adrese: 10.0.0.0/24
Default Gateway: 10.0.0.254/24

Ordinea de subnetare a retelelor din zona de HEX CORE este urmatoarea:
ATENTIE! Router-ul care va lua prima adresa din spatiu va fi cea cu indexul mai mic, spre ex: dintre HEXCoreRouter_1 si HEXCoreRouter_2, HEXCoreRouter_1 va lua prima adresa iar HEXCoreRouter_2 a doua.
HEXCoreRouter_1-HEXCoreRouter_2: 2 hosturi
HEXCoreRouter_2-HEXCoreRouter_5: 2 hosturi
HEXCoreRouter_5-HEXCoreRouter_6: 2 hosturi
HEXCoreRouter_6-HEXCoreRouter_4: 2 hosturi
HEXCoreRouter_4-HEXCoreRouter_3: 2 hosturi
HEXCoreRouter_3-HEXCoreRouter_1: 2 hosturi
HEXCoreRouter_1-HEXCoreRouter_6: 2 hosturi
HEXCoreRouter_2-HEXCoreRouter_4: 2 hosturi
HEXCoreRouter_3-HEXCoreRouter_5: 2 hosturi

Ordinea de subnetare pentru retelele catre colonii:
HEXCoreRouter_1-PathToSOLARA: 2 hosturi cu prima adresa la PathToSOLARA
PathToSOLARA-ArrivedAtSOLARA: 2 hosturi cu prima adresa la ArrivedAtSOLARA
HEXCoreRouter_2-PathToLYXIS: 2 hosturi cu prima adresa la PathToLYXIS
PathToLYXIS-ArrivedAtLYXIS: 2 hosturi cu prima adresa la ArrivedAtLYXIS
HEXCoreRouter_3-PathToOBSIDIA: 2 hosturi cu prima adresa la PathToOBSIDIA
PathToOBSIDIA-ArrivedAtOBSIDIA: 2 hosturi cu prima adresa la ArrivedAtOBSIDIA
HEXCoreRouter_4-PathToNIXUS: 2 hosturi cu prima adresa la PathToNIXUS
PathToNIXUS-ArrivedAtNIXUS: 2 hosturi cu prima adresa la ArrivedAtNIXUS
HEXCoreRouter_5-PathToSKYLON: 2 hosturi cu prima adresa la PathToSKYLON
PathToNIXUS-ArrivedAtSKYLON: 2 hosturi cu prima adresa la ArrivedAtSKYLON
HEXCoreRouter_6-PathToDARKWELL: 2 hosturi cu prima adresa la PathToDARKWELL
PathToNIXUS-ArrivedAtDARKWELL: 2 hosturi cu prima adresa la ArrivedAtDARKWELL



Pentru cluster-ele SKYLON si DARKWELL veti avea spatiul urmator: 179.69.22.0/23
SKYLON(zona dintre routere si ArrivedAtSKYLON): 4 hosturi
SKYLON(zona echipamentelor wireless): 126 hosturi
DARKWELL: 31 hosturi
SKYLON(zona wired, dintre routere si home router): 63 hosturi

EX pentru SKYLON:
Spatiul de adresare: 10.0.0.0/24
SKY-Router0: 10.0.0.1/24
SKY-Router1: 10.0.0.2/24
SKY-Router2: 10.0.0.3/24

ATENTIE! End device-urile vor fi configurate prin dhcp la subiectul de wireless!

In zona dintre routere si ArrivedAtSKYLON <b>ordinea adreseleor ip</b> este urmatoarea: SKY-Router0, SKY-Router1, SKY-Router2, ArrivedAtSKYLON

Subiectul 3 - VLANs & Inter-VLAN routing

In cluster-ul SOLARA:
Creati si denumiti corespunzator urmatoarele VLAN-uri:
VLAN 2: NOVA
VLAN 4: FROSTLINE
VLAN 6: ZEPHYRIA
VLAN 8: CIPHER

HINT: Configurati legaturile dintre switch-uri si cele catre end devices corespunzator!

Pentru partea de inter-VLAN routing se va configura Router-On-A-Stick pe router-ul ArrivedAtSOLARA, de tinut minte ca default gateway-ul va fi ultima adresa din spatiul de adrese corespunzator.

In cluster-ul LYXIS:
Creati si denumiti corespunzator urmatoarele VLAN-uri:
VLAN 11: NEXZONE
VLAN 22: OBSCURON
VLAN 33: PULSE

HINT: Configurati legaturile dintre switch-uri si cele catre end devices corespunzator!
Pentru partea de inter-VLAN routing se va configura un Layer 3 switch, ArrivedAtLYXIS, de tinut minte ca default gateway-ul va fi ultima adresa din spatiul de adrese corespunzator.


Subiectul 4 - Dynamic Host Configuration Protocol
Avand in vedere ca cel mai apropiat server DHCP se afla in cluster-ul SOLARA, il veti cnfigura corespunzator astfel incat host-urile din LYXIS sa primeasca in mod dinamic adresa ip, masca de retea, default gateway-ul si adresa serverului de DNS.

Pentru fiecare vlan veti avea in vedere:
Pool Name: numele vlan-ului
Default gateway: ultima adresa din spatiul disponibil
DNS Server: adresa serverului de DNS din cluster-ul SOLARA
Start IP Address: prima adresa din spatiul disponibil
Maximum number of users: numarul maxim de utilizatori din retea -1 pt adresa default gateway

Bineinteles din moment ce serverul de afl ain alt cluster va trebui implementat serviciul de dhcp relay. 
ATENTIE! Adresarea cu ajutorul serverului DHCP nu va fi functionala pana cand va fi rezolvat subiectul care asigura conectivitatea intre retele!

Analog serverului din SOLARA, în OBSIDIA va fi configurat un server DHCP propriu, care va atribui dinamic parametrii IP dispozitivelor acelui cluster.


Subiectul 5 - Etherchannel
In cluster-ul OBSIDIA se va configura protocolul ethernet pe legaturile dintre switch-urile de layer 3 dupa instructiunile urmatoare:

Intre MSW0 si MSW1 va fi grupul 1 si se va folosi LACP
Intre MSW0 si MSW2 va fi grupul 2 si se va folosi LACP
Intre MSW1 si MSW2 va fi grupul 3 si se va folosi PagP
Intre MSW2 si MSW4 va fi grupul 1 si se va folosi LACP
Intre MSW1 si MSW3 va fi grupul 2 si se va folosi LACP
Intre MSW3 si MSW4 va fi grupul 3 si se va folosi PagP
Intre MSW1 si MSW4 va fi grupul 6 si se va folosi PagP
Intre MSW2 si MSW3 va fi grupul 6 si se va folosi PagP
Intre MSW3 si MSW5 va fi grupul 1 si se va folosi LACP
Intre MSW4 si MSW6 va fi grupul 2 si se va folosi LACP

De asemenea configurati si viteza de 100Mbps pentru fiecare legatura

ATENTIE! Switch-ul care va initia legatura va fi cel cu index-ul mai mare. Spre ex: dintre MSW2 si MSW4, MSW4 va initia negocierea


Subiectul 6 - Debug - first wave -

Inainte sa veniti a incercat cineva sa configureze cluster-ul OBSIDIA pentru inter-vlan routing, insa au lasat mai degraba haos in urma. In schimb doua switch-uri au reusit sa fie configurate corect si anume ---- si ---- insa si router-ul ArrivedAtOBSIDIA. Uitati-va pe configuratiile sale plus description-urile lasate in running-config-ul fiecarui echipament

HINTS:
Exista trei vlan-uri:
STOCK - vlan 31
CYREX - vlan 32
MISTIS - vlan 33
In locurile incercuite cu rosu, pe legaturile trunk, vor fi permise doar vlan urile din acea zona.



Subiectul 6 - Debug - First Wave
Inaintea voastra, cativa roboti s-au oucpat cu pregatirea cluster-ului OBSIDIA, insa, vizibil, nu au facut o treaba foarte buna. Cand am inceput sa le corectam greselile, dupa ce am reusit sa punem configuratiile corecte doar pe switch-ul MSW6 si pe router-ul ArivedAtOBSIDIA, am constatat ca ne-ar lua prea mult timp sa facem asta pe fiecare echipament, asa ca de ce sa nu va lasam pe voi? 
Reparati configuratiile din cluster pe baza celor din MSW6 si ArrivedAtOBSIDIA, pe langa veti avea de configurat si server-ul de DHCP astfel incat host-urile sa primeasca adresare dinamica.



Subiectul 7 - LAN Security
In acest subiect se vor implementa mai multe masuri de securitate pe switch-urile din OBSIDIA:

Sa se activeze serviciul de port security pe toate legaturile switch-urilor catre end devices. De asemenea adresele MAC vor fi invatate dinamic in modul sticky, cu maximul de 1 per legatura. Se va tine cont ca in cazul in care nu se va respecta cerinta de securitate se va face un log iar violation counter-ul se va incrementa.
Trebuie creat VLAN-ul 99, denumit NATIVEVLAN, care va fi setat ca VLAN nativ.
Se va configura ip dhcp snooping, legaturile dintre switch-uri fiind considerate trusted, iar celelalte trebuie sa aiba o limita de 5 request-uri/s.
Trebuie configurat bpduguard si portfast.
Va fi activat serviciul de Dynamic ARP Inspection unde tot conexiunile dintre switch-uri vor fi considerate trusted.
La 6 minute de la invatarea unei adrese mac pe switch aceasta va fi stearsa.
Vor fi inchise toate porturile nefolosite si se vor trece in vlan-ul nefolosit 100 care trebuie denumit UNUSEDPORTS.


Subiectul 8 - Debug - second wave -
OH NU! Virex au inceput sa ne atace, au inceput prin a strica toate configuratiile din cluster-ul NIXUS. Grabeste-te sa corectezi toate greselile pana nu vor aparea altele. Se vor retine urmatorele specificatii spre a reusi sa duci acest task la final:

Trebuie sa existe patru VLAN-uri denumite pe switch-uri: 
VLAN 5 - QUANT
VLAN 21 - NEBULIX
VLAN 47 - THESIA
VLAN 55 - CHARON
VLAN-ul 55 este folosit doar ca sa ofere o oarecare conectivitate catre reteaua wireless condusa de NIX-HomeRouter
Legaturile dintre switch-uri vor fi configurate corespunzator iar pe conexiunile de tip trunk vor avea voie sa circule doar vlanurile din cluster. Toate celelalte port-uri sunt inchise si trecute in vlan-ul 100 numit UNUSEDPORTS care este si vlan-ul destinat nativ.

Subinterfetele router-ului trebuie sa corespunda cu id-ul vlan-ului pentru punctaj maxim.

Exista trei channel-group-uri intre:
SW0-MSW0, SW0 are port channel-ul 1 iar MSW0 10, folosesc LACP iar MSW0 initiaza conexiunea la viteza maxima
SW1-MSW0, SW1 are port channel-ul 2 iar MSW0 15, folosesc LACP iar SW1 initiaza conexiunea la viteza maxima
SW0-MSW0, SW5 are port channel-ul 1 iar SW3 6, folosesc PagP iar SW3 initiaza conexiunea la viteza maxima

Este asigurata si securitatea porturilor unde adresele mac sunt invatate dinamic iar in cazul in care regulile sunt incalcate este ales cel mai restrictiv mod pentru protejarea lor

Parolele pentru ca end device-urile sa se conecteze la APs si la HomeRouter sunt urmatoarele:
VLAN 21: SSID: NEBULIX PASS: CrazySeeingYouHere
VLAN 47: SSID: THESIA PASS: CantCrackThis
HomeRouter 2.4 GHz (conexiune pentru telefoane):  SSID: CHARON2.4GHz PASS: TryAndBreakThisPass-2.4
HomeRouter 5 GHz (conexiune pentru tablete):  SSID: CHARON5GHz PASS: TryAndBreakThisPass-5

IP-urile trebuie atribuite in urmatorul fel:
Def gateway: ultima adresa asignabila
EndDevice0: prima adresa asignabila
EndDevice1: a doua adresa asignabila
etc. ...

ATENTIE! STP ul ruleaza varianta de rapid-pvst, insa root-ul care trebuia ales initial a fost schimbat cu un alt switch. Sa se configureze prioritatea cea mai mica posibil pe switch-ul care ar trebui sa fie root-ul fara sa-l luati in considerare pe cel setat pentru acest task.


Subiectul 9 - Gateway Load Balancing Protocol
Configurati GLBP in cluster-ul SKYLON pe cele trei routere. Pe legatura catre SKY-SW0 va fi grupul 90 iar pe legatura catre SKY-MSW0 grupul 99. Prioritatile vor fi aceleasi in ambele grupuri si anume:
SKY-Router0: 66
SKY-Router1: 63
SKY-Router2: 60
Adresa ip virtuala trebuie sa fie ultima din spatiul de adresare disponibil.
Asigură-te că, atunci când routerul cu prioritatea cea mai mare revine în rețea după o cădere, acesta își recapătă automat rolul de gateway activ fără a fi nevoie de intervenție manuală.
Pe SKY-Router0 sa se configurează urmărirea interfetei gig0/0/1, ca în cazul în care aceasta cade, prioritatea să scadă cu 22. 



Subiectul 10 - Wireless

Tot in SKYLON se va seta router-ul SKY-HomeRouter astfel incat sa atribuie in mod dinamic echipamentelor adrese ip. 
Interfata WAN: a 4 a adresa
Interfata LAN: prima adresa
Start IP address: a doua adresa asignabila
Max number of users: 125
Sa se configureze canalul de 2.4GHz(destinat tabletelor) cu SSID-ul SKYLON_2.4GHz_Warriors, parola sa fie 1nvinc1bl3_4s_4lw4ys iar avertizarea beacon-urilor sa fie pasiva. De asemenea modul de securitate selectat sa fie WPA2-Personal cu incriptia AES.
In continuare sa se configureze canalul de 5GHz - 1 (destinat telefoanelor) cu SSID-ul SKYLON_5GHz_Warriors, parola sa fie mor3_th4n_1nv1ncibl3 iar avertizarea beacon-urilor sa fie pasiva. La fel, modul de securitate selectat sa fie WPA2-Personal cu incriptia AES.



Subiectul 11 - Open Shprtes Path First(OSPF)
Pentru a asigura conectivitatea intre toate cluster-ele se va configura protocolul OSPF. HexCORE va fi considerat aria de backbone (aria 0), router-ele sale fiind ABRs conectate catre celelalte arii (clusterele coloniilor) numerotate in topologie. 

Configurati OSPF cu numarul procesului de 6 pe toate router-ele cu Area 0 ca backbone prin adaugarea retetelor. Toate ariile non-backbone trebuie setate ca stub. !Link-urile interfetelor catre celelalte arii si cele seriale din HEXCore vor fi configurate drept point-to-point

Configurati urmatoarele router ID-uri:
HEXCoreRouter_1: 1.1.1.1, PathToSOLARA: 1.1.1.2, ArrivedAtSOLARA: 1.1.1.3
HEXCoreRouter_2: 2.2.2.1, PathToLYXIS: 2.2.2.2, ArrivedAtLYXIS: 2.2.2.3
HEXCoreRouter_3: 3.3.3.1, PathToOBSIDIA: 3.3.3.2, ArrivedAtOBSIDIA: 3.3.3.3
HEXCoreRouter_4: 4.4.4.1, PathToNIXUS: 4.4.4.2, ArrivedAtNIXUS: 4.4.4.3
HEXCoreRouter_5: 5.5.5.1, PathToSKYLON: 5.5.5.2, ArrivedAtSKYLON: 5.5.5.3, SKY-Router0: 5.5.5.4, SKY-Router1: 5.5.5.5, SKY-Router2: 5.5.5.6
HEXCoreRouter_6: 6.6.6.1, PathToLYXIS: 6.6.6.2, ArrivedAtLYXIS: 6.6.6.3

Configurati sumarizarea rețelelor fiecărei arii pe ABR-ul corespunzător.
EX:
Area 1: rețele 192.168.1.0/24 – 192.168.4.0/24 → sumarizare 192.168.0.0/22
Area 2: rețele 192.168.5.0/24 – 192.168.8.0/24 → sumarizare 192.168.4.0/22

Modificati intervalele de hello si dead astfel incat: pe interfetele ethernet hello sa fie 10s si dead 40s, iar pe cele seriale hello 6s si dead 20s.

Realizati autentificarea OSPF prin MD5 unde in fiecare arie cheia va fi Go4wayV1R3x.


Subiectul 12 - Server NTP

In clusterul SOLARA, serverul va fi configurat pentru a oferi servicii NTP.

Se va activa serviciul NTP pe serverul din SOLARA, cu data de 18 octombrie, ora 10:00 AM.
Autentificarea NTP se va face folosind o cheie comuna, cu numarul 1 si parola S0l4r4T1m3.
Pe toate routerele si switchurile din cluster se va seta ca sursa de timp serverul NTP din SOLARA.

La final, toate echipamentele trebuie sa afiseze ora sincronizata cu serverul NTP.

Subiectul 13 - Secure Shell(SSH)
In clusterul LYXIS veti configura SSH pe switch-urile atribuite VLAN-ului MNGMT. Aceste vor primi pe SVI urmatoarele adrese IP din spatiul dispoinibil in urmatoarea ordine: LYX-MSW0, LYX-SW0, LYX-MSW1.

Numele de domeniu: 0withoutLYXIS.local
Versiunea SSH: 2
Username-ul: hostname-ul echipamentului
Parola(secreta): LYX1Sh4sSW4GG
Dimensiunea cheilor RSA: 1024

ATENTIEEE! INTRE TIMP AI FOST FURAT SI DUS INTR-UNA DIN NAVELE VIREX, PANA NU LE VEI CONFIGURA RETEAUA NU TE VOR LASA SA PLECI

Subiectul 14 - Adresare IPv6

In zona VIREXCORE echipamentele vor lua adresele IPv6 in ordinea crescatoare a indexului lor:

-Intre VIREXCORE_0 si VIREXCORE_1 : dead:beef:6::/64
- Iar VIREXCORE_0 va primi adresa link-local de fe80::1 si VIREXCORE_1 fe80::2
-Intre VIREXCORE_0 si VIREXCORE_2 : dead:beef:66::/64
- Iar VIREXCORE_0 va primi adresa link-local de fe80::1 si VIREXCORE_2 fe80::2
-Intre VIREXCORE_1 si VIREXCORE_2 : dead:beef:666::/64
- Iar VIREXCORE_1 va primi adresa link-local de fe80::1 si VIREXCORE_2 fe80::2
-Intre VIREXCORE_0, VIREXCORE_1 si VIREXCORE_2 : dead:beef:6666::/64
- Iar VIREXCORE_0 va primi adresa link-local de fe80::1, VIREXCORE_1 fe80::2 si VIREXCORE_2 fe80::3


Reteaua dintre VIREXCORE_0 si Slobocity2.0 : c0de:1337:5::/64 unde VIREXCORE_0 - prima adresa si fe80::1 adresa link-local iar Slobocity2.0 - a doua adresa si fe80::2 adresa link-local

Reteaua dintre VIREXCORE_1 si SistemulVIREX : c0ff:ee:bad2:e24::/64 unde VIREXCORE_1 - prima adresa si fe80::1 adresa link-local iar Slobocity2.0 - a doua adresa si fe80::2 adresa link-local

Reteaua catre internet: c0ca:c01a:101::/64 unde VIREXCORE_2 ia prima adresa si fe80::1 link-local

Clusterul:
1.MYSTERION - spatiul de adrese: s10b:b4be:221F::/64 iar routerul Slobocity2.0 primeste prima adresa + cea de link-local fe80:1.

2. N-AI_TUPEU_VERE - spatiul de adrese: bad:1dea:ff96::/64 iar routerul SistemulVIREX primeste prima adresa + cea de link-local fe80:1.

Subiectul 15 - Dynamic Host Configuration Protocol IPv6 (DHCPv6)

In clusterul MYSTERION veti configura un server de DHCPv6 stateless pe routerul Slobocity2.0.
Pool-ul IPv6 se va numi Stateless_IPv6FaraNumar
Numele domeniului va fi GimmeMyMoney.local iar adresa serverului de DNS s10b:b4be:221F::66/64

In clusterul N-AI_TUPEU_VERE veti configura un server de DHCPv6 stateful pe routerul SistemulVIREX.
Pool-ul IPv6 se va numi Stateful_IPv6FaraNumar
Numele domeniului va fi  EmilutWasHERE.local iar adresa serverului de DNS bad:1dea:ff96::66/64



Subiectul 16 - Open Shortest Path First IPv6 (OSPFv3)

Un ultim task dat de VIREX este sa asigurati conectivitatea intre toate clusterele de pe nava folosind OSPFv3. 

Procesul OSPF va avea numarul 2 iar activarea retelelor se va face pe fiecare interfata in parte.
Veti configura urmatoarele router IDs:

VIREXCORE_0: 101.1.1.1
   Slobocity2.0: 101.0.0.1
VIREXCORE_1: 99.9.9.9
   SistemulVirex: 99.0.0.1
VIREXCORE_2: 102.0.0.0


Pe routerul VIREXCORE_2 veti adauga o ruta default catre Internet cu next hop-ul : c0ca:c01a:101::2/64 si o veti propaga in procesul OSPF.

Pe legaturile seriale se va dezactiva alegera DR-ului si a BDR-ului.

Pentru autentificarea OSPfv3 va veti folosi de IPsec. Veti avea in vedere urmatoarele:
Numele key-chain-ului: MatchaPePrimaverii
Numarul key-chain-ului va fi 1 si va avea un key-string faraJ*B
Algoritmul de criptare folosit va fi MD5.
Asociați key-chain-ul pe fiecare interfață cu comanda ipv6 ospf authentication ipsec spi 69 md5 MatchaPePrimaverii

Subiectul 17 - Tunelare IPv6
In cluster-ul hai_la_J*B veti configura tunelare IPv6. In retea sunt deja configurate adresele IPv4 dintre routere si cele IPv6 din LAN-uri. 
Configurati cate o ruta statica atat pe Ghinionul0 catre reteaua 2.6.2.4/30 cat si pe Ghinionul2 catre 2.6.2.0/30, astfel incat sa existe conectivitate intre ele
Sa se creeze interfata tunnel0 intre Ghinionul0 si Ghinionul2 iar adresele pentru fiecare vor fi:
Ghinionul0: 2001:db8:666::1/64
Ghinionul2: 2001:db8:666::2/64
!Nu uitati sa configurati sursa, destinatia tunelului cat si mode ipv6ip.


Subiectul <oricare e ultimul subiect:)> - DARKWELL

In ultimul cluster, DARKWELL, la care un utilizator cu privilegii normale precum voi nu are acces, nu a fost configurata autentificarea MD5 iar un router corupt de VIREX a reusit sa castige alegerea DR-ului. 

Traversati cluster-ul folosindu-va de SSH/Telnet pentru a va conecta de la dstanta la echipamentele de retea iar CDP pentru a descoperi device-urile aflate in apropiere.

Parola catre Privileged EXEC: D0ntC0m3Her3
Parola pe liniile vty: JustG0Aw4y96
Username-ul folosit pentru autentificarea SSH/Telnet va fi hostname-ul echipamentului.

Cand ajungeti in reteaua multiaccess in care este prezent router-ul D3f1n3tlyN0tC0r-r-rupt si este DR, trebuie sa navigati printre toate router-ele din acea retea si sa determinati care ar trebuit sa fie DR-ul in urma modificarilor facute de echipa astfel incat DR-ul sa nu mai fie router-ul corupt. Pe router-ul potrivit veti face prioritatea maxima pentru ca in urmatoarea alegere acesta sa fie cu siguranta noul DR. 


In continuare va trebui sa stergeti ruta default data pe unul dintre routere de un utilizator corupt de VIREX. Iar in final veti inchide link-ul catre router-ul corrupt.
