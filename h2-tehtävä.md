# H2

## x)
#### Pyramid of Pain
- Yksinkertainen kaavio näyttää yhteyden eri havaintotyyppeihin (indikattoreihin), joita voit käyttää vihollisen toiminnan havaitsemiseen
- Kertoo kuinka paljon "kipua" hän siitä kokee, kun onnistut poistamaan häneltä nämä indikaattorit

#### Diamond Model
- Kyberturvallisuuden alusta, joka auttaa organisaatioita analysoimaan kyberhyökkäyksiä
- Mallin esittivät ensimäisen kerran Sergio Caltagirone, Andrew Pendergast ja Christopher Betz Yhdysvaltain puolustusministeriön teknisessä raportissa vuonna 2013
- Raportin nimi oli Timanttimalli hyökkäysanalyysille

## a) Apache asennus
- Apache asennettiin komennolla
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

## c) Skriptit

## d) Jäljet lokissa

## e) Wire sharking

## f) Net grep

## g) Agentti

## h) Pienemmät jäljet

## i) LoWeR ChEcK


## Lähteet:
Karvinen, T. (2025). Verkkoon tunkeutuminen ja tiedustelu

https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

Bianco, D. (2013). The Pyramid of Pain

https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html

Caltagirone, S., Pendergast, A. & Betz, C. (2013). The Diamond of Intrusion Analysis

https://www.threatintel.academy/wp-content/uploads/2020/07/diamond-model.pdf
