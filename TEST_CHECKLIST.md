# TEST CHECKLIST
## LavoroGiochi80 – Test Manuali

Questa checklist va eseguita prima di ogni merge su `dev` e prima di ogni release su `main`.  
I test sono manuali (nessun framework di test automatico nel MVP).

**Testato da:** `___________`  
**Data:** `___________`  
**Versione:** `___________`  
**Browser:** `___________`

---

## 🟢 Categoria 1 – Avvio e schermate

| # | Test | Atteso | Risultato | Note |
|---|------|--------|-----------|------|
| 1.1 | Aprire `index.html` nel browser | Il canvas appare, nessun errore in console | ☐ Pass ☐ Fail | |
| 1.2 | La Start Screen è visibile | Titolo gioco + istruzioni + "Premi Enter per iniziare" | ☐ Pass ☐ Fail | |
| 1.3 | Premere `Enter` sulla Start Screen | Il gioco inizia, player appare | ☐ Pass ☐ Fail | |
| 1.4 | La HUD è visibile durante il gioco | Barra HP e nome player visibili | ☐ Pass ☐ Fail | |

---

## 🟢 Categoria 2 – Movimento player

| # | Test | Atteso | Risultato | Note |
|---|------|--------|-----------|------|
| 2.1 | Premere `→` | Player si muove a destra | ☐ Pass ☐ Fail | |
| 2.2 | Premere `←` | Player si muove a sinistra | ☐ Pass ☐ Fail | |
| 2.3 | Premere `↑` | Player si muove verso l'alto (indietro nella profondità) | ☐ Pass ☐ Fail | |
| 2.4 | Premere `↓` | Player si muove verso il basso (avanti nella profondità) | ☐ Pass ☐ Fail | |
| 2.5 | Muoversi fino al bordo sinistro del canvas | Player si ferma, non esce | ☐ Pass ☐ Fail | |
| 2.6 | Muoversi fino al bordo destro del canvas | Player si ferma, non esce | ☐ Pass ☐ Fail | |
| 2.7 | Muoversi fino al bordo superiore | Player si ferma | ☐ Pass ☐ Fail | |
| 2.8 | Muoversi fino al bordo inferiore | Player si ferma | ☐ Pass ☐ Fail | |
| 2.9 | Movimento diagonale (↑+→ insieme) | Player si muove diagonalmente | ☐ Pass ☐ Fail | |

---

## 🟢 Categoria 3 – Attacco

| # | Test | Atteso | Risultato | Note |
|---|------|--------|-----------|------|
| 3.1 | Premere `A` senza nemici vicini | Animazione attacco riprodotta | ☐ Pass ☐ Fail | |
| 3.2 | Premere `A` durante il movimento | Player si ferma e attacca (o attacca in movimento, da definire) | ☐ Pass ☐ Fail | |
| 3.3 | Attaccare un nemico in range | Nemico perde HP, animazione hurt su nemico | ☐ Pass ☐ Fail | |
| 3.4 | Attaccare ripetutamente | L'attacco non si sovrappone a se stesso (cooldown) | ☐ Pass ☐ Fail | |
| 3.5 | Attaccare un nemico fuori range | Nessun danno applicato | ☐ Pass ☐ Fail | |

---

## 🟢 Categoria 4 – Nemici

| # | Test | Atteso | Risultato | Note |
|---|------|--------|-----------|------|
| 4.1 | Inizio gioco | I nemici appaiono nella posizione definita | ☐ Pass ☐ Fail | |
| 4.2 | Nemico a distanza dal player | Il nemico si avvicina verso il player | ☐ Pass ☐ Fail | |
| 4.3 | Nemico in range attacco | Il nemico attacca il player | ☐ Pass ☐ Fail | |
| 4.4 | Nemico riceve danno | HP del nemico diminuisce, animazione hurt | ☐ Pass ☐ Fail | |
| 4.5 | Nemico HP = 0 | Animazione morte, nemico rimosso dalla scena | ☐ Pass ☐ Fail | |
| 4.6 | Più nemici contemporaneamente | Tutti si comportano indipendentemente | ☐ Pass ☐ Fail | |

---

## 🟢 Categoria 5 – Sistema HP e danno

| # | Test | Atteso | Risultato | Note |
|---|------|--------|-----------|------|
| 5.1 | Player riceve danno da nemico | HP player diminuisce, barra HP aggiornata | ☐ Pass ☐ Fail | |
| 5.2 | Barra HP player a 50% | Barra parzialmente piena (colore giallo) | ☐ Pass ☐ Fail | |
| 5.3 | Barra HP player a 25% o meno | Barra quasi vuota (colore rosso) | ☐ Pass ☐ Fail | |
| 5.4 | HP player = 0 | Game Over screen appare | ☐ Pass ☐ Fail | |
| 5.5 | HP nemico = 0 | Nemico muore e viene rimosso | ☐ Pass ☐ Fail | |

---

## 🟢 Categoria 6 – Collisioni fisiche

| # | Test | Atteso | Risultato | Note |
|---|------|--------|-----------|------|
| 6.1 | Player cammina verso un nemico | Player non passa attraverso il nemico | ☐ Pass ☐ Fail | |
| 6.2 | Nemico cammina verso player | Nemico non passa attraverso il player | ☐ Pass ☐ Fail | |
| 6.3 | Più nemici nello stesso punto | Non si sovrappongono completamente | ☐ Pass ☐ Fail | |

---

## 🟢 Categoria 7 – Schermate e stati

| # | Test | Atteso | Risultato | Note |
|---|------|--------|-----------|------|
| 7.1 | Game Over appare | Testo "Game Over" visibile + istruzioni per ricominciare | ☐ Pass ☐ Fail | |
| 7.2 | Premere Enter sul Game Over | Il gioco ricomincia dalla Start Screen (o dall'inizio del livello) | ☐ Pass ☐ Fail | |
| 7.3 | Tutti i nemici sconfitti | Schermata vittoria / livello completato appare | ☐ Pass ☐ Fail | |

---

## 🟢 Categoria 8 – Performance e stabilità

| # | Test | Atteso | Risultato | Note |
|---|------|--------|-----------|------|
| 8.1 | FPS durante il gameplay | 60 FPS stabili (controllare con DevTools) | ☐ Pass ☐ Fail | |
| 8.2 | Console del browser durante il gioco | Nessun errore o warning JS | ☐ Pass ☐ Fail | |
| 8.3 | Gioco attivo per 5 minuti | Nessun crash o rallentamento | ☐ Pass ☐ Fail | |
| 8.4 | Perdita di focus e ritorno alla finestra | Il gioco continua normalmente | ☐ Pass ☐ Fail | |

---

## 🟢 Categoria 9 – Cross-browser (pre-release)

| # | Browser | Risultato |
|---|---------|-----------|
| 9.1 | Chrome (ultima versione) | ☐ Pass ☐ Fail |
| 9.2 | Firefox (ultima versione) | ☐ Pass ☐ Fail |
| 9.3 | Edge (ultima versione) | ☐ Pass ☐ Fail |

---

## Esito complessivo

| Categoria | Totale test | Passati | Falliti |
|-----------|-------------|---------|---------|
| 1 – Avvio | 4 | | |
| 2 – Movimento | 9 | | |
| 3 – Attacco | 5 | | |
| 4 – Nemici | 6 | | |
| 5 – HP e danno | 5 | | |
| 6 – Collisioni | 3 | | |
| 7 – Schermate | 3 | | |
| 8 – Performance | 4 | | |
| 9 – Cross-browser | 3 | | |
| **TOTALE** | **42** | | |

---

## Bug trovati

| ID | Descrizione | Gravità | Assegnato a | Stato |
|----|-------------|---------|-------------|-------|
| BUG-001 | | Alta/Media/Bassa | | Aperto/Chiuso |
