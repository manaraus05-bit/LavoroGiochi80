# DECISIONS
## LavoroGiochi80 – Registro Decisioni Tecniche

Questo documento traccia le decisioni tecniche e di design prese dal team, con motivazione e alternative valutate.

---

## Template

```
### DEC-XXX – Titolo decisione
**Data:** YYYY-MM-DD  
**Decisore:** [nome/ruolo]  
**Stato:** Accettata | In discussione | Superata

**Contesto:** Perché era necessario prendere questa decisione.  
**Decisione:** Cosa è stato deciso.  
**Motivazione:** Perché questa scelta.  
**Alternative valutate:** Cosa è stato scartato e perché.  
**Conseguenze:** Impatti della decisione.
```

---

## Decisioni

### DEC-001 – Nessun framework o libreria esterna
**Data:** 2025  
**Decisore:** Team  
**Stato:** Accettata

**Contesto:** Scelta dello stack tecnologico per il progetto.  
**Decisione:** Sviluppo con vanilla JS, HTML5 Canvas, CSS3. Zero dipendenze.  
**Motivazione:** Progetto educativo, controllo totale sul codice, nessun build step, avvio immediato aprendo `index.html`.  
**Alternative valutate:**
- Phaser.js → troppo complesso per gli obiettivi didattici
- PixiJS → dipendenza non necessaria per questo scope
- React/Vue → del tutto fuori scope per un gioco Canvas  

**Conseguenze:** Più codice da scrivere, ma maggiore comprensione di ogni singola parte.

---

### DEC-002 – Canvas fisso 800×450 px
**Data:** 2025  
**Decisore:** Team  
**Stato:** Accettata

**Contesto:** Definizione delle dimensioni del canvas di gioco.  
**Decisione:** Canvas fisso 800×450 (16:9), centrato via CSS, `image-rendering: pixelated`.  
**Motivazione:** Semplicità, nessuna gestione resize/responsive nel MVP. Rapporto 16:9 compatibile con la maggioranza degli schermi.  
**Alternative valutate:** Canvas responsive → aggiunge complessità non necessaria nel MVP.  
**Conseguenze:** Post-MVP si potrà aggiungere scaling proporzionale.

---

### DEC-003 – Asse Y come profondità
**Data:** 2025  
**Decisore:** Programmatore  
**Stato:** Accettata

**Contesto:** Come simulare la profondità tipica dei beat 'em up senza 3D.  
**Decisione:** L'asse Y del canvas funge da asse di profondità. Y più alto = sfondo, Y più basso = primo piano.  
**Motivazione:** Standard del genere beat 'em up 2D classico. Semplice da implementare con z-sorting.  
**Alternative valutate:** True isometrico → troppo complesso per MVP.  
**Conseguenze:** Richiede z-sorting degli sprite prima del render.

---

### DEC-004 – Collisioni AABB semplici
**Data:** 2025  
**Decisore:** Programmatore  
**Stato:** Accettata

**Contesto:** Sistema di collisioni da implementare.  
**Decisione:** Axis-Aligned Bounding Box (rettangoli semplici) per tutte le collisioni.  
**Motivazione:** Semplice da implementare e debuggare. Sufficiente per il genere.  
**Alternative valutate:** Circle collisions, SAT → non necessarie per hitbox rettangolari.  
**Conseguenze:** Hitbox visivamente approssimata ma accettabile.

---

### DEC-005 – Personaggi e asset 100% originali
**Data:** 2025  
**Decisore:** Team  
**Stato:** Accettata

**Contesto:** Scelta su licenze e copyright del materiale di gioco.  
**Decisione:** Tutti i personaggi, nomi, asset grafici e audio devono essere originali o royalty-free (CC0).  
**Motivazione:** Evitare qualsiasi violazione copyright. Il progetto è ispirato al genere, non a un titolo specifico.  
**Alternative valutate:** Usare asset di Streets of Rage come placeholder → rischio legale, cattive abitudini.  
**Conseguenze:** Richiede tempo per la produzione di asset originali o ricerca di risorse CC0.
