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

<img width="499" height="242" alt="h3_0" src="https://github.com/user-attachments/assets/f08d49c6-21cf-4b1b-83cc-c80468196582" />

<img width="551" height="242" alt="h3_1" src="https://github.com/user-attachments/assets/15b80b67-ea2e-4c66-b94a-7c61ee6e063b" />


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

Käytin kurssin kotisivujen tehtäväkohtaisista vinkeistä löytyvää ohjetta, eli asennus muodossa  
`sudo apt-get update

 sudo apt-get -y install pipx

 pipx install urh

 pipx ensurepath

sulje ja avaa terminaali

 urh

URH graafinen käyttöliittymä aukeaa`


