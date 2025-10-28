# H1 - Sniff

## x) Tiivistys
- Wireshark on työkalu verkkoliikenteen sieppaamiseen ja analysointiin
- Liittännän etuliite nykyaikaisissa Linux-järjestelmissä systemd-nimeämismallin mukaan
- Verkkoliitäntä on melkein sama kuin verkkokortti, mutta se voi olla myös virtuaalinen

## a) Linux asennus
<img width="1101" height="953" alt="image" src="https://github.com/user-attachments/assets/d5ed94a3-157a-4382-920d-88030d9106a8" />

## b) Internet-yhteyden katkaiseminen
Internet-yhteys oli katkaistettu ja yritin pingata testakseen yhteyden.
<img width="1011" height="257" alt="image" src="https://github.com/user-attachments/assets/8bff0fca-f4cf-4cdf-9dce-e65eed6afead" />

Yhteyden palautettua testasin pingia uudelleen ja tuloksena Internet-yhteys palasi.
<img width="1007" height="674" alt="image" src="https://github.com/user-attachments/assets/8bb6155b-f075-4f66-93ac-accaf84180bc" />

## c) Wireshark
[oma.zip](https://github.com/user-attachments/files/23097867/oma.zip)

## d) TCP/IP
TCP/IP neljä kerrosta
1) Application Layer
   - DNS
3) Transport Layer
   - UDP
5) Internet Layer
   - IPv4
7) Link Layer
   - Ethernet II
<img width="1801" height="707" alt="image" src="https://github.com/user-attachments/assets/e659ee3e-0993-4f0a-a244-f7de2abe10df" />

## e) Mitäs tuli surfattua?
<img width="1220" height="846" alt="image" src="https://github.com/user-attachments/assets/8d74e5aa-1be5-45c5-ad8e-a15f55b50bd2" />

- Kesto 7.5 sekuntia (2025-03-28 klo 17:28:09-17:28:16)
- Paketteja 283 kappaletta
- Siirtonopeus noin 129 kbit/s
## g) Minkä merkkinen verkkokortti käyttäjällä on?
<img width="1195" height="757" alt="image" src="https://github.com/user-attachments/assets/23cc96b2-fffb-4664-9361-18ee49eccc81" />
MAC-osoitteesta selviää käyttäjän verkkokortti (52:54:00:2f:e1:e5)

## h) Millä weppipalvelimella käyttäjä on surffaillut?
<img width="1763" height="785" alt="image" src="https://github.com/user-attachments/assets/fb40f1d6-e93a-4bfc-bc19-179f57a85ba4" />

Tarkastelemalla DNS-kyselyitä näkee, missä käyttäjä on surffaillut eli "google.com" ja "terokarvinen.com"

## i) Analyysi
<img width="1760" height="710" alt="image" src="https://github.com/user-attachments/assets/9471ab9c-59dd-4003-ab7b-3541f44dd43e" />

-  Tarkastelin omaa liikennettä ensimmäiset 14 framea
-  Tein terminaalissa komennon 'ping temu.com', jolloin tuli DNS-kyselyitä selvittääkseen IP-osoitteen
-  Sitten lähti pari ping-pakettia osoitteeseen '151.101.66.58' osoitteesta '10.0.2.15'
-  Vastaanotetut Echo Replyt osoittivat, että yhteys toimi ja vastausaika oli lyhyt


## Lähteet:
Karvinen, T. (2025). Verkkoon tunkeutuminen ja tiedustelu

https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

Karvinen, T. (2025). Wireshark - Getting Started

https://terokarvinen.com/wireshark-getting-started/

Karvinen, T. (2025). Network Interface Names on Linux

https://terokarvinen.com/network-interface-linux/
