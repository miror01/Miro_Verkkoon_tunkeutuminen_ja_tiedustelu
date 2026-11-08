## x) Tiivistelmä

### Hubacek 2019: Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs

* Videolla käytetään SDR-vastaanotinta jonka avulla tallennetaan radiosignaaleja.
* Signaali analysoidaan URH-ohjelmalla. Ohjelma näyttää radiosignaalin visuaalisesti aaltoina ja "pulsseina".
* Signaalia yritetään purkaa tunnistamalla siitä raakabittejä, siis nollia ja ykkösiä.
* Tämän avulla, radiosignaalin lähettämää viestiä voidaan lukea.


### Cornelius 2022: Decode 433.92 MHz weather station data

* Artikkelissa käsitellään langattoman sääaseman ulkoanturin lähettämän signaalin purkamista.
* Saatiin selville käytetty taajuus: 433.92 MHz, ja se softaradion kanssa oli tarpeeksi aloittaakseen purkauksen.
* Käytetään ohjelmaa nimeltä `rtl_433` selvittääkseen signaalin viestiä. Se sai selville ilmankosteuden ja lämpötilan.
* Sitten artikkelissa käytetään URH-ohjelmaa (Universal Radio Hacker). Se saa selville signaalista, että se käyttää PDM-modulaatiota (Pulse Distance Modulation).
* Pulssien välisiä aikaeroja tutkimalla voidaan selvittää bittejä. Esimerkissä 1ms väli pulssien välillä tarkoittaa nollaa ja 2ms väli tarkoittaa ykköstä. Pidemmät välit tarkoittavat taukoa lähetyksessä.
* Näitä tutkimalla saadaan bittijonot, joita on helpompi tutkia tarkemmin.


## b) rtl_433

Asensin rtl_433:n Kaliin komennolla `sudo apt-get install rtl-433`. Seuraavaksi ajoin komennon `rtl_433` josta paljastui versio 25.12.

<img width="499" height="342" alt="h3_0" src="https://github.com/user-attachments/assets/f08d49c6-21cf-4b1b-83cc-c80468196582" />

<img width="551" height="342" alt="h3_1" src="https://github.com/user-attachments/assets/15b80b67-ea2e-4c66-b94a-7c61ee6e063b" />


## c) Automaattinen analyysi

Latasin Kalin kautta C-tehtävässä mainitun tiedoston. Siirryin kalissa Downloads-hakemistoon, jossa uusi ladattu tiedostoni sijaitsee, ja ajoin siellä analyysin komennolla `rtl_433 -r Converted_433.92M_2000k.cs8`.

<img width="706" height="365" alt="h3_2" src="https://github.com/user-attachments/assets/d1b5e81e-47d0-481c-a191-3d2b596a4a0b" />

Komento toimi. Ohjelma onnistui saamaan selville tallennetusta radiosignaalista erilaisia tietoja, esimerkiksi laitteen, protokollan, tilan, jne, esimerkiski "Off" tarkoittaa laitteen pois päältä kytkemistä.

## d) Too complex 16?

Asensin ensiksi samalla tavalla tehtävän tiedoston kurssin verkkosivuilta Kalin kautta, Kalin Downloads-hakemistoon.

Koska tehtävässä pitää muuttaa tiedosto rhl_433:n ymmärtämään muotoon nimetään se uudestaan. Downloads-hakemistossa kopioin tiedoston uudella nimellä komennolla `cp`.

Käytin komentoa `cp Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s Uusi_433.92M_2000k.cs8`. Tiedostonimessä täytyy olla taajuus jotta rtl_433 pystyy lukemaan sitä oikein.

Ajoin komennon `rtl_433 -r Uusi_433.92M_2000k.cs8`. Tulosteesta paljastui, että tiedoston nimen kanssa venkslaaminen toimi, ja saamme tulkittavaa dataa. Tuloste on identtinen aiemman tehtävän kanssa.

<img width="709" height="379" alt="h3_3" src="https://github.com/user-attachments/assets/581caaef-1c3f-445d-9003-6b2c3a8d8216" />

## e) Ultimate

Käytin kurssin kotisivujen tehtäväkohtaisista vinkeistä löytyvää ohjetta, eli asennus rivi kerralla muodossa:

<img width="676" height="229" alt="h3_4" src="https://github.com/user-attachments/assets/ec57fd22-09c0-4d23-a280-d3a866fed472" />

Ohjeiden mukaan suljin ja avasin terminaalin uudestaan, ja ajoin komennon `urh`. Pääsin ohjelman käyttöliittymään käsiksi.

## f) Yleiskuva

Latasin ohjeiden mukaan f- ja g- tehtäviin liittyvän tiedoston kurssisivuilta, ja tarkastelin sitä URH:ssa `File -> Open -> Tiedosto` kautta.

Tiedoston nimestä pystymme tulkitsemaan jo tallenteen tietoja:
* Taajuus on 433.912 MHz
* Näytteenottotaajuus on 2 MS/s
* Päivämäärä ja aika on 12.04.2025, klo 11:38:05

Silmämääräisesti näytteessä näyttäisi olevan 3 ryhmittymää, joissa on kaikissa samannäköiset 5 pulsseista muodostuvaa "palikkaa". Jokaista palikkaa erottaa pieni väli, eli tauko signaalin lähettämisessä. Nämä ovat niitä tehtävässä mainittuja kaukosäätimen ON-näppäimen painalluksia.

## g) Bittistä

Kun avasin tiedoston URH:ssa, asetukset olivat jo valmiina. Modulaatio oli jo ASK, ja "Samples/Symbol" oli 500.

Sain pienen näpräilyn jälkeen selville, että Samples total on 5,491,580, eli noin 5,5 miljoonaa näytettä.

Tässä kohdassa olin jo aika hukassa, joten kysyin ChatGPT:n GPT-6 mallia avuksi.

Bitit ja tauot näkyy signaalin alapuolella URH:ssa, kuten kuvasta näkee, ja koska näytteenottotaajuus on 2MSps, eli 2 miljoonaa näytettä sekunnissa, ja "Samples/Symbol" on 500, se tarkoittaa, että yksi symboli on 500 näytettä. Tästä tehdään simppeli laskukaava 500 jaettuna / 2 miljoonaa. Tulos on 0,00025 s joka muutetaan millisekunneiksi kertomalla tuhannella. siis 0,25ms. Yhden symbolin kesto on 0,25 millisekuntia, eli neljäsosa. Neljäsosa millisekunnista on 1/4000-osa sekunnista. Vertailun vuoksi, kameran salaman välähdys voi kestää noin yhden millisekunnin ajan. Tässä ajassa ehtii mennä vain 4 symbolia. Silmämme näkevät salaman tosin paljon pidempään.


<img width="948" height="365" alt="h3_5" src="https://github.com/user-attachments/assets/03da1c83-3083-44b8-b58b-b2a8a5b0b320" />



Myös asetuksesta vasemmalla näkee että Bits/Symbol: 1. Eli 1 bitti per symboli.




