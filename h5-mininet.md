# H5 Laboratorio- ja simulaatioympäristöt hyökkäyksissä

## A) Tutustuminen Evilginx2

Aloitin asennuksen latamaalla evilginx2 komennolla 
```bash
git clone https://github.com/kgretzky/evilginx2
```
Ensimmäinen `go build` yritys epäonnistui, koska on ollut vanga Go-versio. Päivitin Go:n poistamalla sen ja asentamalla uuden version, sekä projektin riippuvuudet komennoilla `go mod tidy` ja `go mod vendor`. Tämä jälkeen `go build` onnistui ja sain projektin käännettyä

<img width="841" height="619" alt="image" src="https://github.com/user-attachments/assets/7b0b46b3-206f-4c4f-88d0-85eb1193cc09" />

Käännöksen jälkeen käynnistin ohjelman komennolla 
```
./evilginx2
```
<img width="922" height="702" alt="image" src="https://github.com/user-attachments/assets/03c32369-c25e-45bc-a647-ed7bf4802257" />

Tässä lyhyesti evilginx2 rakenteesta:

- Evilginx2:ssa *phishletit* ovat asetustiedostoja, jotka määrittelevät, miten työkalu jäljittelee palveluja ja käsittelee esimerkiksi kirjautumiseen liittyviä tietoja. Ne siis ohjaavat työkalun perustoimintaa

- Lisäksi työkalussa on *lures*-toimintoja, jotka liittyvät erilaisten houkuttelulinkkien tuottamiseen ja liikenteen ohjauksen hallintaan. Ne määrittävät, miten liikenne kulkee palvelimen läpi

En jatkanut työkalun kokeilemista, koska ei ole riittävää osaamista tällaisista työkaluista ja eettisistä syistä

## B) TCP SYN-Flood hyökkäys

#### Ensimmäisenä käynnistettiin Ryu-manager komennolla
```
ryu-manager ryu.app.simple_switch_13
```

<img width="514" height="88" alt="image" src="https://github.com/user-attachments/assets/3e47b9e3-fabe-4f9d-bea3-a8386148c7a2" />

#### Toisessa terminaalissa Mininet-verkko komennolla
```
sudo mn --topo single,3 --mac --switch ovsk --controller remote
```
<img width="739" height="380" alt="image" src="https://github.com/user-attachments/assets/fab80967-cf87-46be-8416-a6a3353a54dd" />

#### Testasin verkon yhteydet komennolla `pingall`

<img width="326" height="138" alt="image" src="https://github.com/user-attachments/assets/6b28db8f-48e7-4643-8159-12067b468709" />

#### X11-ongelman ratkaisu

Yritin avata terminaalin komennolla `xterm h1` mutta sain virheeksi `X11 connection rejected because of wrong authentication`  
Tämän korjaamiseen olen käyttänyt opettajan ohjeita:
```
Mininet ongelmien ratkomista
1. Jos Xterm auheuttaa ongelmia niin
2. Aja scripti ./get_xauth.sh
Se palauttaa jotain tämän näköistä
mininet-vm/unix:10 MIT-MAGIC-COOKIE-1 22ce67f9c6514c99d2903e2b9d97e496
3. Kopioi tämä ja aja seuraava komento
sudo -s xauth add mininet-vm/unix:10 MIT-MAGIC-COOKIE-1 22ce67f9c6514c99d2903e2b9d97e496
4. Tämän jälkeen xtermin pitäisi toimia mininetissä.
```
#### Virheen korjaamisen jälkeen sain kaikki terminaalit auki

<img width="1181" height="724" alt="image" src="https://github.com/user-attachments/assets/cabf47ad-d24c-4ffc-9a2c-d3d973852ca3" />

#### Terminaali **H1** (hyökkääjä) 
Käynnistin TCP SYN-Flood hyökkäyksen kohteeseen `10.0.0.2`
```
hping3 -S -p 80 --flood 10.0.0.2
```

<img width="443" height="104" alt="image" src="https://github.com/user-attachments/assets/c69464ce-0ac1-48b4-a987-cb5e81099523" />

#### Terminaali **H2** (uhri) 
HTTP-serverin käynnistys
```
python3 -m http.server 80
```

<img width="413" height="108" alt="image" src="https://github.com/user-attachments/assets/411ebcca-f446-4890-ad24-508879675b1c" />

#### Terminaali **H2** (Lisämonitorointi)
Ajoin komennon `tcpdump -i h2-eth0` nähdäkseen pakettien visualisoinnin suoraan

<img width="1285" height="744" alt="image" src="https://github.com/user-attachments/assets/ac165bb0-1e60-4038-88e8-b23c99ef84b9" />

#### Hyökkäyksen vaikutus

TCP SYN-Flood hyökkäys käynnistyi onnistuneesti ja **tcpdump** kaappausten perusteella kohde (h2) vastaanotti jatkuvasti suuria määriä paketteja. Käytännön vaikutukset jäivät kuitenkin vähäisiksi. Ping-viiveet pysyivät lähes samoina ennen ja jälkeen hyökkäyksen, eikä selkeää palvelun heikkenemistä havaittu

## Lähteet:
Karvinen, T. & Iso-Anttila, L. 2025. Verkkoon tunkeutuminen ja tiedustelu

https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

Kumar, S. 2025. A dataset for TCP-SYN flood DDoS attack detection in software-defined networks

https://www.sciencedirect.com/science/article/pii/S2352340925000460


