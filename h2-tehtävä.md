# H2

## x)
#### Pyramid of Pain
<img width="720" height="405" alt="image" src="https://github.com/user-attachments/assets/8391c65e-4c0e-4cd8-b472-5e37cff87a3b" />

- Yksinkertainen kaavio näyttää yhteyden eri havaintotyyppeihin (indikattoreihin), joita voit käyttää vihollisen toiminnan havaitsemiseen
- Kertoo kuinka paljon "kipua" hän siitä kokee, kun onnistut poistamaan häneltä nämä indikaattorit

#### Diamond Model
- Kyberturvallisuuden alusta, joka auttaa organisaatioita analysoimaan kyberhyökkäyksiä
- Mallin esittivät ensimäisen kerran Sergio Caltagirone, Andrew Pendergast ja Christopher Betz Yhdysvaltain puolustusministeriön teknisessä raportissa vuonna 2013, raportin nimi oli timanttimalli hyökkäysanalyysille


## a) Apache asennus
#### Apache asennettiin komennolla
```bash
sudo aupt-get install apache2
```
<img width="2085" height="608" alt="image" src="https://github.com/user-attachments/assets/bbdf315f-d294-4495-98c5-709cc84f902d" />

- Koska aiempaa kokemusta ei ollut, testasin palvelinta oletusweppisivulla osoitteessa `http://localhost`
<img width="1574" height="77" alt="image" src="https://github.com/user-attachments/assets/0fddb4da-5f26-4eee-b935-74d28fffaa67" />

#### Lokirivin analyysi

| Osa | Selitys |
| ------ | ----------- |
| 127.0.0.1   | IP-osoite |
| [29/Oct/2025:20:00:28 +0200] | Päivämäärä ja kellonaika, jolloin pyynto tehtiin |
| "GET / HTTP/1.1"    | HTTP-metodi (GET) ja pyydetty resurssi |
| 200    | Vastauskoodi: onnistunut pyyntö |
| 3383    | Vastauksen koko tavuina |
| Mozilla/5.0...   | Käytetty selain ja käyttöjärjestelmä |

## b) Nmapped
#### Nmapped suoritus komennolla
```
sudo nmap -A localhost
```
#### Tulokset
```bash
PORT    STATE SERVICE VERSION
80/tcp  open  http    Apache httpd 2.4.65 ((Debian))
|_http-server-header: Apache/2.4.65 (Debian)
|_http-title: Apache2 Debian Default Page: It works
631/tcp open  ipp     CUPS 2.4
|_http-title: Home - CUPS 2.4.10
|_http-server-header: CUPS/2.4 IPP/2.1
| http-robots.txt: 1 disallowed entry 
|_/
```
#### Selitykset
- 80/tcp open http, portti 80 (HTTP) on avoinna ja vastaa HTTP-liikenteeseen. Tarkoittaa, että web-palvelin toimii paikallisesti
- Apache httpd 2.4.65 ((Debian)) Nmap tunnisti palvelinohjelmiston ja version. 

## c) Skriptit
Omasta tuloksesta skripteiksi ajettiin:

- **80/tcp**
  - http-tittle
  - http-server-header

- **631/tcp**
   - http-title
   - http-server-header
   - httpr-robots.txt

## d) Jäljet lokissa
Tein haun `sudo tail -F /var/log/apache2/access.log` komenolla
  - Komento näyttää access.log-tiedoston 10 viimeistä riviä
Apache-lokeista löytyi selviä jälkiä Nmap Scripting Engine -skripteistä:
```bash
- - [30/Oct/2025:21:17:44 +0200] "OPTIONS / HTTP/1.1" 200 181 "-" "Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)"
127.0.0.1 - - [30/Oct/2025:21:17:44 +0200] "OPTIONS / HTTP/1.1" 200 181 "-" "Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)"
127.0.0.1 - - [30/Oct/2025:21:17:44 +0200] "GET / HTTP/1.1" 200 10977 "-" "Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)"
127.0.0.1 - - [30/Oct/2025:21:17:44 +0200] "OPTIONS / HTTP/1.1" 200 181 "-" "Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)"
127.0.0.1 - - [30/Oct/2025:21:17:44 +0200] "OPTIONS / HTTP/1.1" 200 181 "-" "Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)"
127.0.0.1 - - [30/Oct/2025:21:17:44 +0200] "OPTIONS / HTTP/1.1" 200 181 "-" "Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)"
127.0.0.1 - - [30/Oct/2025:21:17:44 +0200] "OPTIONS / HTTP/1.1" 200 181 "-" "Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)"
127.0.0.1 - - [30/Oct/2025:21:17:44 +0200] "OPTIONS / HTTP/1.1" 200 181 "-" "Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)"
```
- Sana "nmap" esintyy isolla kirjaimella
   - "Nmap Scripting Engine"

## e) Wire sharking
<img width="1622" height="548" alt="image" src="https://github.com/user-attachments/assets/1a833cd9-7c69-4047-9fab-3e868fb49797" />

- Nmapin NSE-skriptit
- Useat peräkkäiset HTTP-pyynnöt (GET, OPTIONS, POST) samalta IP:ltä eli localhost
  
## f) Net grep
Sieppasin verkkoliikenettä `ngrep` komenolla ja näytin kohdat, joissa on sana "nmap"
#### Terminaali 1 - ngrep-sieppaus:
```
sudo ngrep -d lo -i nmap
```
<img width="712" height="80" alt="image" src="https://github.com/user-attachments/assets/e7053aa8-f8ec-40b8-ad72-b57713a648b1" />

#### Terminaali 2 - nmap-ajo:
```
sudo nmap -A localhost
```
#### Tulokset
<img width="530" height="195" alt="image" src="https://github.com/user-attachments/assets/cb8a4210-3545-4b5e-87e2-21f651f88a39" />

- 2805 pakettia vastaanotettu, 54 kohdetta sisälsi sanan "nmap"

## g) Agentti
Vaihdoing user-agenttiä komenolla `nmap -A --script-args 'http.useragent=Mozilla/4.0 (Windows NT 11.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3' localhost`
#### Tulokset
<img width="1500" height="519" alt="image" src="https://github.com/user-attachments/assets/58a8279b-afb8-4fce-bd67-539877eb598a" />

## h) Pienemmät jäljet
Ajoin porttiskannaukset omalla user-agentillä komenolla `nmap -A --script-args 'http.useragent=Mozilla/4.0 (Windows NT 11.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3' localhost`
#### Tulokset
<img width="1511" height="414" alt="image" src="https://github.com/user-attachments/assets/20474f6f-d514-401d-87a1-ee8b13678427" />
<img width="1725" height="152" alt="image" src="https://github.com/user-attachments/assets/a06fd556-97b1-40c6-923b-17512db7254d" />

- Kun muutin User-Agentin, apache-lokista katosi 52 kohdetta, jotka sisälsivät sanan "nmap". Vertailun vuoksi f) Net grep-tehtävässä havaittiin aiemmin 54 vastaavaa osumaa
- Lokista ja kaapatusta liikenteestä löytyy kuitenkin edelleen Nmapin tuottamia URL-polkuja, kuten /nmaplowercheck1761995157



## i) LoWeR ChEcK
- Etsin nmap-tekstiä hakemistosta `/usr/share/nmap` komennolla:
<img width="1154" height="57" alt="image" src="https://github.com/user-attachments/assets/13316f91-891b-40e4-87f2-ac03b9e4e7d5" />

- Löysin tiedoston `nselib/http.lua`, joka sisälsi generoidun URL-polun:
<img width="959" height="142" alt="image" src="https://github.com/user-attachments/assets/1e54478b-0cd3-49ab-adfa-2c29d5dec8e7" />

- Muokkasin tiedostoa komennolla: `sudoedit ./nselib/http.lua`, korvasin kaikki "nmaplowercheck" esiintymät tekstillä "testcheck":
<img width="990" height="149" alt="image" src="https://github.com/user-attachments/assets/5bb05f5a-74f9-46be-a65f-50c029412443" />

- Testasin muutoksen suorittamalla porttiskannauksen uudelleen

#### Tulokset
<img width="1279" height="717" alt="image" src="https://github.com/user-attachments/assets/2f6c1e96-50f7-4b4d-90ab-31c4191acdaa" />

<img width="1419" height="998" alt="image" src="https://github.com/user-attachments/assets/f5ae3e4e-c7e6-4e9f-b685-c592b36362d9" />

<img width="383" height="36" alt="image" src="https://github.com/user-attachments/assets/15af024f-8b9d-42aa-a760-e5cc61248745" />

- Siepatussa verkkoliikenteessä ei näy "nmap" tekstiä
- Ainoastaan muokattu "testcheck" näkyy
- `net grep` tuloksessa 0 osumaa verrattuna tehtävässä h) oli 2 kohdetta

## Lähteet:
Karvinen, T. (2025). Verkkoon tunkeutuminen ja tiedustelu

https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

Bianco, D. (2013). The Pyramid of Pain

https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html

Caltagirone, S., Pendergast, A. & Betz, C. (2013). The Diamond of Intrusion Analysis

https://www.threatintel.academy/wp-content/uploads/2020/07/diamond-model.pdf

Nmap.org. (2025). Nmap Reference Guide

https://nmap.org/book/man.html

Nmap.org. (2025). Usage and Examples

https://nmap.org/book/nse-usage.html#nse-categories
