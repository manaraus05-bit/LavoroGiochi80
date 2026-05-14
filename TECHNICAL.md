#  Documentazione Tecnica

## Indice
1. [Stack Tecnologico](#stack-tecnologico)
2. [Architettura del Progetto](#architettura-del-progetto)
3. [Game Loop](#game-loop)
4. [Gestione degli Stati](#gestione-degli-stati)
5. [Classe Player](#classe-player)
6. [Classe Enemy](#classe-enemy)
7. [Classe InputHandler](#classe-inputhandler)
8. [Sistema di Combattimento](#sistema-di-combattimento)



---

## Stack Tecnologico

| Tecnologia | Versione | Utilizzo |
|------------|----------|----------|
| HTML5 | — | Struttura pagina, elemento `<canvas>` |
| CSS3 | — | HUD, overlay, animazioni |
| JavaScript (ES6+) | — | Logica di gioco, classi, moduli |
| Canvas API | — | Rendering 2D di tutti gli elementi |

**Nessuna dipendenza esterna.** Il progetto funziona interamente con le API native del browser, senza librerie di terze parti.

Il codice utilizza la sintassi **ES Modules** (`import`/`export`), che permette di suddividere il codice in file separati e mantenere una struttura modulare pulita.

---

## Architettura del Progetto

Il gioco è strutturato in **4 moduli JavaScript** più i file statici:

```
┌─────────────────────────────────────────────────────────────┐
│                        index.html                           │
│         (Entry Point — HUD — Canvas — Overlay)              │
└──────────────────────┬──────────────────────────────────────┘
                       │ carica
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                        game.js                              │
│    (Game Loop — State Machine — Spawn — Score)              │
│                                                             │
│   importa ──► player.js   importa ──► enemy.js             │
│   importa ──► input.js                                      │
└─────────────────────────────────────────────────────────────┘

┌──────────────┐   ┌──────────────┐   ┌──────────────────────┐
│  player.js   │   │  enemy.js    │   │     input.js         │
│  class Player│   │  class Enemy │   │  class InputHandler  │
└──────────────┘   └──────────────┘   └──────────────────────┘
```

### Principio di Separazione delle Responsabilità

Ogni file ha una singola responsabilità ben definita:

- **`game.js`** — Orchestratore principale. Non contiene logica di gioco specifica delle entità.
- **`player.js`** — Tutto ciò che riguarda il giocatore: movimento, attacchi, stato.
- **`enemy.js`** — Tutto ciò che riguarda i nemici: IA, danno al giocatore, vita.
- **`input.js`** — Astrazione dell'input da tastiera. Completamente disaccoppiato dalla logica di gioco.

---

## Game Loop

Il game loop è implementato in `game.js` tramite `requestAnimationFrame`, che garantisce la sincronizzazione con il refresh rate del monitor (tipicamente 60 fps).

```javascript
function loop() {
    update();   // Aggiorna la logica di gioco
    draw();     // Disegna il frame corrente
    requestAnimationFrame(loop);  // Pianifica il prossimo frame
}
loop(); // Avvia il loop
```

### Funzione `update()`
Responsabilità per frame:
1. Controlla input globali (ESC, Invio)
2. Aggiorna il giocatore (`player.update()`)
3. Aggiorna tutti i nemici (`enemy.update()`)
4. Filtra i nemici morti e aggiorna il punteggio
5. Spawna nuovi nemici se la lista è vuota
6. Controlla la condizione di game over

### Funzione `draw()`
Responsabilità per frame:
1. Pulisce il canvas (`clearRect`)
2. Disegna la linea dell'orizzonte
3. Ordina le entità per coordinata Y (pseudo-profondità)
4. Disegna ogni entità nell'ordine corretto

### Pseudo-Profondità (Y-Sorting)

Per simulare la profondità in 2D, le entità vengono ordinate per coordinata Y prima di essere disegnate. Chi ha Y più alto (più in basso nello schermo) viene disegnato sopra:

```javascript
[player, ...enemies].sort((a, b) => a.y - b.y).forEach(ent => ent.draw(ctx));
```

---

## Gestione degli Stati

Il gioco usa una **State Machine** con 3 stati distinti, gestita tramite una costante `STATES` e una variabile `state`:

```
┌───────────┐  Invio  ┌───────────┐  player.hp <= 0  ┌───────────┐
│           │ ──────► │           │ ───────────────► │           │
│   START   │         │   PLAY    │                  │   OVER    │
│           │ ◄────── │           │ ◄─────────────── │           │
└───────────┘   ESC   └───────────┘       Invio       └───────────┘
                            │
                            │ ESC
                            ▼
                       ┌───────────┐
                       │   START   │
                       └───────────┘
```

| Stato | Valore | Descrizione |
|-------|--------|-------------|
| `START` | `0` | Schermata iniziale, in attesa di input |
| `PLAY` | `1` | Partita in corso |
| `OVER` | `2` | Game Over, in attesa di reset |

La transizione di stato aggiorna anche l'UI: l'overlay HTML viene mostrato/nascosto e il messaggio nel DOM viene aggiornato.

---

## Classe Player

**File:** `player.js`

### Proprietà

| Proprietà | Tipo | Descrizione |
|-----------|------|-------------|
| `x`, `y` | Number | Posizione sul canvas |
| `w`, `h` | Number | Dimensioni (50x100 px) |
| `hp` | Number | Punti vita (max 100) |
| `speed` | Number | Velocità di movimento (4 px/frame) |
| `isAttacking` | Boolean | Blocca il movimento durante l'attacco |
| `state` | String | `IDLE`, `ATTACK`, o `SPECIAL` |
| `comboStep` | Number | Step corrente della combo (0-2) |
| `comboTimer` | Number | Countdown per la finestra combo |
| `attackCooldown` | Number | Frames di cooldown tra attacchi |
| `facing` | String | Direzione (`right` o `left`) |

### Metodi

#### `update(input, enemies)`
Eseguito ogni frame. Gestisce:
- Movimento (bloccato durante `isAttacking`)
- Rilevamento input per attacchi
- Decremento cooldown e timer
- Clamp della posizione nei confini del canvas
- Aggiornamento della barra HP nel DOM

#### `performAttack(enemies)`
- Imposta `isAttacking = true`, cooldown a 15 frame
- Calcola il danno in base a `comboStep` (10/15/20)
- Controlla collisione con i nemici nella direzione in cui il giocatore è rivolto
- Range di attacco: 75px orizzontale, 25px verticale
- Avanza il `comboStep` (modulo 3)

#### `performSpecial(enemies)`
- Cooldown più lungo (40 frame)
- Costa 8 HP al giocatore
- Raggio d'azione circolare di 120px
- Applica 20 danno e knockback ai nemici nel raggio
- Knockback: 60px nella direzione opposta al giocatore

#### `draw(ctx)`
- Colore del rettangolo basato sullo stato: blu (idle), giallo (attacco), viola (speciale)
- Durante l'attacco disegna l'hitbox visiva (rettangolo o cerchio per lo speciale)

---

## Classe Enemy

**File:** `enemy.js`

### Proprietà

| Proprietà | Tipo | Descrizione |
|-----------|------|-------------|
| `x`, `y` | Number | Posizione sul canvas |
| `w`, `h` | Number | Dimensioni (50x100 px) |
| `hp` | Number | Punti vita (max 40) |
| `speed` | Number | Velocità di inseguimento (1.5 px/frame) |
| `hitCooldown` | Number | Cooldown tra danni al giocatore (60 frame) |

### IA di Inseguimento

L'IA è di tipo **seek semplice**: il nemico si avvicina al giocatore mantenendo una distanza di 45px sull'asse X per simulare il "fianco a fianco":

```javascript
const targetX = player.x + (player.x < this.x ? 45 : -45);
```

Il movimento sull'asse Y è più lento (moltiplicato per 0.5) per dare una sensazione di profondità.

### Sistema di Danno

Il nemico infligge 5 HP al giocatore se:
- La distanza orizzontale è < 35px
- La distanza verticale è < 15px
- Il cooldown è 0

Dopo ogni colpo, il cooldown viene impostato a 60 frame (1 secondo a 60fps).

### `draw(ctx)`
- Rettangolo grigio scuro (#555)
- Barra vita rossa sopra l'entità, proporzionale agli HP rimanenti

---

## Classe InputHandler

**File:** `input.js`

Implementazione minimale e pulita dell'input da tastiera usando un `Set` per tracciare i tasti premuti.

```javascript
export class InputHandler {
    constructor() {
        this.keys = new Set();
        window.addEventListener('keydown', e => this.keys.add(e.code));
        window.addEventListener('keyup', e => this.keys.delete(e.code));
    }
    has(code) {
        return this.keys.has(code);
    }
}
```

### Vantaggi di questo approccio

- **Nessun delay:** Rileva la pressione continua dei tasti senza il ritardo del sistema operativo
- **Multi-tasto:** Il `Set` permette di tenere premuti più tasti contemporaneamente
- **Disaccoppiato:** Non conosce nulla della logica di gioco
- **`e.code` vs `e.key`:** L'uso di `e.code` rende l'input indipendente dalla lingua della tastiera (ad es. `KeyA` è sempre `A`, indipendentemente dal layout)

---

## Sistema di Combattimento

### Tabella dei Danni

| Azione | Danno Inflitto | Costo per il Player | Cooldown |
|--------|---------------|---------------------|----------|
| Attacco Combo 1 | 10 HP | 0 | 15 frame |
| Attacco Combo 2 | 15 HP | 0 | 15 frame |
| Attacco Combo 3 | 20 HP | 0 | 15 frame |
| Attacco Speciale | 20 HP (area) | 8 HP | 40 frame |
| Colpo nemico | 5 HP | — | 60 frame (nemico) |

### Finestra Combo

La combo funziona con un timer a scorrimento:
1. Primo `A` → `comboStep = 0`, `comboTimer = 40`
2. Secondo `A` entro 40 frame → `comboStep = 1`, timer resettato
3. Terzo `A` entro 40 frame → `comboStep = 2`, poi ritorna a 0
4. Se il timer scade → `comboStep` resettato a 0





