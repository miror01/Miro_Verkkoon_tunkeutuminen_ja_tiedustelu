## a)

Aloitin tehtävän luomalla Mininet-koneen, kirjautumalla siihen sisälle ja ajamalla komennon `ip addr`. Mininetin ip-osoite on `192.168.56.101`. Avasin Kalin ja otin Mininettiin SSH-yhteyden komennolla `ssh -X mininet@192.168.56.101`

<img width="579" height="224" alt="h5_0" src="https://github.com/user-attachments/assets/f7e54a1f-cb16-44bb-9a35-c5ef30894a68" />

Nyt olemme sisällä minetissä. Syötin ensiksi komennon `ryu-manager ryu.app.simple_switch_13` joka käynnistää Ryu SDN-ohjaimen.

kuva

Se toimi. Avasin uuden terminaalin kalissa ja loin SSH-yhteyden uudestaan. Siellä käynnistin verkko-yhteyden ja 3 virtuaalista konetta komennolla `sudo mn --topo single,3 --mac --switch ovsk --controller remote`.

kuva

Kokeilin yhteydet komennolla `pingall` jossa kaikki pingit meni läpi, kun tuloste oli `Results: 0% dropped (6/6 received)`. Yritin avata koneen h1 terminaalin komennolla `xterm h1`, mutta sain virheilmoituksen. Löysin ratkaisuksi kokeilla aiempaa
"sudo mn" komentoa -E parametrilla, joten kokeilin sitä. Kirjoitin mininettiin komennon `exit` ja kokeilin komentoa uudestaan, nyt kaikki toimii. 

 kuva

 Avasin myös h3-koneen komennolla `xterm h3`. Mutta siirryin takas h1-terminaaliin. Ajoin siellä komennon `arp -n` MAC-osoitteiden selvittämiseksi. Sitten valmistelin h3-koneen komennolla `sysctl -w net.ipv4.ip_forward=1`. Se mahdollistaa
 IP-reitittämisen. Seuraavaksi ajoin komennot `iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-ports 22`, sekä `tcpdump -i h3-eth0 -w arp-p.pcap &`.

 kuva H5_5

 Sitten käynnistin wiresharkin komennolla `wireshark &`, ja painoin enteriä jolla wireshark aukesi, en löytänyt ohjeissa olevaa "h1_eth0" verkkoa, mutta ainoa vaihtoehto oli `h3_eth0`, joten valitsin sen. Palasin h3-terminaaliin ja ajoin komennon `sshesame -port 22 -listen_address 0.0.0.0`
 Avasin seuraavaksi uuden h3-terminaalin samalla komennolla `xterm h3`, kalin mininet terminaalissa, ei siis jo valmiiksi olevassa h3-terminaalissa. Uudessa h3-terminaalissa ajoin komennon `python3 ./arp_poison.py`, joka on itse hyökkäys.

 Seuraavaksi siirryin h1-terminaaliin, josta koitin ottaa ssh-yhteyttä h2:een. Sain ohjeissakin näkyvän tulosteen.

 h5_6

 Kaikki toimii.

 Seuraavaksi poistin vanhan avaimen h1:stä komennolla `ssh-keygen -R h2`, ja loin uuden yhteyden `ssh h2`. Nyt, h1 terminaalissa näyttää siltä, että olisi h2:ssa, ja näen tulosteen h3:n sshesamessa.

 h5_7

 Myös Wiresharkissa näkyy.

 h5_8

 Tässä siis h3 alkoi lähettää vääriä ARP-vastauksia h1:lle ja h2:lle, joissa se ilmoitti MAC-osoitteet väärin. Seurauksena h1 ja h2 tallensivat tiedot myös väärin, ja alkoivat lähettää liikennettä h3:n kautta. Kyseessä
 on tunnettu "Man in the Middle"-tyylinen hyökkäys. Kun loin h1:ltä yhteyttä h2:een, se menikin h3:n sshesamen kautta.
 
