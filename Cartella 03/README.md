
# Trigger `Thing`/FOI per `datastream` (SQLite/GeoPackage)

> Documentazione dei trigger:
> - `datastream_ai_set_guid_thing`
> - `datastream_au_set_guid_thing_on_foi_change`
> - `soilsite_au_sync_thing_name`
> - `soilprofile_au_sync_thing_name`
> - `soilderivedobject_au_sync_thing_name`

---

## 🧭 Introduzione

In questo schema GeoPackage/SQLite i trigger qui documentati automatizzano l’aggancio tra i `datastream` e i corrispondenti `thing` (nel senso SensorThings/IoT di “cosa osservata”).

L’obiettivo è doppio:

1. **Creare o riutilizzare on‑demand** il `thing` corretto non appena un `datastream` viene associato a un **FOI** (Feature of Interest) *unico* — `soilsite`, `soilprofile`, `profileelement` o `soilderivedobject` — evitando prepoulazioni inutili e duplicazioni.
2. **Impostare automaticamente** `datastream.guid_thing` **solo** quando è ancora al **placeholder** (default) per **non sovrascrivere valori impostati manualmente**.

La logica differenzia esplicitamente i **soil profile**:
- **observed** → risale a `soilsite` tramite `soilplot` e collega il `thing` del **sito**;
- **derived** → collega il `thing` del **profilo** stesso.

Per i `profileelement` si risale al relativo `soilprofile` e si applica la stessa regola (derived → profilo; observed → sito).

A corredo, tre trigger **di sincronizzazione del nome** mantengono `thing.name` allineato al `localid` di `soilsite`, `soilprofile` e `soilderivedobject` (fallback al `guid` se il `localid` è vuoto).

---

## 🔑 Concetti chiave

- **Cardinalità FOI**: la tabella `datastream` impone che i quattro campi FOI (`guid_soilsite`, `guid_soilprofile`, `guid_profileelement`, `guid_soilderivedobject`) siano popolati in conteggio **0 o 1**.
- **Placeholder `guid_thing`**: valore di default `a615db6e-383c-42b0-add3-5a9e333c0664`. I trigger su `datastream` **agiscono solo** se `guid_thing` è ancora uguale a questo valore.
- **Idempotenza**: la creazione dei `thing` usa `INSERT OR IGNORE` sul `guid` della sorgente; se la riga esiste già, l’INSERT non ha effetto.
- **Coerenza referenziale**: `datastream.guid_thing` è una FK verso `thing.guid`.

---

## 🗺️ Panoramica del comportamento

- Su **INSERT**/\**UPDATE** di `datastream`, se c’è **esattamente 1 FOI** e `guid_thing` è il **placeholder**:
  1. si **materializza** (se manca) il `thing` target in base al FOI;
  2. si aggiorna `datastream.guid_thing` con il GUID target calcolato (altrimenti resta il placeholder).
- Su **UPDATE** di `inspireid_localid` in `soilsite`, `soilprofile`, `soilderivedobject`: se esiste il relativo `thing`, si **aggiorna solo `thing.name`**.

---

## 🔁 Mappatura FOI → Thing

| FOI                       | Thing collegato (GUID) | Descrizione sintetica |
|---------------------------|------------------------|-----------------------|
| `soilsite`                | `thing.guid = soilsite.guid` | Area d’indagine del suolo (sito). |
| `soilderivedobject`       | `thing.guid = soilderivedobject.guid` | Oggetto spaziale con proprietà del suolo derivate. |
| `soilprofile` *(derived)* | `thing.guid = soilprofile.guid` | Profilo di riferimento non puntuale. |
| `soilprofile` *(observed)*| `thing.guid = soilsite.guid` (tramite `soilplot → soilsite`) | Il `thing` è il **sito** del rilievo. |
| `profileelement`          | se `isderived=1` → `thing` del **profilo**; se `isderived=0` → `thing` del **sito** (`soilplot → soilsite`). |

> **Nome e descrizione dei Thing creati**: `name = COALESCE(NULLIF(inspireid_localid,''), guid)`; `description` testuale coerente con il tipo (sito, profilo derived, derived object).

---

## 🔬 Dettaglio dei trigger

### 1) `datastream_ai_set_guid_thing` (AFTER INSERT su `datastream`)

**Condizioni**
- Dopo l’INSERT è valorizzato **esattamente 1** tra: `guid_soilsite`, `guid_soilprofile`, `guid_profileelement`, `guid_soilderivedobject`.
- `guid_thing = 'a615db6e-383c-42b0-add3-5a9e333c0664'` (placeholder).

**Azioni**
1. **Crea/riusa** il `thing` target in base al FOI (vedi mappatura sopra). 
2. **Aggiorna** `datastream.guid_thing` con il GUID target calcolato; se non determinabile, resta il placeholder (via `COALESCE`). L’UPDATE è circoscritto alla riga (`WHERE rowid = NEW.rowid`).

---

### 2) `datastream_au_set_guid_thing_on_foi_change` (AFTER UPDATE su `datastream`)

**Quando**
- Dopo un UPDATE di uno qualsiasi dei 4 campi FOI.

**Condizioni**
- Risulta **esattamente 1** FOI valorizzato.
- `guid_thing` è ancora il **placeholder**.

**Azioni**
- Replica la pipeline del trigger di INSERT: `INSERT OR IGNORE` del `thing` target e successivo `UPDATE` del `guid_thing`.

---

### 3) `soilsite_au_sync_thing_name` (AFTER UPDATE su `soilsite`)

**Quando**
- Dopo un UPDATE di `inspireid_localid` su `soilsite`.

**Azione**
- Se esiste `thing.guid = soilsite.guid`, aggiorna `thing.name = COALESCE(NULLIF(NEW.inspireid_localid,''), NEW.guid)`.

---

### 4) `soilprofile_au_sync_thing_name` (AFTER UPDATE su `soilprofile`)

**Quando**
- Dopo un UPDATE di `inspireid_localid` su `soilprofile`.

**Azione**
- Se esiste `thing.guid = soilprofile.guid`, aggiorna `thing.name` con la stessa regola del punto precedente. Tipicamente presente per **profili derived**.

---

### 5) `soilderivedobject_au_sync_thing_name` (AFTER UPDATE su `soilderivedobject`)

**Quando**
- Dopo un UPDATE di `inspireid_localid` su `soilderivedobject`.

**Azione**
- Se esiste `thing.guid = soilderivedobject.guid`, aggiorna `thing.name` come sopra.

---

## 🧪 Casi limite e note operative

- **0 FOI**: i trigger su `datastream` non scattano; `guid_thing` rimane al **placeholder**.
- **1 FOI & `guid_thing` ≠ placeholder**: nessuna azione (si rispetta il valore impostato manualmente).
- **Target non determinabile**: gli `INSERT OR IGNORE` non generano errore; l’UPDATE lascia il **placeholder**.
- **Idempotenza**: più inserimenti/aggiornamenti ripetuti non producono duplicati.
- **FK**: le chiavi esterne proteggono la coerenza (`datastream.guid_thing` → `thing.guid`).

---

## 🚀 Esempi rapidi

> Gli esempi sono dimostrativi: adatta i GUID alle tue istanze.

### Esempio A — FOI = `soilsite`
```sql
-- 1) Creo (o individuo) un soilsite
INSERT INTO soilsite (geometry, inspireid_localid, soilinvestigationpurpose)
VALUES (GeomFromText('POLYGON((...))', 3035), 'SS-001', '...');
-- Il trigger genera soilsite.guid se NULL.

-- 2) Creo un datastream con FOI=soilsite (guid_thing resta al default/placeholder)
INSERT INTO datastream (name, type, guid_sensor, guid_observedproperty, guid_soilsite)
VALUES ('T°C Suolo - Sito SS-001', 'Quantity', '...sensor-guid...', '...obsprop-guid...',
        (SELECT guid FROM soilsite WHERE inspireid_localid='SS-001'));

-- 3) Effetti:
--   - Se manca, viene creato thing.guid = soilsite.guid (name = 'SS-001' o guid)
--   - datastream.guid_thing è aggiornato con quel guid
```

### Esempio B — Profilo **observed**
```sql
-- Profile observed: thing = soilsite (via soilplot → soilsite)
INSERT INTO soilprofile (isderived, location)
VALUES (0, '...soilplot-guid...');

INSERT INTO datastream (name, type, guid_sensor, guid_observedproperty, guid_soilprofile)
VALUES ('Umidità - Profilo OBS', 'Quantity', '...sensor-guid...', '...obsprop-guid...',
        (SELECT guid FROM soilprofile WHERE ...));
-- Effetto: datastream.guid_thing diventa il guid dello soilsite raggiunto via soilplot.
```

### Esempio C — Sincronizzazione del nome
```sql
-- Aggiornando il localid di uno soilsite
UPDATE soilsite SET inspireid_localid = 'SS-001-REV' WHERE guid = '...';
-- Se esiste thing.guid = soilsite.guid, il trigger aggiorna thing.name = 'SS-001-REV'.
```

---

## ⚙️ Note di implementazione

- I trigger sono definiti come **AFTER** per operare con i valori finali e inserire/aggiornare record correlati nella **stessa transazione**.
- Gli UPDATE su `datastream` sono sempre circoscritti alla riga corrente (`WHERE rowid = NEW.rowid`).
- Le descrizioni testuali dei `thing` creati sono coerenti con il tipo (sito, profilo derived, derived object) e possono essere personalizzate nel DDL se serve.

---

## 📄 Autore e credito

- **Autore**: Andrea Lachi — CREA (Council for Agricultural Research and Economics)
- **Contatto**: andrea.lachi@crea.gov.it

> Questo README descrive la logica implementata nei trigger presenti nel DDL del template GeoPackage/SQLite per INSPIRE Soil.

