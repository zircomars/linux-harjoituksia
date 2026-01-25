## Esimerkki: eri tasoiset käyttäjät ja oikeusrakenne

Tässä esimerkissä luodaan kolme käyttäjää, joilla on eri roolit ja oikeudet:

- **john_doe** – admin / pääkäyttäjä projektissa
- **jane_doe** – kehittäjä
- **michael_doe** – vain lukuoikeudet

---

## Käyttäjät ja ryhmät

### Käyttäjät
 
- john_doe
- jane_doe
- michael_doe

### Ryhmät

- admin
- dev
- viewer


---

## Ryhmäjako

- `john_doe` → admin
- `jane_doe` → dev
- `michael_doe` → viewer

Tämä mahdollistaa oikeuksien hallinnan ryhmätasolla ilman
yksittäisten käyttäjien jatkuvaa säätöä.

---

## Projektihakemistorakenne

```
/project
├── bin/ # suoritettavat skriptit
├── src/ # lähdekoodi
└── docs/ # dokumentaatio
```


---

## Omistajuus ja ryhmät

Projektin omistaja ja ryhmä:

- owner: john_doe
- group: dev


Asetetaan omistajuus:

```
chown -R john_doe:dev /project
```


## Hakemistojen oikeudet

`src/` – kehittäjille kirjoitusoikeus

```
chmod 770 /project/src
```

- owner (john_doe): rwx
- group (dev): rwx
- others: ---
- ✔️ john_doe ja jane_doe
- ❌ michael_doe


## bin/ – suoritettavat tiedostot

```
chmod 755 /project/bin
```

- owner: rwx
- group: r-x
- others: r-x
- ✔️ kaikki voivat suorittaa
- ❌ vain admin muokkaa


## docs/ – vain luku muille

```
chmod 754 /project/docs
```

- owner: rwx
- group: r-x
- others: r--
- ✔️ michael_doe voi lukea
- ❌ ei voi muokata

---

## Käyttäjäkohtainen vaikutus

john_doe (admin)
- täysi hallinta kaikkiin hakemistoihin
- voi muuttaa oikeuksia ja omistajia

jane_doe (dev)
- voi muokata lähdekoodia (src/)
- voi lukea dokumentaatiota
- ei voi muuttaa oikeuksia

michael_doe (viewer)
- voi lukea dokumentaatiota (docs/)
- ei pääse lähdekoodiin
- ei voi muokata mitään

---

