# LavoroGiochi80
## Zeus : Ruolo Programmatore Principale 
## Andre : WorkFlow
## Ale : Supporto Programmtore + Documentazione
## Vale : Asset Grafici
### Tutti Supporter



# Beat 'Em Up Project – Ispirato ai classici arcade anni '90

> Progetto di un videogioco **beat 'em up 2D** sviluppato con **HTML5, CSS3 e JavaScript**, ispirato ai grandi classici del genere.  

---

## 1. Obiettivo del progetto
Realizzare un **prototipo giocabile** di un picchiaduro a scorrimento laterale con le seguenti funzionalità minime:

- 1 personaggio giocante
- movimento su asse X + profondità su asse Y
- attacco base
- 1 tipo di nemico
- collisioni
- barra della vita
- 1 livello breve
- schermata Start / Game Over
- repository GitHub ordinata e documentata

---

## 2. Stack tecnologico
- **HTML5** – struttura del progetto e canvas
- **CSS3** – layout UI e stile dell'interfaccia
- **JavaScript** – logica di gioco, input, animazioni, collisioni, AI base
- **GitHub** – versionamento e collaborazione
- **VS Code** – ambiente di sviluppo

---

## 3. Struttura del progetto

```text
project-root/
│
├── index.html
├── style.css
├── README.md
├── .gitignore
│
├── src/
│   ├── main.js
│   ├── game.js
│   ├── player.js
│   ├── enemy.js
│   ├── input.js
│   ├── collision.js
│   ├── ui.js
│   └── utils.js
│
├── assets/
│   ├── sprites/
│   ├── backgrounds/
│   ├── effects/
│   ├── ui/
│   └── audio/
│
├── docs/
│   ├── GAME_DESIGN_DOCUMENT.md
│   ├── TECH_SPEC.md
│   ├── DECISIONS.md
│   └── CHANGELOG.md
│
└── tests/
```

---

## 4. MVP (Minimum Viable Product)
Per mantenere il progetto sostenibile, il primo obiettivo è completare il seguente MVP:

- [ ] 1 player giocabile
- [ ] 1 attacco base
- [ ] 1 nemico base
- [ ] collisioni funzionanti
- [ ] sistema HP
- [ ] 1 livello breve
- [ ] HUD minima
- [ ] schermata Start
- [ ] schermata Game Over
- [ ] documentazione iniziale pronta

---

## 5. Suddivisione del team

### 1) Programmatore JavaScript / HTML5 / CSS3
**Responsabilità:** gameplay core, logica del gioco, UI in-game.

**Task principali:**
- Creare il canvas HTML5
- Implementare game loop (`update()` / `render()`)
- Gestire input tastiera
- Implementare movimento player
- Implementare attacco base
- Gestire collisioni
- Creare nemico base
- Gestire HP, hit, game over
- Collegare sprite e animazioni

---

### 2) Addetto Documentazione Tecnica / Markdown / AI usage
**Responsabilità:** documentazione, organizzazione tecnica, linee guida.

**Task principali:**
- Scrivere e mantenere `README.md`
- Creare `GAME_DESIGN_DOCUMENT.md`
- Creare `TECH_SPEC.md`
- Aggiornare `CHANGELOG.md`
- Documentare struttura cartelle e naming convention
- Preparare linee guida sull'uso dell'AI
- Creare checklist di test
- Preparare template issue / pull request

---

### 3) Addetto Asset Grafici
**Responsabilità:** sprite, sfondi, HUD, effetti visivi.

**Task principali:**
- Definire stile grafico
- Creare moodboard
- Preparare sprite player e nemici
- Preparare animazioni base
- Disegnare HUD
- Creare sfondi del primo livello
- Esportare asset ottimizzati
- Fornire placeholder rapidi per sviluppo

---

### 4) Addetto GitHub / VS Code / Workflow
**Responsabilità:** gestione repository, branch, collaborazione tecnica.

**Task principali:**
- Creare repository GitHub
- Definire branch strategy (`main`, `dev`, `feature/...`)
- Configurare `.gitignore`
- Creare board Kanban
- Gestire issue, label, milestone
- Supportare pull request e merge
- Verificare ordine e consistenza del repository

---

## 6. Roadmap del progetto

### Fase 1 – Pre-produzione
- Definizione concept
- Definizione MVP
- Setup repo
- Documentazione iniziale
- Placeholder grafici

### Fase 2 – Prototipo tecnico
- Movimento player
- Collisioni
- Attacco base
- Nemico semplice
- Sistema HP

### Fase 3 – Vertical Slice
- Livello completo breve
- HUD
- Start screen / Game Over
- Audio placeholder
- Miglioramenti animazioni

### Fase 4 – Polish
- Refactor codice
- Asset migliorati
- Bug fixing
- Performance base
- Aggiornamento documentazione

---

## 7. Convenzioni di lavoro

### Branch
- `main` → versione stabile
- `dev` → integrazione sviluppo
- `feature/nome-task` → sviluppo singola feature

### Commit (consigliati)
- `feat:` nuova funzionalità
- `fix:` correzione bug
- `docs:` documentazione
- `style:` modifiche estetiche / CSS
- `refactor:` riorganizzazione codice
- `assets:` aggiunta/modifica asset grafici o audio

Esempi:

```bash
git commit -m "feat: aggiunto movimento player"
git commit -m "docs: aggiornato README con struttura cartelle"
git commit -m "assets: aggiunti sprite idle del player"
```

---

## 8. Come avviare il progetto

### Opzione semplice
1. Clonare la repository
2. Aprire la cartella in VS Code
3. Aprire `index.html` nel browser

### Clonazione repository
```bash
git clone https://github.com/USERNAME/NOME-REPO.git
cd NOME-REPO
```

> Se usate **Live Server** in VS Code, potete lanciare il progetto direttamente da lì.

---

## 9. Kanban consigliato
Colonne suggerite:
- Backlog
- To Do
- In Progress
- Review
- Done

Esempi di task:
- Setup repository
- Canvas HTML5
- Input tastiera
- Movimento player
- Collisioni
- Attacco base
- Nemico base
- HUD
- Start screen
- Game Over
- README iniziale
- Sprite player idle/walk/attack

---

## 10. Definition of Done
Un task si considera completato solo se:
- [ ] è funzionante nel gioco
- [ ] è stato testato
- [ ] è stato pushato nel branch corretto
- [ ] è tracciato via commit / PR
- [ ] rispetta naming convention
- [ ] non rompe altre parti del progetto
- [ ] è documentato se necessario

---

## 11. Rischi da evitare
- Copiare direttamente un gioco esistente invece di creare una versione originale
- Aggiungere troppe feature troppo presto
- Lavorare tutti direttamente su `main`
- Usare asset finali prima dei placeholder
- Non definire chiaramente l'MVP
- Non documentare le decisioni tecniche

---

## 12. Idee future (post-MVP)
Da considerare **solo dopo** il completamento dell'MVP:
- combo avanzate
- boss fight
- più tipi di nemici
- più livelli
- effetti sonori completi
- multiplayer locale
- sistema punteggio avanzato
- power-up

---

## 13. Documenti consigliati nella cartella /docs
- `GAME_DESIGN_DOCUMENT.md` → meccaniche, livello, personaggi, flow di gioco
- `TECH_SPEC.md` → architettura, moduli JS, collisioni, stati
- `DECISIONS.md` → decisioni tecniche del team
- `CHANGELOG.md` → storico aggiornamenti

---

## 14. Licenza e note legali
Questo progetto deve utilizzare:
- **nome originale del gioco**
- **personaggi originali**
- **asset grafici originali o liberi da diritti**
- **audio originale o royalty-free**

Il progetto è **ispirato al genere beat 'em up arcade** ma non deve riprodurre contenuti protetti da copyright.

---

## 15. Stato del progetto
**Versione attuale:** `v0.1-prototype`  
**Stato:** In pianificazione / setup iniziale

---

## 16. Contatti del team
Compilare qui i riferimenti interni del team:

- Programmazione: `NOME`
- Documentazione: `NOME`
- Asset grafici: `NOME`
- GitHub / Workflow: `NOME`

---


