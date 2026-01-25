# Linux-oikeudet, käyttäjät ja ryhmät

Tämä dokumentti selittää Linuxin tiedosto- ja hakemisto-oikeuksien perusteet selkeästi ja käytännönläheisesti.

---

## Käyttäjät ja oikeudet

Linuxissa jokaisella tiedostolla ja hakemistolla on:

- **owner** – omistajakäyttäjä
- **group** – omistajaryhmä
- **others** – kaikki muut käyttäjät

Oikeudet määritellään aina tässä järjestyksessä: `owner | group | others`


Vaikka järjestelmässä olisi `root` (vahvimmat oikeudet), tiedostojen
oikeudet silti määrittävät kuka saa lukea, kirjoittaa tai suorittaa tiedoston.

---

## Oikeustyypit

Linuxissa on kolme perusoikeutta:

| Numero | Kirjain | Merkitys |
|------|--------|---------|
| 4 | r | read (luku) |
| 2 | w | write (kirjoitus) |
| 1 | x | execute (suoritus) |

### Lunttilappu

Oikeudet saadaan laskemalla numerot yhteen:
```
4 = read (r)
2 = write (w)
1 = execute (x)
```

Esimerkkejä:
```
7 = 4 + 2 + 1 = rwx
6 = 4 + 2 = rw-
5 = 4 + 1 = r-x
4 = 4 = r--
```

---

# Kaksi tyypistä komento nimikettä

- chmod → muuttaa oikeuksia
- chown → muuttaa omistajaa ja ryhmää


## chmod – oikeuksien asettaminen

`chmod`-komennolla asetetaan tiedoston tai hakemiston oikeudet.

### Numeraalinen muoto

`$chmod 755 script.sh`

- Tarkoittaa:
  - owner: 7 → rwx
  - group: 5 → r-x
  - others: 5 → r-x

Visualisoituna:
```
rwx   r-x   r-x
 ^     ^     ^
owner group others
```

---

Toinen esimerkki:
`$chmod 754 file.txt`

- owner = 7 → rwx
- group = 5 → r-x
- others = 4 → r--


Symbolinen muoto

Samat oikeudet voidaan asettaa symboleilla: `$chmod u=rwx,g=rx,o=r file.txt`

- Missä:
  - u = user (owner)
  - g = group
  - o = others

--- 

Lisäesimerkkejä:
```
chmod o-r file.txt    # poista muilta lukuoikeus
chmod g+w file.txt   # lisää ryhmälle kirjoitusoikeus
```

---

### chown – omistajan ja ryhmän vaihto
- chown-komennolla vaihdetaan tiedoston omistaja ja/tai ryhmä.

```
chown user file.txt
chown user:group file.txt
```

Hakemisto rekursiivisesti: 
```
chown -R user:group folder/
```

⚠️ Vaatii yleensä root-oikeudet:
```
sudo chown user:group file.txt
```

---

### Oikeuksien tarkastelu

Oikeudet näkyvät komennolla: `$ls -l`

esim. visualisoituna:
```
-rwxr-xr-- 1 user group 1234 file.txt
```

Purettuna:
```
- rwx r-x r--
  ^   ^   ^
owner group others
```

Ensimmäinen merkki:
- - = tiedosto
  - d = hakemisto


---

## **Varoitus: `chmod 777`**
>
> `chmod 777` antaa **kaikille käyttäjille täydet oikeudet**
> (read, write ja execute).
>
> Tämä voi olla **merkittävä tietoturvariski**, koska kuka tahansa käyttäjä
> voi muokata tai suorittaa tiedostoa.
>
> Käytä `chmod 777` vain **väliaikaisesti testaukseen**, jos tiedät tarkalleen
> mitä teet – ja poista se heti käytön jälkeen.
>
> Useimmissa tapauksissa turvallisempi vaihtoehto on esimerkiksi:
> - `755` → skriptit ja hakemistot
> - `644` → tavalliset tiedostot

- ❌ `chmod 777` – lähes aina huono idea (liian avoin)

---

---

## Linuxin oikeusmalli – miksi käyttäjä, ryhmä ja muut?

Linux käyttää **owner / group / others** -oikeusmallia kaikille tiedostoille ja
hakemistoille. Tämä malli on peräisin Unix-järjestelmistä ja se on suunniteltu
monen käyttäjän ympäristöihin.

Mallin päätarkoitus on:
- estää käyttäjiä rikkomasta toistensa tiedostoja
- parantaa tietoturvaa
- mahdollistaa hallittu käyttö palvelimilla

---

## Sama malli tiedostoille ja hakemistoille

Sama oikeusmalli pätee sekä **tiedostoihin** että **hakemistoihin**, mutta
oikeuksien merkitys on erilainen.

### Tiedosto

| Oikeus | Merkitys |
|------|---------|
| r | saa lukea tiedoston sisällön |
| w | saa muokata tiedostoa |
| x | saa suorittaa tiedoston |

### Hakemisto

| Oikeus | Merkitys |
|------|---------|
| r | saa listata hakemiston sisällön (`ls`) |
| w | saa luoda ja poistaa tiedostoja |
| x | saa siirtyä hakemistoon (`cd`) |

Huomio:
- hakemistossa `x`-oikeus on usein tärkeämpi kuin `r`
- ilman `x`-oikeutta et pääse tiedostoon, vaikka tietäisit sen nimen

---

## Käyttäjä (owner)

- tiedoston alkuperäinen luoja tai omistaja
- vastaa tiedoston ensisijaisista oikeuksista
- omistaja voi muuttaa oikeuksia (`chmod`)

---

## Ryhmä (group)

- kokoelma käyttäjiä
- helpottaa oikeuksien hallintaa useille käyttäjille

Esimerkki:
- useita kehittäjiä samassa projektissa
- kaikki kuuluvat ryhmään `dev`
- tiedoston ryhmä on `dev`

Tällöin kaikilla ryhmän jäsenillä on samat oikeudet ilman erillistä säätöä.

---

## Others (muut käyttäjät)

- kaikki käyttäjät, jotka eivät ole omistaja tai ryhmän jäseniä
- usein rajoitetut oikeudet turvallisuuden vuoksi

Tyypillisesti:
- tiedostot: `r--`
- hakemistot: `r-x`

---

## Pätevätkö oikeudet aina?

- käyttäjän oikeudet → kyllä
- ryhmän oikeudet → kyllä
- others-oikeudet → kyllä

Poikkeus:
- `root` voi ohittaa oikeudet
- normaalit käyttäjät ja palvelut eivät voi

Tämän vuoksi oikeuksien asettaminen on tärkeää myös silloin, kun
järjestelmässä on root-käyttäjä.

---

## Muut oikeusmekanismit (lyhyesti)

Linuxissa on myös lisämekanismeja oikeuksien hallintaan:

- `umask` – uusien tiedostojen oletusoikeudet
- `setuid` / `setgid` – ohjelma ajaa omistajan tai ryhmän oikeuksilla
- `sticky bit` – estää muiden tiedostojen poiston (esim. `/tmp`)
- ACL (`setfacl`, `getfacl`) – tarkemmat käyttöoikeudet

Nämä täydentävät perusoikeuksia, mutta `chmod` ja `chown` ovat aina perusta.













