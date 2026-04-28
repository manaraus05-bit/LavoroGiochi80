# GAME DESIGN DOCUMENT
## street of rage – Beat 'Em Up Arcade

**Versione documento:** 0.1  
**Data:** 2026
**Stato:** Bozza / MVP

### **FORMATO:** 16:9
---

## 1. Concept del gioco

### 1.1 Logline
> *Un picchiaduro a scorrimento laterale ambientato nelle strade di una città degli anni '90: ondate di nemici da abbattere, una missione da completare, puri riflessi e pugni.*

### 1.2 Genere
Beat 'Em Up 2D a scorrimento laterale con visuale pseudo-isometrica (asse Y = profondità).

### 1.3 Ispirazione
- Genere: *Streets of Rage*, *Final Fight*, *Double Dragon*
- Stile grafico: pixel art anni '90, palette limitata, sprite leggibili
- Mood: azione rapida, soddisfazione dei colpi, ritmo serrato

### 1.4 Piattaforma
Browser (desktop) – HTML5 Canvas

### 1.5 Target
Giocatori casual / nostalgici, studenti di sviluppo web

---

## 2. Personaggi

---

### 2.1 Protagonista 

---

### 2.2 Nemico base – 

---

## 3. Meccaniche di gioco

### 3.1 Movimento player
- **Asse X:** sinistra / destra
- **Asse Y:** su / giù (simula profondità / z-depth)
- Il player non può uscire dai bordi del canvas
- La coordinata Y determina anche l'ordine di rendering (z-sorting)

### 3.2 Sistema di attacco
- L'attacco si attiva alla pressione del tasto `A`
- Durante l'animazione di attacco il player non può muoversi
- La hitbox dell'attacco è attiva solo per N frame definiti nella config
- Il danno viene applicato a tutti i nemici nella hitbox

### 3.3 Sistema HP e danno
- Ogni entità (player, nemico) ha un valore `hp` e `maxHp`
- Il danno riduce `hp`
- `hp ≤ 0` → stato `dead`
- Player `dead` → Game Over
- Tutti i nemici `dead` → livello completato

### 3.4 Collisioni
- **Player vs Nemico:** impedire compenetrazione fisica
- **Hitbox attacco vs Nemico:** applicare danno
- **Hitbox nemico vs Player:** applicare danno
- Le hitbox sono rettangoli semplici (AABB)

### 3.5 Scorrimento livello (post-MVP)
Nel MVP il livello è una singola schermata fissa.  
Post-MVP: scorrimento orizzontale con camera che segue il player.

---

## 4. Struttura del livello (MVP)

### Livello 1 – "Le Strade del Quartiere"

**Ambientazione:** Strada urbana anni '90, notte, luci al neon sfocate.

**Flow:**
1. **Start Screen** → premi Enter per iniziare
2. **Intro breve** (testo su schermo)
3. **Fase 1** – compaiono 2 teppisti
4. **Fase 2** – compaiono 3 teppisti
5. **Vittoria** → schermata "Livello completato" *(post-MVP: next level)*
6. Se HP player = 0 → **Game Over**

**Spawn nemici:**  
I nemici appaiono da destra (e sinistra post-MVP) a intervalli di tempo o al superamento di una soglia X.

---

## 5. HUD (Heads-Up Display)

```
┌─────────────────────────────────────────────────┐
│ MARCO          [██████████░░░░] 70/100 HP         │
│                                                   │
│                    CANVAS DI GIOCO                │
│                                                   │
│                                          SCORE: 0 │
└─────────────────────────────────────────────────┘
```

Elementi HUD MVP:
- Barra HP player (con colore che vira da verde → giallo → rosso)
- Nome personaggio
- Punteggio (anche se semplice, da tenere per post-MVP)

---

## 6. Stati del gioco

```
START_SCREEN
     │
     ▼ (Enter)
  PLAYING ──────────────────► GAME_OVER
     │                            │
     ▼ (tutti nemici morti)       ▼ (Enter)
 LEVEL_COMPLETE              START_SCREEN
```

| Stato | Descrizione |
|-------|-------------|
| `START_SCREEN` | Schermata titolo, attende input |
| `PLAYING` | Gameplay attivo |
| `GAME_OVER` | Player morto, mostra schermata |
| `LEVEL_COMPLETE` | Livello finito *(post-MVP: next level)* |
| `PAUSED` | *(post-MVP)* |

---

## 7. Audio (placeholder MVP)

| Evento | Suono |
|--------|-------|
| Attacco | `attack` |
| Hit ricevuto | `hit ricevuto` |
| Nemico morto | `enemydeath` |
| Game Over | `player death` |
| Musica di gioco | `soundgame` (loop) |
| mossa speciale | ``|

> Tutti i file audio devono essere **royalty-free** o prodotti originalmente.

---

## 8. Idee post-MVP

- Combo system (3 pugni = calcio finale)
- Attacco speciale (costa HP)
- Power-up a terra (cibo per HP, armi)
- Boss di fine livello
- Più livelli con ambientazioni diverse
- 2° personaggio selezionabile
- Multiplayer locale (2 player)
- Sistema punteggio con high score
- Effetti particellari (sangue stilizzato, scintille)
