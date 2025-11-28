# H4 NFC ja RFID

## A) RFID-tuotteet ja oma suojautuminen

Käytän puhelimessa NFC:tä Apple Pay maksuihin. Apple Pay vaatii aina käyttäjän vahvistuksen (Face ID tai salasana) ennen maksua, joten maksutapahtuma ei käynnisty ilman lupaa. 
Puhelin ei anna lukijalle tietoja ennen vahvistusta, eikä kerättyjä maksutietoja voi käyttää verkkokaupoissa. Tämä tekee Apple Paysta turvallisemman vaihtoehdon fyysisille maksukorteille.
Passissa on RFID-siru, mutta se on erittäin vähäisessä käytössä. Nykyiset passit ovat hyvin suojattuja, sillä niiden siruja ei voi lukea ilman passin tunnistesivulla olevaa MRZ-koodia.

## B) APDU-komennot

APDU (Application Protocal Data Unit) on kortin ja päätelaitteen välinen viestimuoto. Päätelaite lähettää komento-APDU:n (C-APDU) ja kortti vastaa vastaus APDU:lla (R-APDU).

#### C-APDU rakenne:

| Osa        | Koko          | Kuvaus                                                  |
| ---------- | ------------- | ------------------------------------------------------- |
| **HEADER** | 4 tavua       | Pakollinen osa: CLA, INS, P1, P2                        |
| **CLA**    | 1 tavu        | Komennon luokka (loginen kanava, turvallinen viestintä) |
| **INS**    | 1 tavu        | Komennon koodi (ei saa olla 6x tai 9x)                  |
| **P1**     | 1 tavu        | Parametri 1                                             |
| **P2**     | 1 tavu        | Parametri 2                                             |
| **BODY**   | 0–65536 tavua | Valinnainen osa: Lc, Data, Le                           |
| **Lc**     | 1 tai 3 tavua | Lähetettävän datan pituus                               |
| **Data**   | Lc tavua      | Lähetettävä data                                        |
| **Le**     | 1 tai 3 tavua | Odotettu vastausdatan pituus                            |

#### R-APDU rakenne:

| Osa         | Kuvaus                                                                      |
| ----------- | --------------------------------------------------------------------------- |
| **Data**    | Vastausdata (valinnainen)                                                   |
| **SW1-SW2** | Status Word, 2 tavua, pakollinen – kertoo komennon onnistumisen tai virheen |

#### C-APDU tyypit (Case 1-4):

| Tyyppi | Rakenne                  | Kuvaus                             |
| ------ | ------------------------ | ---------------------------------- |
| Case 1 | CLA INS P1 P2            | Ei dataa lähetetä eikä odoteta     |
| Case 2 | CLA INS P1 P2 Le         | Ei dataa lähetetä, dataa odotetaan |
| Case 3 | CLA INS P1 P2 Lc Data    | Data lähetetään, ei dataa odoteta  |
| Case 4 | CLA INS P1 P2 Lc Data Le | Data lähetetään ja dataa odotetaan |

## C) RFID-hakkerointi

## Lähteet:
Karvinen, T. & Iso-Anttila, L. 2025. Verkkoon tunkeutuminen ja tiedustelu

https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/
