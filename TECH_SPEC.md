# TECH SPEC
## LavoroGiochi80 – Specifiche Tecniche

**Versione documento:** 0.1  
**Data:** 2025  
**Stato:** Bozza / MVP

---

## 1. Stack tecnologico

| Layer | Tecnologia | Note |
|-------|-----------|------|
| Rendering | HTML5 Canvas 2D API | Nessun framework grafico |
| Logica | JavaScript ES6+ (moduli) | Classi, arrow functions, import/export |
| Struttura | HTML5 | Solo `index.html` come entry point |
| Stile UI | CSS3 | Layout contenitore canvas + HUD overlay |
| Versionamento | Git + GitHub | Branch strategy definita nel README |
| Editor | VS Code + Live Server | |

**Nessuna dipendenza esterna.** Zero librerie, zero build tool, zero bundler.  
Il gioco deve funzionare aprendo `index.html` nel browser.

---

## 2. Architettura dei moduli

```
main.js
  └── game.js          ← orchestratore centrale
        ├── input.js   ← stato tastiera
        ├── player.js  ← entità player
        ├── enemy.js   ← entità nemico (istanze multiple)
        ├── collision.js ← rilevamento e risposta
        └── ui.js      ← HUD e schermate
              └── utils.js  ← helper puri (no stato)
```

### 2.1 `main.js`
- Punto di ingresso
- Ottiene il riferimento al canvas e al contesto 2D
- Istanzia `Game`
- Chiama `game.start()`

```js
// Esempio struttura
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
const game = new Game(canvas, ctx);
game.start();
```

### 2.2 `game.js` – classe `Game`
Responsabilità: game loop, gestione stati, coordinamento moduli.

```js
class Game {
  constructor(canvas, ctx) { ... }

  start()           // avvia requestAnimationFrame
  update(dt)        // aggiorna logica (chiamata ogni frame)
  render()          // disegna tutto (chiamata ogni frame)
  changeState(s)    // transizione stati: START|PLAYING|GAMEOVER|WIN

  // Proprietà chiave
  state             // stato corrente
  player            // istanza Player
  enemies[]         // array istanze Enemy
  lastTime          // timestamp ultimo frame (per delta time)
}
```

**Game loop pattern:**
```js
loop(timestamp) {
  const dt = timestamp - this.lastTime;
  this.lastTime = timestamp;
  this.update(dt);
  this.render();
  requestAnimationFrame(this.loop.bind(this));
}
```

### 2.3 `player.js` – classe `Player`

```js
class Player {
  constructor(x, y)

  update(inputState, dt)
  render(ctx)
  attack()
  takeDamage(amount)
  die()

  // Proprietà
  x, y              // posizione
  width, height     // dimensioni hitbox
  hp, maxHp
  speed
  isAttacking       // bool
  attackFrame       // frame corrente animazione attacco
  state             // 'idle' | 'walk' | 'attack' | 'hurt' | 'dead'
  facing            // 'left' | 'right'
}
```

### 2.4 `enemy.js` – classe `Enemy`

```js
class Enemy {
  constructor(x, y, config)

  update(player, dt)
  render(ctx)
  takeDamage(amount)
  die()

  aiUpdate(player)  // logica AI: avvicinarsi, attaccare

  // Proprietà
  x, y
  width, height
  hp, maxHp
  speed
  attackRange
  attackCooldown
  state             // 'idle' | 'walk' | 'attack' | 'hurt' | 'dead'
}
```

### 2.5 `input.js` – classe `InputHandler`

```js
class InputHandler {
  constructor()

  // Proprietà
  keys = {
    ArrowLeft: false,
    ArrowRight: false,
    ArrowUp: false,
    ArrowDown: false,
    KeyA: false,      // attacco
    Enter: false,
  }
}
```

Registra `keydown` e `keyup` sul `document`.  
`game.js` legge lo stato dei tasti ogni frame senza polling diretto agli eventi.

### 2.6 `collision.js`

Funzioni pure (no stato), esportate come modulo.

```js
// Collisione AABB tra due rettangoli
export function rectsOverlap(a, b) { ... }

// Applica separazione fisica tra due entità
export function resolveOverlap(entity1, entity2) { ... }

// Controlla se hitbox attacco colpisce un'entità
export function checkAttackHit(attacker, target) { ... }
```

### 2.7 `ui.js` – classe `UI`

```js
class UI {
  constructor(ctx, canvas)

  drawHUD(player)
  drawStartScreen()
  drawGameOver()
  drawLevelComplete()
  drawHealthBar(entity, x, y, w, h)
}
```

### 2.8 `utils.js`

```js
// Clamp di un valore tra min e max
export function clamp(value, min, max) { ... }

// Distanza euclidea tra due punti
export function distance(x1, y1, x2, y2) { ... }

// Genera numero casuale in range
export function randomRange(min, max) { ... }
```

---

## 3. Sistema di coordinate e rendering

### 3.1 Canvas
- Dimensioni fisse: **800 × 450 px** (16:9)
- Origine in alto a sinistra `(0, 0)`
- CSS: centrato nella pagina, `image-rendering: pixelated`

### 3.2 Asse Y come profondità
```
Y basso  =  primo piano  (davanti)
Y alto   =  sfondo       (dietro)
```

### 3.3 Z-sorting (rendering order)
Le entità vengono ordinate per `y + height` prima del render:
```js
allEntities.sort((a, b) => (a.y + a.height) - (b.y + b.height));
```

### 3.4 Hitbox
- Ogni entità ha una hitbox AABB separata dallo sprite visivo
- Offset hitbox configurabile per un feel più permissivo

---

## 4. Sistema di collisioni

### Tipi di collisione

| Tipo | Tra | Effetto |
|------|-----|---------|
| Fisica | Player ↔ Nemico | Impedisce compenetrazione |
| Attacco | Hitbox attacco player → Nemico | `enemy.takeDamage()` |
| Attacco | Hitbox attacco nemico → Player | `player.takeDamage()` |

### AABB overlap check
```js
function rectsOverlap(a, b) {
  return (
    a.x < b.x + b.width &&
    a.x + a.width > b.x &&
    a.y < b.y + b.height &&
    a.y + a.height > b.y
  );
}
```

### Hitbox attacco player
Quando `player.isAttacking`:
```js
const attackHitbox = {
  x: player.facing === 'right' ? player.x + player.width : player.x - ATTACK_RANGE,
  y: player.y,
  width: ATTACK_RANGE,
  height: player.height
};
```

---

## 5. Gestione delta time

Il game loop usa il delta time per movimento indipendente dal framerate:
```js
// Movimento con dt (in ms)
entity.x += entity.speed * (dt / 16.67); // 16.67ms = 60fps target
```

---

## 6. Configurazione centralizzata

Valori di bilanciamento in un unico oggetto `CONFIG` in `utils.js` o file dedicato:

```js
export const CONFIG = {
  canvas: { width: 800, height: 450 },
  player: {
    speed: 3,
    hp: 100,
    attackDamage: 15,
    attackDuration: 20,   // frame
    attackRange: 60,
    hurtDuration: 15,
  },
  enemy: {
    speed: 1.5,
    hp: 40,
    damage: 10,
    attackRange: 40,
    attackCooldown: 90,   // frame
    detectionRange: 300,
  },
  spawn: {
    wave1: [{ x: 750, y: 200 }, { x: 780, y: 280 }],
    wave2: [{ x: 750, y: 180 }, { x: 800, y: 250 }, { x: 770, y: 320 }],
  }
};
```

---

## 7. Struttura dei file asset

### Sprite (PNG con trasparenza)
```
assets/sprites/
  player_idle.png         # spritesheet idle (N frame)
  player_walk.png         # spritesheet walk
  player_attack.png       # spritesheet attack
  player_hurt.png
  player_death.png
  enemy_idle.png
  enemy_walk.png
  enemy_attack.png
  enemy_hurt.png
  enemy_death.png
```

### Naming convention asset

| Tipo | Pattern | Esempio |
|------|---------|---------|
| Sprite | `{entity}_{state}.png` | `player_walk.png` |
| Sfondo | `bg_{livello}_{variante}.png` | `bg_level1_night.png` |
| Effetti | `fx_{nome}.png` | `fx_punch_impact.png` |
| UI | `ui_{elemento}.png` | `ui_healthbar_bg.png` |
| Audio SFX | `sfx_{evento}.wav` | `sfx_punch.wav` |
| Audio BGM | `bgm_{livello}.mp3` | `bgm_level1.mp3` |

---

## 8. Naming convention codice

| Elemento | Stile | Esempio |
|----------|-------|---------|
| Variabili | `camelCase` | `playerSpeed`, `isAttacking` |
| Classi | `PascalCase` | `Player`, `Enemy`, `InputHandler` |
| Costanti | `UPPER_SNAKE_CASE` | `MAX_HP`, `ATTACK_DAMAGE` |
| File JS | `camelCase.js` | `player.js`, `collision.js` |
| File asset | `snake_case` | `player_walk.png` |
| Funzioni | `camelCase` verbo+sostantivo | `takeDamage()`, `drawHealthBar()` |

---

## 9. Performance baseline

- Target: **60 FPS** su hardware medio
- Nessun loop interno pesante durante il render
- `ctx.clearRect()` su tutto il canvas ogni frame
- Sprite pre-caricate all'avvio (no lazy load in game loop)
- Array nemici pulito di quelli `dead` ad ogni wave

---

## 10. Browser support

| Browser | Supportato |
|---------|-----------|
| Chrome 90+ | ✅ |
| Firefox 88+ | ✅ |
| Edge 90+ | ✅ |
| Safari 14+ | ✅ |
| IE / Legacy | ❌ |
