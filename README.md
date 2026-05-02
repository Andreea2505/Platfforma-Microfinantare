# README – Platformă de Microfinanțare

---

## I) Descriere proiect

Proiectul reprezintă o platformă de microfinanțare. Acesta gestionează modalitatea de finanțare a proiectelor, cât și planul de rambursare al sumelor necesare.

### Fluxul aplicației

Un beneficiar creează un proiect. Pentru proiectul respectiv se creează o cerere de finanțare, în cadrul căreia se stabilește dacă proiectul este eligibil de finanțare sau nu. Dacă proiectul este aprobat, primește finanțare si se creeaza documentele.

Investitorii sunt persoanele care finanțează proiectele. Un investitor poate investi în mai multe proiecte și un proiect poate fi finanțat de mai mulți investitori. După ce suficienți investitori fac investiții într-un proiect, se creează contractul, care este unic per proiect.
Fiecare investititie este intermediata de un agent,o entitate independenta,fiecare agent adaugand un comision propriu pentru realizarea tranzactiei.
Pentru suma din contract se stabilește planul de rambursare, plata sumei fiind împărțită în rate si acestea la randul lor in tranzactii.

---

## II) Descriere entități și atribute

### 1) Beneficiar

Persoana care solicită finanțare. Acesta creează proiectele.

**Atribute:**
- id_beneficiar (PK)
- nume
- email
- telefon

---

### 2) Investitor

Investitorul este persoana care investește în proiecte.

**Atribute:**
- id_investitor (PK)
- nume
- email

---

### 3) Proiect

Proiectul creat de beneficiar, pentru care se solicită finanțare.

**Atribute:**
- id_proiect (PK)
- titlu
- descriere
- id_beneficiar (FK)

---

### 4) CERERE_FINANTARE

Cererea formală de finanțare a proiectului.

**Atribute:**
- id_cerere (PK)
- data_cerere
- suma_solicitata
- statut_cerere (ex: in analiza, aprobata, respinsa)
- id_proiect (FK)

---

### 5) CONTRACT

Acordul financiar încheiat odată ce cererea de finanțare este aprobată și proiectul devine eligibil de finanțare.

**Atribute:**
- id_contract (PK)
- data_semnare
- dobanda
- durata_luni
- id_proiect (FK)

---

### 6) INVESTITIE

Contribuția unui investitor la un proiect eligibil de finanțare în baza contractului (tabel asociativ).

**Atribute:**
- id_investitor (PK, FK) – referință către investitor
- id_proiect (PK, FK) – referință către proiect
- id_agent (PK, FK) – referință către agent 
- suma_investita
- data_investitie

---

### 7) PLAN_RAMBURSARE

Planul de rambursare al sumei împrumutate pentru finanțarea unui proiect.

**Atribute:**
- id_plan (PK)
- suma_totala
- nr_luni
- id_contract (FK)

---

### 8) RATA

O plată individuală din planul de rambursare.

**Atribute:**
- id_rata (PK)
- suma
- data_scadenta
- statut (ex: platita, neplatita)
- id_plan (FK)

---

### 10) PROIECT_DOMENIU

Tabel asociativ dintre proiect și domeniu.

**Atribute:**
- id_proiect (PK, FK)
- id_domeniu (PK, FK)

---
### 11) TRANZACTIE-RATA
O rata poate fii platita integral sau in mai multe rate,fiecare fiind inregistrata ca o tranzactie.

**Atribute:**
- id_tranzactie (PK)
- suma_platita
- data_plata
- id_rata (FK)

---
### 12)DOCUMENT
Documentele asociate unui proiect dupa aprobarea acestuia.

**Atribute:**
- id_document (PK)
- tip_document
- nume_fisier
- data_incarcare
- id_proiect (FK)

---
### 13)AGENT
Persoana care intermediaza investitia.

**Atribute:**
- id_agent (PK)
- tip_agent
- comision

---

## III) Relații dintre entități

### 1) Beneficiar : Proiect (1:N)

- un proiect îi aparține unui singur beneficiar
- un beneficiar creează mai multe proiecte

---

### 2) Proiect : Cerere Finantare (1:1)

- unui proiect îi corespunde o singură cerere de finanțare
- o cerere de finanțare poate fi întocmită pentru un singur proiect

---

### 3) Proiect : Contract (1:1(0))

- fiecare contract aparține exact unui proiect
- nu toate proiectele au contract, deoarece contractul se întocmește odată ce un proiect este aprobat și nu toate proiectele sunt aprobate

---

### 4) Investitor : Investitie (1:N(0))

- un investitor poate face mai multe investiții sau niciuna
- o investiție îi aparține unui singur investitor
- (investitorii pot investi în mai multe proiecte)

---

### 5) Proiect : Investitie (1:N(0))

- fiecare proiect poate avea mai multe investiții
- fiecare investiție este pentru un proiect unic
- (mai mulți investitori pot finanța același proiect)

---

### 6) Contract : Plan Rambursare (1:1)

- unui contract îi este asociat un plan unic de rambursare
- fiecare plan de rambursare este creat pentru un contract unic

---

### 7) Plan Rambursare : Rata (1:N(1))

- un plan de rambursare este organizat în mai multe rate
- fiecare rată îi aparține unui plan de rambursare unic

---

### 8) Proiect : Domeniu (M:N)

Implementată prin tabelul asociativ PROIECT_DOMENIU, deoarece:

- unui proiect îi pot corespunde mai multe domenii
- într-un domeniu se pot încadra mai multe proiecte

---

### 9) Contract : Proiect (1:1)

- unui contract se creează pentru un singur proiect
- unui proiect îi corespunde un singur contract

---
### 10) Rata : Tranzactie_Rata (1:N)

- o rata poate avea mai multe tranzactii
- o tranzactie apatine unei singure rate 

---
### 11) Cerere_Finantare : Document  (1:N)

- o cerere de finantare poate avea mai multe documente
- un document ii corespunde unei singuri cereri de finantare 
---
### 11) Agent : Investite  (1:N)

- un agent poate intermedia mai multe investitii
- o investite poate fii intermediata de un singur agent 
---

## IV) Restricții impuse

- Un proiect nu poate avea contract dacă nu este finanțat complet
- Investițiile pot fi realizate doar după existența unui contract
- Suma totală a investițiilor pentru un proiect trebuie să fie egală sau mai mare decât suma necesară
- Numărul de rate trebuie să fie pozitiv și stabilit la crearea planului de rambursare
- Un proiect trebuie să aparțină cel puțin unui domeniu
- Nu pot exista investitii fara agent
---

## Diagrama ERD
<img width="827" height="569" alt="image" src="https://github.com/user-attachments/assets/29133238-1a96-487a-bba4-78485a32c8b6" />

## Diagrama conceptuala 
<img width="1008" height="702" alt="image" src="https://github.com/user-attachments/assets/6d13e7eb-ea67-48f9-b292-aff41e72e179" />


---
