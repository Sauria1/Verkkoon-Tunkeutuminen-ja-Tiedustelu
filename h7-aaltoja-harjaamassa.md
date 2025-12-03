# H7 Aaltoja harjaamassa

## X) Lue ja tiivistä
- Hubacek 2019: Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs (Video, alkaen 3:19 ja päättyen 7:40. Yhteensä noin 4 min.)
  - Signaalin tallentaminen ja tarkastelu URH:ssa alkaa taajuuden valinnalla ja lähetyksen havainnoinnilla
  - Ohjelma tunnistaa automaattisesti bittien pituuden ja muut parametrit modulaation valinnan jälkeen
  - Demoduloidun signaalin perusteella voidaan varmistaa paketit ja muuntaa data jatkokäyttöä varten heksadesimaaliseksi
- Cornelius 2022: Decode 433.92 MHz weather station data
  - Sensorin 433,92 MHz signaali on ASK / OOK-moduloitu, ja data lähetetään PDM-tyyppisesti: 500 µs carrier + 1000 µs tauko = 0, 2000 µs tauko = 1
  - Yksi datapaketti sisältää 36 bittiä, ja ne toistetaan useita kertoja lähetyksen aikana; data sisältää sensorin ID:n, akun tilan, kanavan, lämpötilan ja kosteuden

## A) WebSDR
Menin WebSDR sivustolle ja avasin NA5B WebSDR vastaanottimen selaimessa.
<img width="2094" height="173" alt="image" src="https://github.com/user-attachments/assets/8a9d3537-5149-430f-9f7f-92d2ce92ce0f" />

Valitsin MW-160 taajuusalueen ja liikuin vesiputousnäytöllä. Löysin voimakkaan signaalin noin 1310 kHz kohdalla ja klikkasin sitä, jolloin taajuudeksi tuli 1310 kHZ.
<img width="1271" height="723" alt="image" src="https://github.com/user-attachments/assets/37822b5f-47bc-4eac-9c8a-26d257f20508" />

Kun taajuus oli valittuna, WebSDR asetti automaattisesti amplitudimodulaation (AM), joka on tyypillinen yleisradiolähetykselle keskiaalloilla. Kuulin selkeää radio-ohjelmaa, joka sisälsi musiikkia ja puhetta. Lisätutkimuksen perusteella kyseessä on todennäköisesti WDCT AM 1310 "K-Radio" koreankielinen asema, joka lähettää Fairfaxista, Virginiasta (Yhdysvallat). Tämä selittää signaalin hyvän vastaanoton Pohjois-Amerikan WebSDR-vastaanottimmella

**Yhteenveto**
- Tajuus 1310 kHZ
- Modulaatio on AM
- Aallonpituus noin 229 m

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
Muutettu tiedostonimi
```
Recorded-HackRF_433.92M_2000k.cs8
```
<img width="603" height="327" alt="image" src="https://github.com/user-attachments/assets/93e2dcd1-e222-4102-abf7-610b8fbca8d9" />

Tämän jälkeen avasin tiedoston komennolla
```bash
rtl_433 -r Recorded-HackRF_433.92M_2000k.cs8   
rtl_433 version 25.02 (2025-02-19) inputs file rtl_tcp RTL-SDR SoapySDR
[Input] Test mode active. Reading samples from file: Recorded-HackRF_433.92M_2000k.cs8
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
Tulokset olivat täsmälleen samat kuin tehtävässsä C, mikä osoittaa, että kyseessä oli sama signaali. Muuutoksessa riitti siis vain tiedoston nimen mukauttaminen.

## E) Ultimate Radio Hacker (URH)
Yritin asentaa `Ultimate Radio Hacker` ohjelman opettajan antamien ohjeiden mukaan mutta asennus ei onnistunut. Käytetyt komennot olivat:
```
Helppo asennus, sopii valmiiksi kaapattujen signaalien analyysiin
$ sudo apt-get update
$ sudo apt-get -y install pipx
$ pipx install urh
$ pipx ensurepath
sulje ja avaa terminaali
$ urh
URH graafinen käyttöliittymä aukeaa
```
Tämän jälkeen kokeilin tunnilla näyttämää vaihtoehtoista asennustapaa. Asennus eteni muteen onnistuneesti, mutta komento `pip install .` antoi virheilmoituksen, jonka mukaan URHn kääntäminen edellyttää Cythonia. Asensin sen manuaalisesti, minkä jälkeen asennus toimi.
```bash
mkdir urh
virtualenv --system-site-packages env
source env/bin/activate
git clone --depth=1 https//github.com/jopohl/urh.git
cd urh/
pip install . (мirhe: "You need Cython to build URH's extensions!" )
pip install cython
pip install . (onnistui)
```
Asennuksen jälkeen URH käynnistyi normaalisti
<img width="1303" height="629" alt="image" src="https://github.com/user-attachments/assets/7b66ded1-dc14-4b8e-bdbd-888db69855b8" />

## F) Yleiskuva ja G) Bittitaaso
**Yleiskuva**
- Pituus 2.75 s
- Keskitajuus 433.912 MHz
- Nauhoituspäivä ja aika: 12.4.2025 klo 11:38:05
- Näyte sisältää kolme erillistä signaalipursketta, jotka ovat lähes identtisiä ja jokainen purske vastaa yhtä ON-napin painallusta Nexan kaukosäätimestä
<img width="2013" height="769" alt="image" src="https://github.com/user-attachments/assets/8d222a4a-0a97-4abb-bc3e-be5639e85e39" />

**Biittitaaso**
- Epäilen että oikea modulaatio on ASK (Amplitude Shift Keying), koska amplitudi vaihtelee bittien mukaan siten, että bitti 1 on ylhäällä ja bitti 0 alhaalla
- Yksi raakabitti on ajassa 261 μs
- Silmänräpäys kestää noin 100 ms, eli 261 μs on vain noin 0,26 % silmänräpäyksestä
<img width="1192" height="338" alt="image" src="https://github.com/user-attachments/assets/de2a2b31-48cd-4fbb-a31a-b204bb345d3f" />
<img width="1275" height="380" alt="image" src="https://github.com/user-attachments/assets/56673814-1b4b-4b86-9490-6b69378e786e" />

## Lähteet:
Karvinen, T. & Iso-Anttila, L. 2025. Verkkoon tunkeutuminen ja tiedustelu

https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/
