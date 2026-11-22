## a) ARP-Hyökkäys

Aloitin tehtävän luomalla Mininet-koneen, kirjautumalla siihen sisälle ja ajamalla komennon `ip addr`. Mininetin ip-osoite on `192.168.56.101`. Avasin Kalin ja otin Mininettiin SSH-yhteyden komennolla `ssh -X mininet@192.168.56.101`

<img width="579" height="224" alt="h5_0" src="https://github.com/user-attachments/assets/f7e54a1f-cb16-44bb-9a35-c5ef30894a68" />

Nyt olemme sisällä minetissä. Syötin ensiksi komennon `ryu-manager ryu.app.simple_switch_13` joka käynnistää Ryu SDN-ohjaimen.

kuva

Se toimi. Avasin uuden terminaalin kalissa ja loin SSH-yhteyden uudestaan. Siellä käynnistin verkko-yhteyden ja 3 virtuaalista konetta komennolla `sudo mn --topo single,3 --mac --switch ovsk --controller remote`.

kuva

Kokeilin yhteydet komennolla `pingall` jossa kaikki pingit meni läpi, kun tuloste oli `Results: 0% dropped (6/6 received)`. Yritin avata koneen h1 terminaalin komennolla `xterm h1`, mutta sain virheilmoituksen. Löysin ratkaisuksi kokeilla aiempaa
"sudo mn" komentoa -E parametrilla, joten kokeilin sitä. Kirjoitin mininettiin komennon `exit` ja kokeilin komentoa uudestaan, nyt kaikki toimii. 

kuva

Avasin myös h3-koneen komennolla `xterm h3`. Mutta siirryin takas h1-terminaaliin. Ajoin siellä komennon `arp -n` MAC-osoitteiden selvittämiseksi. Sitten valmistelin h3-koneen komennolla `sysctl -w net.ipv4.ip_forward=1`. Se mahdollistaa IP-reitittämisen. Seuraavaksi ajoin komennot `iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-ports 22`, sekä `tcpdump -i h3-eth0 -w arp-p.pcap &`.

kuva H5_5

Sitten käynnistin wiresharkin komennolla `wireshark &`, ja painoin enteriä jolla wireshark aukesi, en löytänyt ohjeissa olevaa "h1_eth0" verkkoa, mutta ainoa vaihtoehto oli `h3_eth0`, joten valitsin sen. Palasin h3-
terminaaliin ja ajoin komennon `sshesame -port 22 -listen_address 0.0.0.0`
Avasin seuraavaksi uuden h3-terminaalin samalla komennolla `xterm h3`, kalin mininet terminaalissa, ei siis jo valmiiksi olevassa h3-terminaalissa. Uudessa h3-terminaalissa ajoin komennon `python3 ./arp_poison.py`, joka 
on itse hyökkäys.

Seuraavaksi siirryin h1-terminaaliin, josta koitin ottaa ssh-yhteyttä h2:een. Sain ohjeissakin näkyvän tulosteen.

h5_6

Kaikki toimii.

Seuraavaksi poistin vanhan avaimen h1:stä komennolla `ssh-keygen -R h2`, ja loin uuden yhteyden `ssh h2`. Nyt, h1 terminaalissa näyttää siltä, että olisi h2:ssa, ja näen tulosteen h3:n sshesamessa.

h5_7

Myös Wiresharkissa näkyy.

h5_8

Tässä siis h3 alkoi lähettää vääriä ARP-vastauksia h1:lle ja h2:lle, joissa se ilmoitti MAC-osoitteet väärin. Seurauksena h1 ja h2 tallensivat tiedot myös väärin, ja alkoivat lähettää liikennettä h3:n kautta. Kyseessä
on tunnettu "Man in the Middle"-tyylinen hyökkäys. Kun loin h1:ltä yhteyttä h2:een, se menikin h3:n sshesamen kautta.


## b) ICMP Spoofing ja TCP Session Hijacking

Tehtävä oli minulle todella haastava enkä tiennyt miten edetä, joten kysyin apua ChatGPT:ltä aloittamiseen. Sen ohjeiden mukaan:

Latasin ekana kalin sisältä moodlesta löytyvän "Minilabs nets - harjoitukset" Downloads hakemistooni. Siirsin sen seuraavaksi Mininet-koneelle komennolla `scp ~/Downloads/labs.tar.gz mininet@192.168.56.101:/home/mininet/`
Palasin Mininet-terminaaliini ja purkasin paketin komennolla `tar -xzf labs.tar.gz`. Sain onnistuneesti labrat Mininet-koneelleni, ja aloitin tehtävät

### ICMP Spoofing

Siirryin Mininet-koneella oikeaan hakemistoon komennolla `cd 01-Network-Security-Lab/`, Sieltä siirryin vielä scripts-hakemistoon komennolla `cd scripts`, ja ajoin komennon `sudo python3 hub_topo.py`. Seuraavaksi yritin avata h1 terminaalin komennolla `xterm h1`, mutta komento ei toiminut. Menin yhden vaiheen taaksepäin komennolla `exit` ja kokeilin uudestaan -E parametrillä, siis `sudo -E python3 hub_topo.py`. Nyt toimi. Seuraavaksi avataan h1 ja h2 terminaalit. Ajoin h1-terminaalissa komennon `python3 sniff_icmp.py`, ja h2-terminaalilla ajoin komennon `ping 10.0.0.1`. H2:n ping-tulosteet näkyi MYÖS h1-terminaalilla.

Seuraavaksi avasin h3-terminaalin ja suoritin siellä `python3 spoof_icmp.py`. h2-terminaalissa ajoin pingin uudestaan, ja näin paketit h3-terminaalissa. Ilmeisesti toimii.

h5_11

Tässä tapahtumaketju on siis: 
* h2 lähetti pingauspyynnön
* h1 havaitsee skriptin avulla ICMP-paketin
* h3 lähettää omalla skriptillään ICMP-paketin väärennettynä
* jonka johdosta vastaanottaja voi saada väärennetyn vastauksen

### TCP Session Hijacking

Avasin 3 uutta terminaalia, h1, h2, ja h3. h1-terminaalissa käynnistin TCP-palvelimen komennolla `python3 tcp_server.py`, ja h2-terminaalilla käynnistin h2:n palvelimen asiakkaaksi komennolla `python3 tcp_client.py`.
Terminaaleissa näkyy "Hello from Host 1" ja "Hello from Host 2", eli yhteys toimi.

h5_12

Seuraavaksi suoritin h3:lla komennon `sudo python3 sniff_tcp_session.py`. Loin yhteyden uudestaan h2-terminaalissa, ja nyt h3 näki myös tapahtumat. Yritin ajaa myös hijack-skriptin, mutta h1, ja h2 välisissä TCP-yhteyksissä yhteys sulkeutuu automaattisesti ensimmäisten "Hello" viestien jälkeen, joten en osaa toteuttaa sitä. En tiedä miten edetä.

h5_14

## c) TCP SYN-Flood

Tätä en enää saanut toimimaan yritin suorittaa hyökkäystä `02-SDN_DDoS_Simulation-tryout`-hakemistossa, mutta jumitti heti ekaan komentoon ja mikään ei tehoa.

h5_vika

Yritin myös toista terminaalia jossa suoritin komennon `PYTHONPATH=. ryu-manager main.py` ohjeiden mukaan, mutta se ei toiminut myöskään.

Hyökkäys tarkoittaa kuitenkin DDOS-hyökkäystä. Sen tarkoituksena on lähettää suuri määrä SYN-paketteja(aloituspaketteja) palvelimelle, muttei lähetä ACK-pakettia, jolla yhteys muodostettaisiin. Palvelimelle kasaantuu siis valtava määrä puoliksi avoimia yhteyksiä, jotka jumitta<vat palvelimen.

## Lähteet.

Mininet luento materiaali ja ohjeet (03-mininet.pdf) Ladattavissa Moodlen kurssisivuilta.

Network-Security-Lab (mininet) Luettavissa: https://github.com/ssam246/Network-Security-Lab Luettu 22.11.2026


