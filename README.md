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

---

## 4. Funktsionaalsed nõuded (Funktsionaalsed nõuded)
**Etapi eesmärk:** Määratleda süsteemi konkreetsed funktsioonid ja võimalused, mida platvorm peab kasutajale pakkuma.

* **Otsingumoodul:**
  * Süsteem peab võimaldama lennupiletite otsingut lähte- ja sihtkoha, kuupäevade ning reisijate arvu järgi.
  * Süsteem peab toetama mitmeetapilist (keerulist) marsruuti (Multi-city search).
* **Filtreerimine ja sorteerimine:**
  * Kasutajal peab olema võimalus filtreerida tulemusi hinna, ümberistumiste arvu, pagasi olemasolu, lennufirmade ja lennuaja järgi.
  * Süsteem peab võimaldama tulemuste sorteerimist hindade ("odavaim") ja lennukestuse ("kiireim") alusel.
* **Broneerimine ja isikuandmete haldus:**
  * Süsteem peab võimaldama reisijate isikuandmete sisestamist (sh OCR-skaneerimise tugi dokumentide automaatseks täitmiseks).
  * Süsteem peab toetama interaktiivset lennuki istmevaliku kaarti (Seat Map).
* **Maksesüsteem ja teavitused:**
  * Süsteem peab tagama turvalise makseviiside toe (Pangakaardid, Apple Pay/Google Pay, kohalikud pangalingid).
  * Süsteem peab automaatselt saatma broneeringu kinnituse ja e-pileti (PDF-vormingus) kasutaja e-postile.

---

## 5. Backlog (Toote arendusjärjekord / Story Points)
**Etapi eesmärk:** Struktureerida kõik süsteemi ülesanded (User Stories) ja hinnata nende keerukust abstraktses ühikus (Story Points), et planeerida sprintide mahtu.

* **Kogu projekti maht:** 120 Story Points (SP), mis on jaotatud 12 sprindi peale (keskmine kiirus ehk Velocity on 10 SP sprindi kohta).
* **Backlogi prioriteetsed eeposed (Epics):**
  1. **Core API & Integration (35 SP):** Väliste GDS/NDC API-de liidestamine, andmete agregateerimise ja puhverdamise süsteemi loomine (Go backend).
  2. **Search UI & SERP (25 SP):** Otsingumootori esiosa (frontend) disain ja reaalajas filtreerimise loogika rakendamine (React).
  3. **Booking & Payment Gateway (30 SP):** Reisijate andmete valideerimise vormid, istmete valiku moodul ja turvalise makselahenduse (PCI DSS) integreerimine.
  4. **User Profile & Notifications (15 SP):** Isikliku konto funktsionaalsus, ostuajalugu ja automaatne e-kirjade/SMS teavituste süsteem.
  5. **Testing & DevOps (15 SP):** Automaattestide kirjutamine (QA), koormustestimine ja CI/CD töövoogude seadistamine pilvekeskkonnas.

---

## 6. Meeskond (Projektitiim ja rollide jaotus)
**Etapi eesmärk:** Määratleda projekti elluviimiseks vajalikud spetsialistid ja nende vastutusvaldkonnad (kogu töömaht kokku: ~4120 töötundi).

* **Süsteemiarhitekt (System Architect) [320 tundi / 7.8%]:** Vastutab mikroteenuste arhitektuuri planeerimise, andmebaaside struktuuri ja turvastandardite eest.
* **Backend arendaja (Go Developer) [1440 tundi / 35.0%]:** Tegeleb äriloogika programmeerimise, väliste lennufirmade API-de integreerimise ja süsteemi kiiruse optimeerimisega.
* **Frontend arendaja (React Developer) [960 tundi / 23.3%]:** Vastutab kasutajaliidese (UI) ja interaktiivsete vormide (otsing, filtrid, broneerimisleht) sujuva toimimise eest.
* **Testimisinsener (QA Engineer) [720 tundi / 17.5%]:** Teostab funktsionaalset, integratsiooni- ja koormustestimist, et tagada süsteemi stabiilsus tippkoormustel.
* **DevOps insener [280 tundi / 6.8%]:** Seadistab pilveinfrastruktuuri, serverite skaleerimise reeglid ja tagab pideva tarne (CI/CD) protsessid.
* **Projektijuht / Scrum Master (PM) [400 tundi / 9.6%]:** Juhib Scrum-protsesse (igapäevased koosolekud, sprintide planeerimine), eemaldab takistused ja suhtleb sidusrühmadega.

---

## 7. Aeg (Ajakava ja sprintide jaotus)
**Etapi eesmärk:** Määratleda projekti ajaline raamistik ja jaotada backlogis kirjeldatud ülesanded etappide kaupa konkreetsetesse sprintidesse.

* **Projekti kogukestus:** 12 sprinti (kokku 24 nädalat / ~6 kuud).
* **Sprintide jaotus ja fookus:**
  * **Sprint 1–2 (Ettevalmistus ja Arhitektuur):** Nõuete täpsustamine, tehnilise lähteülesande kinnitamine, CI/CD baasinfrastruktuuri seadistamine ja andmemudeli loomine.
  * **Sprint 3–5 (Tuumarendus - Backend):** GDS/NDC API-de integreerimine, otsingualgoritmi programmeerimine ja andmete puhverdamise (caching) käivitamine.
  * **Sprint 6–8 (Kasutajaliides - Frontend):** Otsinguvormi, otsingutulemuste lehe (SERP) ja paindliku filtreerimissüsteemi arendus ning sidumine backendiga.
  * **Sprint 9–10 (Äriloogika lõpetamine):** Reisijate andmete sisestamise vormide, interaktiivse istmekaardi ja makselahenduse (Payment Gateway) integreerimine.
  * **Sprint 11 (Testimine ja optimeerimine):** Täielik integratsioonitestimine, turvaaudit (PCI DSS vastavus) ja koormustestimine (10 000+ üheaegset sessiooni).
  * **Sprint 12 (Juurutamine):** MVP lõppviimistlus, vigade parandus (bug fixing) ja süsteemi avalikustamine (Live-keskkonda viimine).

---

## 8. Raha (Eelarve ja finantsplaneerimine)
**Etapi eesmärk:** Arvutada projekti käivitamise ja arendamise kogukulu, võttes arvesse meeskonna töötasusid ja infrastruktuuri vajadusi.

* **Arendusmeeskonna töötasud (põhikulu):** Kulud põhinevad punktis 6 toodud töötundide jaotusel (~4120 tundi) ja spetsialistide keskmistel turupõhistel tunnitasudel.
  * *Backend & Arhitektuur:* Suurim eelarverida (~43% kogueelarvest) süsteemi tehnilise keerukuse ja API integratsioonide tõttu.
  * *Frontend & QA:* ~40% eelarvest kasutajakogemuse ja stabiilsuse tagamiseks.
  * *Juhtimine & DevOps:* ~17% eelarvest pilvehalduse ja Scrum-protsesside juhtimise jaoks.
* **Infrastruktuuri ja litsentside kulud:**
  * Pilveserverite rent (AWS / Google Cloud) arendus-, testimis- ja tootmiskeskkondade (Production) jaoks.
  * SSL/TLS sertifikaadid, API litsentsitasud (kui kohaldatakse) ja väliste teenuste (nt SMS-teavituste lüüsid) ettemaksud.
* **Riskifond:** Eelarvesse on planeeritud 10% reserv ootamatute tehniliste väljakutsete või uute nõuete tekkimise puhuks.

---

## 9. Tehnilised lahendused (Arhitektuur ja tehnoloogiastack)
**Etapi eesmärk:** Valida tehnoloogiad, mis tagavad punktis 3 esitatud mitte-funktsionaalsete nõuete (kiirus, turvalisus, skaleeritavus) täitmise.

* **Arhitektuuriline lähenemine:** Mikroteenuste arhitektuur (Microservices). Süsteem on jagatud iseseisvalt skaleeritavateks teenusteks (otsinguteenus, broneerimisteenus, makseteenus), mis suhtlevad omavahel läbi kiire gRPC protokolli.
* **Tehnoloogiastack:**
  * **Taustasüsteem (Backend):** **Go (Golang)**. Valitud tänu suurepärasele jõudlusele, madalale mälukasutusele ja võimekusele töödelda tuhandeid paralleelseid võrgupäringuid minimaalse viivitusega (<500 ms).
  * **Kasutajaliides (Frontend):** **React.js / TypeScript**. Tagab dünaamilise ja kiire liidese, mis võimaldab lennupiletite tulemusi reaalajas filtreerida ilma lehte uuesti laadimata.
  * **Andmebaasid ja puhverdamine:**
    * *PostgreSQL* — põhiandmebaas kasutajate andmete ja broneeringute turvaliseks säilitamiseks.
    * *Redis* — lennupiletite otsingutulemuste ajutiseks puhverdamiseks (caching), et vähendada päringute arvu lennufirmade serveritesse ja kiirendada korduvaid otsinguid.
  * **DevOps ja infrastruktuur:** **Docker** konteineriseerimiseks, **Kubernetes** mikroteenuste automaatseks skaleerimiseks ja **GitHub Actions** CI/CD automatiseerimiseks.

---

## 10. Ajagraafiku kujundamine (Gantti diagramm)
**Etapi eesmärk:** Visualiseerida punktis 7 kirjeldatud ajakava dünaamilise Gantti diagrammina, et jälgida ülesannete kattuvust, kriitilist teed ja meeskonna koormust reaalajas.

* **Kriitiline tee (Critical Path):** Projekti edu sõltub otseselt Backend API valmimisest (Sprint 3–5) ja makselahenduse integreerimisest (Sprint 9–10). Need etapid ei tohi viibida, kuna nendega on seotud kõik järgnevad frontend-arenduse ja testimise tegevused.
* **Ülesannete paralleelsus:** Gantti diagramm näitab, et alates 6. sprintist töötavad backend- ja frontend-meeskonnad paralleelselt. Sel ajal kui frontend loob otsingutulemuste liidest (SERP), viimistleb backend andmete puhverdamise süsteemi ja ettevalmistusi broneerimismootori jaoks.
* **Kontrollpunktid (Milestones):** Diagrammile on märgitud selged kontrollpunktid iga sprindi lõpus (Sprint Review), kus hinnatakse töötavat tarkvarajuppi, tagades projekti püsimise graafikus.

---

## 11. Agiilne modelleerimine (Burndown Chart ja Velocity)
**Etapi eesmärk:** Rakendada agiilseid raporteid meeskonna töökiiruse (Velocity) ja sprindi progressi jälgimiseks, tagades prognoositava ja läbipaistva arendusprotsessi.

* **Burndown Chart (Tööde põlemise graafik):**
  * *Ideaalne joon:* Sirgjoon, mis näitab ülesannete ühtlast kahanemist 120 Story Pointilt nulli 12. sprindi lõpuks (tempos 10 SP sprindi kohta).
  * *Reaalne joon:* Dünaamiline kõver, mis kajastab meeskonna tegelikku progressi. Graafik võimaldab varakult märgata riske (nt kui joon jääb ideaalsest kõrgemale, mis viitab takistustele arenduses) ja korrigeerida järgmiste sprintide mahtu.
* **Kiiruse (Velocity) juhtimine:** Iga sprindi lõpus analüüsitakse suletud ülesannete tegelikku mahtu. Kui meeskonna keskmine kiirus kõigub, kohandatakse backlogi prioriteete järgmiseks sprindiks, et tagada MVP õigeaegne valmimine ilma kvaliteedis järeleandmisi tegemata.

---

## 12. Disaini prototüüp ja disaini protsessid (UX/UI ja konversioonivonnel)
**Etapi eesmärk:** Luua toote interaktiivne disainiprototüüp ja analüüsida kasutajate teekonda (User Journey), et maksimeerida platvormi konversiooni ja mugavust.

* **UX/UI Prototüüpimine:** Figma keskkonnas on loodud kõrge täpsusastmega (High-Fidelity) interaktiivne prototüüp, mis katab kõik 7 põhilist kasutajaliidese ekraani (alates otsingust kuni edukat ostu kinnitava leheni). Prototüüp võimaldab testida reaalset kasutajakogemust enne koodi kirjutamist.
* **Disainiprotsesside efektiivsus (Konversioonivonnel / Conversion Funnel):**
  * Disaini tõhusust mõõdetakse läbi kasutajatestide ja simuleeritud konversioonilehtede teekonna: *Otsing $\rightarrow$ Tulemuste valik $\rightarrow$ Andmete sisestamine $\rightarrow$ Maksmine*.
  * *Kasutajamugavuse optimeerimine:* Prototüübi testimise käigus tuvastati, et reisijate andmete käsitsi sisestamine on peamine koht, kus kasutajad "tagasi tõmbuvad". Selle lahendamiseks integreeriti disaini dokumendituvastuse (OCR) funktsioon, mis täidab väljad automaatselt passi pildistamisel. See tehniline disainilahendus tõstab prognoositavat konversiooni andmete sisestamise etapis kuni 40%.
