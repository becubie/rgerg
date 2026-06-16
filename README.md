# FlyStream projekti dokumentatsioon

## 1. Planeerida projekt (Projekti planeerimine)
**Etapi eesmärk:** Määratleda projekti piirid, FlyStream lennupiletite agregaatori eesmärgid ja valida arendusmetoodika.

* **Toote kontseptsioon:** Kõrgjõudlusega lennupiletite agregaatori loomine, mis liidestub väliste jaotussüsteemidega (GDS/NDC) ja pakub nutikat filtreerimist.
* **Metoodika:** Projektijuhtimiseks on valitud agiilne raamistik **Scrum**. Arendustöö on jagatud fikseeritud ajaraamidesse: 12 kahenädalast sprinti.
* **Peamised tähised (Milestones):**
  * Sprint 1-2: Arhitektuuri projekteerimine ja tehnilise ülesande (lähteülesande) koostamine.
  * Sprint 3-9: Aktiivne arendusfaas (Backend API, UI-komponendid).
  * Sprint 10-11: Makselahenduste integreerimine ja koormustestimine.
  * Sprint 12: MVP stabiliseerimine ja toote avalikustamine (Release).

---

## 2. Kasutuslugude loomine (Use Cases)
**Etapi eesmärk:** Kirjeldada kasutajate ja süsteemi vahelisi interaktsioonistsenaariume, mis on aluseks rakenduse loogika disainimisel.

### Peamised osalejad (Actors):
* **Külaline** — sisselogimata kasutaja.
* **Klient** — registreerunud kasutaja, kellel on ligipääs isiklikule kontole (profiilile).
* **Süsteem (FlyStream)** — platvormi taustasüsteem (Backend).
* **Väline pakkuja (GDS/NDC API)** — lennufirmade kolmanda osapoole teenused.

### Peamised stsenaariumid (Use Case):
1. **Lennupiletite otsing:** Kasutaja sisestab otsingukriteeriumid $\rightarrow$ Süsteem pärib andmed välistest API-dest $\rightarrow$ Süsteem filtreerib, puhverdab (cache) ja kuvab tulemused.
2. **Broneerimine ja ostmine:** Kasutaja sisestab reisijate andmed $\rightarrow$ Süsteem blokeerib kohad pakkuja API kaudu $\rightarrow$ Kasutaja sooritab makse $\rightarrow$ Süsteem saadab e-pileti (broneeringu kinnituse) kasutaja e-postile.
3. **Pileti tagastamine/vahetamine:** Klient esitab tagastamistaotluse isiklikul kontol $\rightarrow$ Süsteem arvutab trahvid vastavalt lennufirma tariifile $\rightarrow$ Broneering tühistatakse ja raha tagastatakse.

---

## 3. Mitte-funktsionaalsed nõuded (Mitte-funktsionaalsed nõuded)
**Etapi eesmärk:** Määrata kindlaks süsteemi piirangud ning nõuded kvaliteedile, jõudlusele ja turvalisusele.

* **Jõudlus (Performance):** Süsteemi vastamisaeg otsingupäringule (sh andmete agregateerimine 5+ välisest allikast) ei tohi ületada **3,0 sekundit**. API sisemine viivitusaeg (Latency) tohib olla maksimaalselt **500 ms**.
* **Töökindlus ja kättesaadavus (Availability):** Süsteemi kättesaadavuse määr (Uptime) peab olema vähemalt **99,9%** (töörežiim 24/7/365).
* **Skaleeritavus (Scalability):** Arhitektuur peab taluma tippkoormust kuni **10 000 üheaegset otsingusessiooni** (Highload) ilma vastamisaja pikenemiseta.
* **Turvalisus (Security):** * Kõikide reisijate isikuandmete ja maksete edastus peab olema krüpteeritud, kasutades HTTPS (TLS 1.3) protokolli.
  * Süsteem peab vastama pangakaartide turvastandardile **PCI DSS**.
