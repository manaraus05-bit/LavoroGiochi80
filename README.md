# LavoroGiochi80
 
# Grafica e Art Direction

## Obiettivo
Realizzazione della componente grafica di un videogioco 2D beat ’em up ispirato ai classici arcade anni ’90, con particolare riferimento a titoli come Streets of Rage.

La produzione grafica è stata sviluppata con approccio iterativo, utilizzando strumenti di Intelligenza Artificiale come supporto per concepting, prototipazione rapida e generazione di sprite base successivamente adattati al contesto del gioco.

---

# Direzione Artistica

## Stile Grafico
- Pixel Art 2D side-scrolling
- Visuale laterale
- Palette con prevalenza di colori freddi e saturi
- Ambientazioni urban sci-fi
- Animazioni rapide e leggibili per migliorare il feedback visivo durante il gameplay
- Effetti visivi separati dagli sprite principali per facilitare layering e riutilizzo runtime

---

# Sprite Personaggi

## Risoluzione Base
- 48x48 px

## Player
Animazioni previste:
- Idle → 2 frame
- Walk Cycle → 4 frame
- Attacco Base → 3 frame
- Attacco Speciale → 4 frame
- Hit Reaction → 1 frame
- Death Animation → 4 frame

## Nemici Alieni
Animazioni previste:
- Idle / Walk
- Attacco melee con estensione innaturale degli arti
- Hit reaction
- Death / ragdoll animation

---

# Effetti Visivi (VFX)

Effetti realizzati come sprite separati in overlay:
- Shockwave da impatto
- Smear frame per enfatizzare velocità degli attacchi
- Effetti energia
- Polvere e impatti a terra

Questa scelta permette:
- maggiore modularità
- riutilizzo degli asset
- migliore gestione dei layer in-game
- scalabilità degli effetti via codice

---

# HUD

Elementi UI previsti:
- Barra vita
- Punteggio
- Contatore vite
- Barra energia speciale

---

# Ambientazioni

## Livello 1 — Strada Urbana
Strada notturna illuminata da lampioni con skyline cittadino sullo sfondo.

## Livello 2 — Metropolitana Abbandonata
Ambiente chiuso con illuminazione minima, palette fredda e dettagli neon.

## Livello 3 — Laboratorio Alieno
Scenario tecnologico e futuristico utilizzato per la produzione degli alieni, con forte contrasto estetico rispetto ai livelli precedenti.

---

# Pipeline Grafica

Workflow utilizzato:
1. Concept rapido tramite AI
2. Refinement manuale dello stile
3. Creazione sprite sheet
4. Separazione effetti VFX
5. Integrazione runtime con il motore di gioco Python

---

# Note Tecniche

- Asset progettati per scaling tramite nearest-neighbor interpolation
- Struttura sprite ottimizzata per animazioni frame-by-frame
- Effetti separati dal personaggio per evitare overlap tra frame
- Organizzazione asset pensata per futura estensione del progetto
3. **Laboratorio alieno**  
   Luogo in cui vengono prodotti gli alieni, con ambientazione tecnologica e futuristica, in contrasto rispetto ai livelli precedenti.
