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

Fig.1  
Questo pulsante è disponibile sia nella toolbar di digitalizzazione di QGIS ① che nel menu contestuale di ciascun layer ②.  
Inoltre, lo stesso pulsante—insieme ad altri strumenti utili all’editing—è presente direttamente all’interno dei form ③ e dei sotto‑form ④.

Questa configurazione permette un editing più rapido e completo di tutti gli elementi che compongono il GeoPackage.

::: important
Il pulsante **Toggle Editing Mode** nella toolbar principale di **Digitizing** abilita l’editing *del layer attualmente selezionato* nel pannello Layers.
:::

## Edit mode

Dopo aver cliccato il pulsante **Toggle Editing Mode**, il form entra in **editing mode**.

Fig.1  
Il pulsante rimane nello stato selezionato (pulsante premuto) ⑤ ⑥ a indicare che l’editing è abilitato, e un’icona a forma di matita ⑦ compare nel pannello Layers accanto al layer in modifica.

## Editing Operations

Quando la modalità di editing è attiva, il form consente di eseguire le seguenti operazioni:

### Update existing attribute values

In QGIS, l’aggiornamento dei valori attributi avviene direttamente tramite il form attributi, che si apre selezionando una feature e visualizzandone le proprietà.

Il form permette di modificare uno o più campi, applicando controlli, widget dedicati e vincoli definiti nella configurazione del layer.

### Add new records

Nuovi record possono essere creati utilizzando gli strumenti di digitalizzazione, che generano una nuova feature e aprono automaticamente il form attributi associato.

Per le tabelle prive di geometria, i nuovi record vengono aggiunti direttamente tramite il form, senza operazioni di digitalizzazione.  
Il form consente la compilazione di tutti i campi del modello dati, con widget, valori predefiniti, regole di validazione e controlli personalizzati.

### Delete features

La cancellazione delle feature in QGIS rimuove sia l’elemento geometrico sia i relativi attributi.  
Una volta selezionata una feature, può essere eliminata tramite lo strumento **Delete Feature**.

Questa operazione rimuove definitivamente il record dal dataset, impedendone la visualizzazione o l’uso in operazioni successive.

::: important
Le modifiche **non vengono scritte su disco immediatamente**.  
Rimangono in un buffer di modifica in memoria fino al momento del salvataggio esplicito: vedere [save](./save_form.md).
:::

::: note
Durante la **cancellazione di un record**, la base dati può applicare **regole di cascata** che rimuovono o aggiornano automaticamente record collegati in altre tabelle.

Questo può portare alla rimozione non intenzionale di ulteriori elementi, se la feature cancellata partecipa a relazioni foreign‑key o catene di dipendenze.

L’elenco completo delle regole di cascata applicate è disponibile qui:  
[../cascade_summary.md](../cascade_summary.md)
:::
