# Linux-käyttäjät, ryhmät ja palvelinoikeudet – käytännön esimerkki

Tämä README kuvaa, miten Linuxissa luodaan käyttäjiä ja ryhmiä, liitetään käyttäjiä ryhmiin ja rakennetaan realistinen palvelinrakenne (web, lokit ja käyttäjien uploadit).

---

## Käyttäjät ja roolit

Projektissa on kolme eri tasoista käyttäjää:

- **john_doe** – admin / ylläpitäjä  
- **jane_doe** – kehittäjä  
- **michael_doe** – vain lukuoikeudet  

---

## Ryhmät

Käytössä olevat ryhmät:

- **admin**
- **dev**
- **viewer**

Ryhmiä käytetään oikeuksien hallintaan käyttäjäkohtaisen säätämisen sijaan.

---

## Ryhmien luonti

```
groupadd admin
groupadd dev
groupadd viewer
```

---

## Käyttäjien luonti

``` 
useradd -m john_doe
useradd -m jane_doe
useradd -m michael_doe
```

Aseta salasanat:

```
passwd john_doe
passwd jane_doe
passwd michael_doe
```

---

## Käyttäjien lisääminen ryhmiin

``` 
usermod -aG admin john_doe
usermod -aG dev jane_doe
usermod -aG viewer michael_doe
```

Tarkista ryhmät:

``` 
groups john_doe
groups jane_doe
groups michael_doe
```

---

## Realistinen palvelinrakenne

Hakemistorakenne:

```
/srv/webapp
├── public/     # web-sivuston julkinen sisältö
├── uploads/    # käyttäjien lähettämät tiedostot
└── logs/       # sovelluksen lokit
```

---

## Omistajuus ja ryhmä

Projektin omistaja ja ryhmä:

- **owner:** john_doe  
- **group:** dev  

Aseta omistajuus:

``` 
chown -R john_doe:dev /srv/webapp
```

---

## Hakemistokohtaiset oikeudet

### public/ – web-palvelimen luettavaksi

``` 
chmod 755 /srv/webapp/public
```

- owner: rwx  
- group: r-x  
- others: r-x  

✔️ web-palvelin voi lukea  
❌ vain admin muokkaa  

---

### uploads/ – kirjoitusoikeus kehittäjille

``` 
chmod 770 /srv/webapp/uploads
```

- owner: rwx  
- group: rwx  
- others: ---  

✔️ john_doe ja jane_doe  
❌ michael_doe  

---

### logs/ – vain admin ja sovellus

``` 
chmod 750 /srv/webapp/logs
```

- owner: rwx  
- group: r-x  
- others: ---  

✔️ admin ja dev  
❌ muut käyttäjät  

---

## Käyttäjäkohtainen näkymä

### john_doe (admin)
- täysi hallinta palvelimeen  
- voi muuttaa oikeuksia ja omistajuuksia  
- pääsy kaikkiin hakemistoihin  

### jane_doe (dev)
- voi kirjoittaa uploads/  
- voi lukea public/  
- voi lukea logs/, mutta ei muokata  

### michael_doe (viewer)
- ei pääsyä palvelinhakemistoihin  
- vain mahdollinen luku dokumentaatioon (ei tässä esimerkissä)  

---

## Turvallisuushuomio

⚠️ Vältä chmod 777

chmod 777 antaa kaikille täydet oikeudet ja on merkittävä tietoturvariski palvelinympäristössä.

Käytä aina mahdollisimman rajoitettuja oikeuksia.

---

## Yhteenveto

- Käyttäjät luodaan useradd  
- Ryhmät luodaan groupadd  
- Käyttäjät liitetään ryhmiin usermod -aG  
- Oikeudet hallitaan ryhmien kautta  
- Palvelinrakenne pidetään selkeänä ja turvallisena  

---
