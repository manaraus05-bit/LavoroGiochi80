# 🎮 Gameplay Guide — Street of Iron

## Indice
1. [Obiettivo del Gioco](#obiettivo-del-gioco)
2. [Schermata di Gioco](#schermata-di-gioco)
3. [Il Personaggio](#il-personaggio)
4. [I Nemici](#i-nemici)
5. [Sistema di Combattimento](#sistema-di-combattimento)
6. [Sistema Combo](#sistema-combo)
7. [Attacco Speciale](#attacco-speciale)

---

## Obiettivo del Gioco

Sopravvivi il più a lungo possibile sconfiggendo ondate di nemici.

---

## Schermata di Gioco

```
┌──────────────────────────────────────────────────────────────────┐
│  P1 [████████████████████]                        000600        │  ← HUD
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│                        AREA SUPERIORE                            │
│                      (non raggiungibile)                         │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ORIZZONTE ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─   │
│                                                                  │
│                        AREA DI GIOCO                            │
│                                                                  │
│   [PLAYER]                              [ENEMY]    [ENEMY]       │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
  800px × 450px
```

### Elementi HUD
- **Barra verde (P1):** Punti vita del giocatore. Diminuisce quando vieni colpito, si svuota con l'attacco speciale.
- **Numero in alto a destra:** Punteggio corrente, sempre a 6 cifre (es. `000600`).

### Aree dello Schermo
| Area | Y (pixel) | Descrizione |
|------|-----------|-------------|
| HUD | 0 – 50 | Barra HP e punteggio, non interagibile |
| Superiore | 0 – 180 | Solo visiva, il player non può entrarci |
| Orizzonte | 180 | Linea di separazione, simula la profondità |
| Di gioco | 180 – 450 | Area dove si muovono player e nemici |

---

## Il Personaggio

Il giocatore controlla un personaggio rappresentato da un **rettangolo 50×100px**.

### Statistiche Base
| Statistica | Valore |
|------------|--------|
| HP Massimi | 100 |
| Velocità movimento | 4 px/frame |
| Velocità verticale | 2.4 px/frame (60% della velocità base) |
| Dimensioni | 50 × 100 px |

### Stati Visivi del Personaggio
| Stato | Colore | Quando |
|-------|--------|--------|
| Idle / Movimento | 🟦 Blu `#2980b9` | Nessun attacco in corso |
| Attacco Combo | 🟨 Giallo `#f1c40f` | Durante l'animazione del pugno |
| Attacco Speciale | 🟪 Viola `#9b59b6` | Durante l'animazione speciale |

---

## I Nemici

I nemici sono rappresentati da **rettangoli grigi 50×100px** con una barra vita rossa sopra.

### Statistiche Nemico
| Statistica | Valore |
|------------|--------|
| HP | 40 |
| Velocità inseguimento | 1.5 px/frame |
| Velocità verticale | 0.75 px/frame |
| Danno al giocatore | 5 HP per colpo |
| Cooldown tra colpi | 60 frame (~1 secondo) |
| Punti alla morte | 100 |

### Comportamento
I nemici inseguono sempre il giocatore cercando di posizionarsi al suo fianco (a circa 45px di distanza). Quando sono abbastanza vicini, infliggono danno automaticamente ogni secondo.

### Spawn dei Nemici
- **Inizio partita:** 2 nemici 
- **Quando tutti i nemici sono morti:** ne spawna 1 nuovo sul lato destro dello schermo a Y casuale
- I nemici non aumentano di difficoltà nel tempo

---

## Sistema di Combattimento

### Come Funziona un Attacco Base

1. Premi `A` mentre sei fermo o in movimento
2. Il personaggio si **blocca** 
3. Viene controllata la collisione con i nemici davanti a te
4. Se un nemico è nel range, subisce danno

### Range dell'Attacco
```
        Facing RIGHT →
        
   ┌──────────┐   ┌────────┐
   │  PLAYER  │──►│ RANGE  │
   │  50×100  │   │ 75×50px│
   └──────────┘   └────────┘
   
   Orizzontale: 75px davanti al giocatore
   Verticale:   25px sopra e sotto la posizione del giocatore
```

Il range cambia automaticamente in base alla direzione (`facing`): se sei rivolto a sinistra, l'attacco colpisce a sinistra.

---

## Sistema Combo

Premendo `A` più volte rapidamente si esegue una combo a 3 colpi con danno crescente.

```
  Premi A        Premi A           Premi A
     │              │                 │
     ▼              ▼                 ▼
  [Colpo 1]  →  [Colpo 2]  →  [Colpo 3]  →  [Colpo 1] ...
   10 danno      15 danno       20 danno      (ricomincia)

  ◄──────────── entro 40 frame ────────────►
         (≈ 0.66 secondi a 60fps)
```

### Regole della Combo
- Hai **40 frame** (circa 0.66 secondi) tra un colpo e l'altro per mantenere la combo
- Se aspetti troppo, il contatore torna a 0 e il prossimo colpo farà solo 10 danno
- La combo si ripete ciclicamente: dopo il 3° colpo ritorna al 1°

### Danno Combo
| Step | Danno | Come ottenerlo |
|------|-------|----------------|
| 1° colpo | **10 HP** | Sempre, primo A della combo |
| 2° colpo | **15 HP** | Premi A di nuovo entro 40 frame |
| 3° colpo | **20 HP** | Premi A una terza volta entro 40 frame |

---

## Attacco Speciale

L'attacco speciale è un'abilità ad alto rischio / alta ricompensa, ispirata direttamente al meccanismo originale di Street of Rage.

### Come Funziona
1. Premi `S` (devi avere più di 10 HP)
2. Il personaggio diventa **viola** e genera un'onda circolare
3. Tutti i nemici entro **120px** subiscono 20 danno e vengono **respinti**
4. Il giocatore perde **8 HP**

```
              120px
         ┌────────────┐
         │            │
         │  [PLAYER]  │  ◄── Area di effetto circolare
         │            │       Raggio: 120px
         └────────────┘
         
   Nemici nel raggio: -20 HP + knockback di 60px
   Giocatore: -8 HP (sempre, anche se non colpisci nessuno)
```

### Knockback
I nemici colpiti vengono spinti **60px** nella direzione opposta al giocatore. Questo crea spazio e può interrompere attacchi nemici in corso.

### Quando Usarlo
- Quando sei **circondato** da più nemici
- Come **ultima risorsa** con pochi HP (ma attento: costa 8 HP!)
- Per creare **distanza** quando i nemici ti pressano



