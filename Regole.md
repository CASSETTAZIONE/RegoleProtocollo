# Protocollo di Comunicazione CHAT

## 1. Configurazione iniziale
- Il server apre un **socket** sulla **PORTA 3645** e rimane in ascolto per connessioni.
- Alla connessione di un client, il server crea un **thread dedicato** per gestire in modo indipendente la comunicazione con l'utente.
- Ogni thread gestisce i comandi del client, inviando o ricevendo messaggi e dati senza interrompere le altre connessioni.

- Alla connessione l'utente insersisce il Nickname.
- Qualunque cosa scriva viene mandata nella chat pubblica a tutti gli altri client connessi.

## 2. Comandi

### Comandi di Controllo
- **`"!"` - Disconnessione**: L'utente desidera terminare la comunicazione con il server.
  - **Azione**: Il server chiude il socket dell'utente e interrompe il thread. Il comando associato è `"/quit"`.

- **`"@"` - Cambio Nickname**: L'utente desidera cambiare il proprio nickname.
  - **Azione**: Il server aggiorna il nickname dell'utente e notifica il cambiamento agli altri utenti nella chat. Il comando associato è `"/change"`.

- **`"?"` - Guida ai Comandi**: L'utente richiede la lista dei comandi disponibili.
  - **Azione**: Il server invia al client una lista di comandi e le loro funzioni (comando `"/help"`).

### Comandi di Interazione con Altri Utenti
- **`"*"` - Lista degli Utenti Connessi**: L'utente richiede la lista di tutti gli utenti attualmente connessi.
  - **Azione**: Il server risponde con una stringa contenente i nickname di tutti gli utenti connessi, separati da virgole. Il comando associato è `"/list"`.

- **`"#"` - Chat Privata**: L'utente desidera entrare in una chat privata con un altro utente.
  - **Azione**: Se il nickname specificato esiste, il server attiva la chat privata tra i due utenti (comando `"/msg"`). 
  - **Errore**: Se il nickname non esiste, il server invia `"nf"` (not found); altrimenti, conferma la connessione con `"fn"` (found).

- **`"%"` - Ritorno alla Chat Pubblica**: L'utente desidera uscire dalla chat privata e tornare alla chat pubblica.
  - **Azione**: Il server riporta l'utente alla modalità di chat pubblica, rendendo i messaggi visibili a tutti (comando `"/pb"`).

### Messaggi Pubblici
- **Messaggi Generici**: Qualsiasi messaggio che non inizia con uno dei comandi sopra elencati viene considerato un messaggio pubblico.
  - **Azione**: Il server inoltra il messaggio a tutti gli utenti connessi, indicando il nickname dell'autore sopra il messaggio.

## 3. Gestione Utenti e Messaggi
- **Array di Socket e Nickname**: Il server mantiene un dizionario che associa ogni `socket` utente al rispettivo `nickname`. Questa struttura permette di:
  - Inviare messaggi privati.
  - Aggiornare il nickname di un utente.
  - Fornire agli utenti una lista aggiornata degli utenti attivi nella chat.

- **Broadcast dei Messaggi Pubblici**: Ogni messaggio pubblico viene inoltrato a tutti gli utenti connessi. Il messaggio include:
  - Il **nickname dell’autore** per permettere l'identificazione dell'utente.
  - La **visualizzazione in tempo reale** degli aggiornamenti a tutti gli utenti.

## 4. Errori e Risposte del Server
- **Utente Non Trovato**: Se un utente tenta di entrare in chat privata con un nickname non valido, il server risponde con il messaggio `"nf"` (not found).
- **Conferma Chat Privata**: Se l'utente specificato esiste, il server risponde con `"fn"` (found) e stabilisce la comunicazione privata.
- **Errore di Comando Non Riconosciuto**: Se il comando inserito non è riconosciuto, il server invia un messaggio di errore che invita l'utente a utilizzare `"/help"` per la lista dei comandi validi.
