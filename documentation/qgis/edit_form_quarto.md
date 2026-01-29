---
title: "Editing Records Through a QGIS Form"
format: gfm
toc: true
number-sections: true
execute: false
---

# Editing Records Through a QGIS Form

Per creare o modificare un record tramite un form di QGIS, è necessario attivare la modalità di editing usando il pulsante **Toggle Editing Mode**.

## Start Editing

:::{.columns}
::: {.column width="60%"}
![Fig.1 – Toggle Editing Mode](../assets/fig1.webp)
:::

::: {.column width="40%"}
**Fig.1 — Toggle Editing Mode**

Il pulsante è disponibile:

1. Nella toolbar di digitalizzazione di QGIS ①  
2. Nel menu contestuale del layer ②  
3. Direttamente nei form ③  
4. Nei sotto‑form ④  

Questa configurazione permette un editing più rapido e completo del GeoPackage.
:::
:::

::: important
Il pulsante **Toggle Editing Mode** nella toolbar principale di **Digitizing** abilita l’editing *del layer attualmente selezionato* nel pannello Layers.
:::

## Edit mode

:::{.columns}
::: {.column width="60%"}
![Fig.1 – Edit Mode attivo](../assets/fig1_edit.webp)
:::

::: {.column width="40%"}
**Fig.1 — Stato di editing attivo**

Quando l’editing è abilitato:
- il pulsante resta premuto ⑤ ⑥  
- compare un’icona a forma di matita ⑦ accanto al layer  
:::
:::

## Editing Operations

### Update existing attribute values

In QGIS, l’aggiornamento dei valori attributi avviene tramite il form attributi, che permette la modifica di uno o più campi, applicando:

- controlli
- widget personalizzati
- vincoli configurati nel layer

### Add new records

Nuovi record possono essere creati tramite:

- strumenti di digitalizzazione (con geometria)
- form diretto (senza geometria)

Il form applica valori predefiniti, regole di validazione e controlli coerenti col data model.

### Delete features

Quando una feature viene eliminata in QGIS, vengono rimossi:

- l’elemento geometrico  
- tutti i relativi attributi  

::: important
Le modifiche **non vengono salvate su disco subito**: restano nel buffer di editing finché non si effettua il salvataggio esplicito (vedi: ./save_form.md).
:::

::: note
La cancellazione di un record può attivare **regole di cascata** nella base dati, che rimuovono o aggiornano record correlati.  
L’elenco completo delle regole è disponibile qui:  
../cascade_summary.md
:::
