# H7 Aaltoja harjaamassa

## X) Lue ja tiivistä

## A) WebSDR

## B) rtl_433 - asennus
Aloitin tehtävän asentamalla `rtl_433` ohjelman komennoilla
```
sudo apt update
sudo apt install rtl-433
```

<img width="638" height="170" alt="image" src="https://github.com/user-attachments/assets/e79851aa-e617-47b9-b9d0-526c8ab5ef78" />

Asennuksen jälkeen varmistin, että ohjelma toimii ja on oikein asennettu. Suoritin versiontarkistuksen komennolla
```
rtl_433 -V
```

<img width="605" height="85" alt="image" src="https://github.com/user-attachments/assets/97cbb7e9-5ae1-484c-88ab-883ffde2324b" />

## C) Automaattinen analyysi
Aloitin selvittämällä, miten `rtl_433` ohjelmalla voidaan lukea tallennettu tiedosto.
```
rtl_433 -help
```
Ohjeesta löysin kohdan tiedoston lukemiseen

<img width="601" height="34" alt="image" src="https://github.com/user-attachments/assets/e4b35210-2d4a-4ced-ac7f-a6b7eb02f682" />

Seuraavaksi avasin tiedostoa komennolla
```bash
rtl_433 -r Converted_433.92M_2000k.cs8
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.083284s
model     : KlikAanKlikUit-Switch                  id        : 8785315
Unit      : 0            Group Call: No            Command   : Off
Dim       : No           Dim Value : 0
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.083284s
model     : Proove-Security                        House Code: 8785315
Channel   : 3            State     : OFF           Unit      : 3
Group     : 0
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.083284s
model     : Nexa-Security House Code: 8785315
Channel   : 3            State     : OFF           Unit      : 3
Group     : 0
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.163125s
model     : KlikAanKlikUit-Switch                  id        : 8785315
Unit      : 0            Group Call: No            Command   : Off
Dim       : No           Dim Value : 0
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.163125s
model     : Proove-Security                        House Code: 8785315
Channel   : 3            State     : OFF           Unit      : 3
Group     : 0
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.163125s
model     : Nexa-Security House Code: 8785315
Channel   : 3            State     : OFF           Unit      : 3
Group     : 0
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.242956s
model     : KlikAanKlikUit-Switch                  id        : 8785315
Unit      : 0            Group Call: No            Command   : Off
Dim       : No           Dim Value : 0
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.242956s
model     : Proove-Security                        House Code: 8785315
Channel   : 3            State     : OFF           Unit      : 3
Group     : 0
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.242956s
model     : Nexa-Security House Code: 8785315
Channel   : 3            State     : OFF           Unit      : 3
Group     : 0
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.383568s
model     : KlikAanKlikUit-Switch                  id        : 8785315
Unit      : 0            Group Call: No            Command   : Off
Dim       : No           Dim Value : 0
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.383568s
model     : Proove-Security                        House Code: 8785315
Channel   : 3            State     : OFF           Unit      : 3
Group     : 0
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
time      : @0.383568s
model     : Nexa-Security House Code: 8785315
Channel   : 3            State     : OFF           Unit      : 3
Group     : 0
```

### Analyysi 
- Sama signaali näkyy useana mallina (KlikAanKlikUit, Nexa-Security, Proove-Security)
- Kaikissa viesteissä toistuu sama `id / House Code: 8785315`
- Komento on joka kerralla OFF, mikä viittaa kaukosäätimen lähettämään ohjauskäskyyn
- Todennäköisin lähde on 433 MHz kaukosäädin

## D) Muunnos: complex16s ⮕ .cs8
Aloitin muuttamalla URH nauhoitetun tiedoston nimen `rtl_433` vaatiman formaatin mukaiseksi opettajan ohjeen mukaan
```
Tiedostomuodon muuttaminen 'urh' complex16s -> 'rtl_433' cs8
Vain tiedoston nimi muuttuu
Tiedoston nimessä pitää olla oikea taajuus (center frequency) ja näytteenottotaajuus (sample rate)
Erottimena alaviiva
foo_433.92M_1000k.cs8
File name meta data
```
Muutettu tiedostonimi:
```
Recorded-HackRF_433.92M_2000k.cs8
```
<img width="603" height="327" alt="image" src="https://github.com/user-attachments/assets/93e2dcd1-e222-4102-abf7-610b8fbca8d9" />

Tämän jälkeen avasin tiedoston komennolla
```
rtl_433 -r Recorded-HackRF_433.92M_2000k.cs8   
```

<img width="1067" height="318" alt="image" src="https://github.com/user-attachments/assets/3aa564da-7fcb-46ad-9e8d-aef3e9322819" />

Tulokset olivat täsmälleen samat kuin tehtävässsä C, mikä osoittaa, että kyseessä oli sama signaali. Muuutoksessa riitti siis vain tiedoston nimen mukauttaminen.


## E) Ultimate Radio Hacker (URH)

## F) Yleiskuva ja G) Bittitaaso



## Lähteet:
Karvinen, T. & Iso-Anttila, L. 2025. Verkkoon tunkeutuminen ja tiedustelu

https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/
