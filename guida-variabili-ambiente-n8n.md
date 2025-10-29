# Guida Variabili d'Ambiente n8n
## Spiegazione semplice per tutti

Questa guida spiega tutte le impostazioni che puoi configurare nel tuo server n8n. Ogni impostazione è spiegata in modo semplice, con consigli su quando usarla.

---

## 1. IMPOSTAZIONI GENERALI (Deployment)

### Indirizzo e Accesso

**N8N_HOST**
- **Cosa fa**: L'indirizzo web dove gli utenti possono accedere a n8n
- **Default**: localhost (solo dal tuo computer)
- **Esempio**: `miosito.com`
- **Quando cambiarlo**: Sempre, se vuoi che altre persone possano usare n8n

**N8N_PORT**
- **Cosa fa**: Il "cancello" attraverso cui n8n risponde
- **Default**: 5678
- **Quando cambiarlo**: Se hai già qualcos'altro che usa la porta 5678, scegli un numero diverso tipo 8080

**N8N_PROTOCOL**
- **Cosa fa**: Se usare connessione normale (http) o sicura (https)
- **Valori**: `http` o `https`
- **Default**: http
- **Consiglio**: Usa `https` se hai dati importanti o se n8n è accessibile da internet

**N8N_EDITOR_BASE_URL**
- **Cosa fa**: L'indirizzo completo che n8n usa per mandare email o link agli utenti
- **Esempio**: `https://miosito.com`
- **Quando serve**: Se usi la funzione di invito utenti o autenticazione esterna

**N8N_PATH**
- **Cosa fa**: Se n8n non è all'indirizzo principale ma in una sottocartella
- **Default**: `/` (indirizzo principale)
- **Esempio**: `/n8n` se accedi con `miosito.com/n8n`

**N8N_ENCRYPTION_KEY**
- **Cosa fa**: Una password segreta per proteggere le credenziali salvate nel database
- **Default**: n8n ne crea una automaticamente
- **⚠️ Importante**: NON cambiarla dopo aver iniziato a usare n8n, altrimenti perdi tutte le password salvate

### Sicurezza HTTPS

**N8N_SSL_KEY** e **N8N_SSL_CERT**
- **Cosa fanno**: I file per attivare la connessione sicura (https)
- **Quando servono**: Se usi `N8N_PROTOCOL=https` e gestisci tu i certificati
- **Nota**: Molti usano sistemi esterni (tipo Nginx o Traefik) per gestire https, quindi spesso questi non servono

### Proxy (per connessioni aziendali)

**HTTP_PROXY** / **HTTPS_PROXY**
- **Cosa fanno**: Se il tuo server deve passare attraverso un "intermediario" per accedere a internet
- **Quando servono**: Solo in reti aziendali che richiedono un proxy
- **Esempio**: `http://proxy.aziendale.com:8080`

**N8N_PROXY_HOPS**
- **Cosa fa**: Quanti "intermediari" ci sono tra gli utenti e n8n
- **Default**: 0
- **Quando cambiarlo**: Metti `1` se usi un reverse proxy (tipo Nginx, Traefik, Cloudflare)

### Funzionalità Extra

**N8N_DISABLE_UI**
- **Cosa fa**: Disattiva completamente l'interfaccia grafica
- **Default**: false (interfaccia attiva)
- **Quando usarlo**: Mai, a meno che tu voglia usare n8n solo tramite API

**N8N_TEMPLATES_ENABLED**
- **Cosa fa**: Permette di vedere e usare modelli di workflow già pronti
- **Default**: false (disattivato)
- **Consiglio**: Metti `true` per vedere esempi e velocizzare il lavoro

**N8N_VERSION_NOTIFICATIONS_ENABLED**
- **Cosa fa**: Ti avvisa quando c'è una nuova versione di n8n o problemi di sicurezza
- **Default**: true
- **Consiglio**: Lascia `true` per sapere quando aggiornare

**N8N_DIAGNOSTICS_ENABLED**
- **Cosa fa**: Invia statistiche anonime al team di n8n per migliorare il prodotto
- **Default**: true
- **Nota**: Se lo metti `false`, non potrai usare la funzione "Ask AI" nei nodi Code

### API Pubblica

**N8N_PUBLIC_API_DISABLED**
- **Cosa fa**: Attiva/disattiva l'API per controllare n8n da programmi esterni
- **Default**: false (API attiva)
- **Quando disattivarla**: Se vuoi più sicurezza e non usi l'API

### Spegnimento Sicuro

**N8N_GRACEFUL_SHUTDOWN_TIMEOUT**
- **Cosa fa**: Quanti secondi aspettare i workflow in corso prima di spegnere n8n
- **Default**: 30 secondi
- **Quando aumentarlo**:
  - Metti `60` se i tuoi workflow fanno operazioni importanti (pagamenti, scritture database)
  - Metti `120` se i workflow durano più di 30 secondi
- **Perché serve**: Evita di interrompere operazioni a metà quando riavvii n8n

---

## 2. DATABASE (Dove n8n salva i dati)

### Tipo di Database

**DB_TYPE**
- **Cosa fa**: Sceglie dove salvare tutti i dati di n8n
- **Valori**: `sqlite` o `postgresdb`
- **Default**: sqlite
- **Quale usare**:
  - `sqlite`: Va bene per test o uso personale (1 persona)
  - `postgresdb` (PostgreSQL): **Obbligatorio** per uso serio (più utenti, più workflow)

### PostgreSQL (Database Professionale)

**DB_POSTGRESDB_HOST**
- **Cosa fa**: Dove si trova il database PostgreSQL
- **Esempio**: `localhost` (stesso computer) o `database.miosito.com`

**DB_POSTGRESDB_PORT**
- **Cosa fa**: La porta di PostgreSQL
- **Default**: 5432

**DB_POSTGRESDB_DATABASE**
- **Cosa fa**: Nome del database da usare
- **Default**: n8n

**DB_POSTGRESDB_USER** e **DB_POSTGRESDB_PASSWORD**
- **Cosa fanno**: Username e password per accedere al database
- **⚠️ Importante**: Usa password complesse!

**DB_POSTGRESDB_POOL_SIZE**
- **Cosa fa**: Quanti "canali" di comunicazione tenere aperti con il database
- **Default**: 2
- **Come sceglierlo**:
  - Server con 2GB RAM o meno: lascia `2`
  - Server con 4GB RAM: usa `4`
  - Server con 8GB+ RAM e molti utenti: usa `6-8`
  - ⚠️ Non esagerare: troppi rallentano invece di velocizzare

**DB_POSTGRESDB_SSL_ENABLED**
- **Cosa fa**: Usa connessione criptata al database
- **Default**: false
- **Quando attivarlo**: Se il database è su un server diverso o hai dati sensibili

### SQLite (Database Semplice)

**DB_SQLITE_POOL_SIZE**
- **Cosa fa**: Attiva una modalità più veloce per leggere i dati
- **Default**: 0 (modalità lenta)
- **Consiglio**: Metti `4` per velocizzare (ma solo se usi n8n da solo, non per produzione seria)

**DB_SQLITE_VACUUM_ON_STARTUP**
- **Cosa fa**: "Ripulisce" il database all'avvio per renderlo più piccolo e veloce
- **Default**: false
- **⚠️ Nota**: L'operazione può richiedere diversi minuti con database grandi

---

## 3. ESECUZIONE WORKFLOW

### Timeout (Tempo Massimo)

**EXECUTIONS_TIMEOUT**
- **Cosa fa**: Dopo quanti secondi fermare un workflow che sta impiegando troppo tempo
- **Default**: -1 (nessun limite)
- **Come sceglierlo**:
  - Workflow veloci (meno di 5 minuti): `300` (5 minuti)
  - Workflow normali: `3600` (1 ora)
  - Workflow lunghi (elaborazione file grandi, migliaia di email): `14400` (4 ore)
- **Perché serve**: Evita che un workflow "impazzito" occupi il server per sempre

**EXECUTIONS_TIMEOUT_MAX**
- **Cosa fa**: Il tempo massimo che gli utenti possono impostare per un singolo workflow
- **Default**: 3600 (1 ora)
- **Quando aumentarlo**: Se hai workflow che legittimamente impiegano più di 1 ora

### Salvataggio Risultati

**EXECUTIONS_DATA_SAVE_ON_SUCCESS**
- **Cosa fa**: Salva i dati quando un workflow finisce bene
- **Valori**: `all` (tutto) o `none` (niente)
- **Default**: all
- **Consiglio**:
  - Lascia `all` per vedere cosa ha fatto il workflow
  - Usa `none` solo se hai pochissimo spazio su disco

**EXECUTIONS_DATA_SAVE_ON_ERROR**
- **Cosa fa**: Salva i dati quando un workflow va in errore
- **Default**: all
- **Consiglio**: Lascia sempre `all` per capire cosa è andato storto

**EXECUTIONS_DATA_SAVE_ON_PROGRESS**
- **Cosa fa**: Salva ogni singolo passaggio del workflow mentre è in esecuzione
- **Default**: false
- **Quando attivarlo**:
  - Se hai workflow molto lunghi e vuoi vedere a che punto sono
  - ⚠️ Consuma più spazio e rallenta leggermente

### Pulizia Automatica (Importante!)

**EXECUTIONS_DATA_PRUNE**
- **Cosa fa**: Cancella automaticamente le vecchie esecuzioni per non riempire il disco
- **Default**: true
- **Consiglio**: **Lascia sempre true**, altrimenti il disco si riempie

**EXECUTIONS_DATA_MAX_AGE**
- **Cosa fa**: Dopo quante ore cancellare le vecchie esecuzioni
- **Default**: 336 ore (14 giorni)
- **Come sceglierlo**:
  - Disco piccolo (meno di 20GB liberi): `168` (7 giorni)
  - Disco normale: `336` (14 giorni) o `720` (30 giorni)
  - Disco grande (100GB+ liberi): `2160` (90 giorni)
  - Server importante dove servono log storici: `4320` (6 mesi)

**EXECUTIONS_DATA_PRUNE_MAX_COUNT**
- **Cosa fa**: Numero massimo di esecuzioni da tenere, anche se recenti
- **Default**: 10000
- **Quando abbassarlo**: Se hai poco spazio disco, metti `5000` o `2000`

### Esecuzioni Simultanee

**N8N_CONCURRENCY_PRODUCTION_LIMIT**
- **Cosa fa**: Quanti workflow possono girare contemporaneamente
- **Default**: -1 (nessun limite)
- **Come sceglierlo**:
  - Server con 2GB RAM: `2`
  - Server con 4GB RAM: `5`
  - Server con 8GB+ RAM: `10-15`
- **Perché serve**: Evita che troppe esecuzioni rallentino tutto

---

## 4. SICUREZZA

### Accesso ai File

**N8N_BLOCK_FILE_ACCESS_TO_N8N_FILES**
- **Cosa fa**: Impedisce ai workflow di leggere i file di configurazione di n8n
- **Default**: true (accesso bloccato)
- **Consiglio**: **Lascia sempre true** per sicurezza

**N8N_RESTRICT_FILE_ACCESS_TO**
- **Cosa fa**: I workflow possono leggere file solo da queste cartelle
- **Default**: nessuna limitazione
- **Esempio**: `/home/user/documenti:/home/user/download`
- **Quando usarlo**: Se hai utenti non fidati che potrebbero leggere file sensibili

**N8N_BLOCK_ENV_ACCESS_IN_NODE**
- **Cosa fa**: Impedisce ai workflow di leggere le impostazioni del server
- **Default**: false (possono leggere)
- **Quando metterlo true**: Se hai password o chiavi API nelle variabili d'ambiente e utenti non fidati

**N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS**
- **Cosa fa**: Assicura che i file di configurazione siano leggibili solo dal proprietario
- **Default**: false
- **Consiglio**: Metti `true` su server condivisi o importanti

### Connessioni Web

**N8N_SECURE_COOKIE**
- **Cosa fa**: I cookie di login funzionano solo con connessione sicura (https)
- **Default**: true
- **Consiglio**: Lascia `true` se usi https

**N8N_SAMESITE_COOKIE**
- **Cosa fa**: Controlla quando il browser invia i cookie
- **Valori**: `strict`, `lax`, `none`
- **Default**: lax
- **Quale usare**:
  - `strict`: Massima sicurezza, ma alcuni link esterni potrebbero non funzionare
  - `lax`: Buon compromesso (lascia questo)
  - `none`: Solo se n8n è incorporato in altri siti

### Nodi Pericolosi

**N8N_GIT_NODE_DISABLE_BARE_REPOS**
- **Cosa fa**: Impedisce l'uso di repository Git "bare" che possono essere rischiosi
- **Default**: false
- **Consiglio**: Metti `true` se hai utenti non fidati

**N8N_SECURITY_AUDIT_DAYS_ABANDONED_WORKFLOW**
- **Cosa fa**: Dopo quanti giorni senza modifiche un workflow è considerato "abbandonato"
- **Default**: 90 giorni
- **A cosa serve**: Per sapere quali workflow nessuno usa più

---

## 5. NODI (Componenti dei Workflow)

### Nodi dalla Community

**N8N_COMMUNITY_PACKAGES_ENABLED**
- **Cosa fa**: Permette di installare nodi creati da altre persone
- **Default**: true
- **Consiglio**: Lascia `true` per avere più funzionalità, metti `false` per massima sicurezza

**N8N_UNVERIFIED_PACKAGES_ENABLED**
- **Cosa fa**: Permette nodi non verificati dal team n8n
- **Default**: true
- **Quando disattivarlo**: Se usi n8n per dati aziendali sensibili

**N8N_COMMUNITY_PACKAGES_PREVENT_LOADING**
- **Cosa fa**: Non carica i nodi community all'avvio
- **Default**: false
- **Quando attivarlo**: Se un nodo difettoso impedisce a n8n di avviarsi (situazione rara)

### Nodo Code (Programmazione)

**N8N_PYTHON_ENABLED**
- **Cosa fa**: Permette di scrivere codice Python nei nodi Code
- **Default**: true
- **Quando disattivarlo**: Se vuoi che gli utenti usino solo JavaScript

**NODE_FUNCTION_ALLOW_BUILTIN**
- **Cosa fa**: Permette di usare funzioni avanzate di JavaScript/Python
- **Default**: disabilitato
- **Esempio**: `*` (permetti tutto)
- **⚠️ Sicurezza**: Attiva solo se ti fidi degli utenti

**NODE_FUNCTION_ALLOW_EXTERNAL**
- **Cosa fa**: Permette di usare librerie esterne nel codice
- **Default**: disabilitato
- **Quando attivarlo**: Se hai installato librerie aggiuntive e vuoi usarle

### Bloccare Nodi Pericolosi

**NODES_EXCLUDE**
- **Cosa fa**: Lista di nodi che NON vuoi rendere disponibili
- **Esempio**: `["n8n-nodes-base.executeCommand"]`
- **Quando usarlo**: Il nodo "Execute Command" permette di eseguire comandi sul server - bloccalo se hai utenti non fidati

---

## 6. DIMENSIONI E LIMITI

### File e Dati

**N8N_PAYLOAD_SIZE_MAX**
- **Cosa fa**: Dimensione massima dei dati che un workflow può gestire (in megabyte)
- **Default**: 16 MB
- **Come sceglierlo**:
  - Lavori solo con testo/API: lascia `16`
  - Carichi immagini: `64`
  - Carichi PDF o video piccoli: `128`
  - Elabori file grandi: `256` o più
  - ⚠️ Più alto = più RAM usata

**N8N_FORMDATA_FILE_SIZE_MAX**
- **Cosa fa**: Dimensione massima file caricati via webhook
- **Default**: 200 MB
- **Quando abbassarlo**: Se hai poca RAM o non carichi mai file grossi

### Monitoraggio

**N8N_METRICS**
- **Cosa fa**: Attiva una pagina speciale che mostra statistiche sul funzionamento di n8n
- **Default**: false
- **Quando attivarlo**:
  - Se usi strumenti di monitoraggio tipo Grafana/Prometheus
  - Se vuoi capire come va il server
  - Se hai più di 10 workflow attivi

---

## 7. WORKFLOW

### Nomi e Attivazione

**WORKFLOWS_DEFAULT_NAME**
- **Cosa fa**: Nome che appare quando crei un nuovo workflow
- **Default**: "My workflow"
- **Consiglio**: Cambialo in qualcosa che ti piace di più

**N8N_WORKFLOW_ACTIVATION_BATCH_SIZE**
- **Cosa fa**: Quanti workflow attivare insieme all'avvio di n8n
- **Default**: 1 (uno alla volta)
- **Quando aumentarlo**:
  - Se hai tanti workflow (più di 20) e l'avvio è lento: metti `3` o `5`
  - Server con 8GB+ RAM: metti `10`

**N8N_ONBOARDING_FLOW_DISABLED**
- **Cosa fa**: Nasconde i suggerimenti per principianti
- **Default**: false (suggerimenti attivi)
- **Quando disattivarlo**: Se sei esperto e i suggerimenti ti danno fastidio

### Condivisione e Sicurezza

**N8N_WORKFLOW_CALLER_POLICY_DEFAULT_OPTION**
- **Cosa fa**: Chi può far partire un workflow chiamandolo da un altro workflow
- **Valori**:
  - `workflowsFromSameOwner`: solo workflow dello stesso creatore (default)
  - `any`: chiunque
  - `none`: nessuno
  - `workflowsFromAList`: solo quelli in una lista specifica
- **Consiglio**: Lascia `workflowsFromSameOwner` per sicurezza

---

## 8. LICENZA ENTERPRISE

Queste impostazioni servono solo se hai acquistato una licenza n8n Enterprise.

**N8N_LICENSE_ACTIVATION_KEY**
- **Cosa fa**: Il codice della tua licenza enterprise
- **Quando serve**: Solo se hai comprato funzionalità avanzate

**N8N_LICENSE_AUTO_RENEW_ENABLED**
- **Cosa fa**: Rinnova automaticamente la licenza ogni tot giorni
- **Default**: true
- **Consiglio**: Lascia `true` per non doverti ricordare di rinnovare manualmente

**N8N_LICENSE_DETACH_FLOATING_ON_SHUTDOWN**
- **Cosa fa**: Rilascia la licenza quando spegni n8n (per poterla usare altrove)
- **Default**: true
- **Quando metterlo false**: Se questo è il tuo server principale e deve sempre avere la licenza

---

## 9. MODALITÀ AVANZATA (Queue Mode)

⚠️ **Attenzione**: Questa modalità è per server potenti con utenti esperti. Non attivarla se stai iniziando.

**EXECUTIONS_MODE**
- **Cosa fa**: Come n8n gestisce l'esecuzione dei workflow
- **Valori**: `regular` (normale) o `queue` (avanzata con Redis)
- **Default**: regular
- **Quando usare queue**:
  - Server con 4GB+ RAM
  - Hai workflow che durano più di 5 minuti
  - Hai più di 50 esecuzioni al giorno
  - N8n si blocca spesso
  - ⚠️ Richiede installare e configurare Redis (database aggiuntivo)

### Impostazioni Redis (solo se usi Queue Mode)

**QUEUE_BULL_REDIS_HOST**
- **Cosa fa**: Dove si trova Redis
- **Esempio**: `localhost` o `redis.miosito.com`

**QUEUE_BULL_REDIS_PORT**
- **Cosa fa**: Porta di Redis
- **Default**: 6379

**QUEUE_BULL_REDIS_PASSWORD**
- **Cosa fa**: Password per accedere a Redis
- **Consiglio**: Metti sempre una password forte!

**OFFLOAD_MANUAL_EXECUTIONS_TO_WORKERS**
- **Cosa fa**: Anche i workflow avviati manualmente usano il sistema avanzato
- **Default**: false
- **Quando attivarlo**: Se hai worker configurati e vuoi più affidabilità

---

## CONSIGLI GENERALI PER DIVERSE SITUAZIONI

### Server Piccolo (2GB RAM, 1-2 CPU)
```
EXECUTIONS_TIMEOUT=3600
EXECUTIONS_DATA_MAX_AGE=336
N8N_PAYLOAD_SIZE_MAX=32
DB_TYPE=postgresdb
DB_POSTGRESDB_POOL_SIZE=2
N8N_CONCURRENCY_PRODUCTION_LIMIT=2
```

### Server Medio (4GB RAM, 2-4 CPU)
```
EXECUTIONS_TIMEOUT=7200
EXECUTIONS_DATA_MAX_AGE=720
N8N_PAYLOAD_SIZE_MAX=64
DB_POSTGRESDB_POOL_SIZE=4
N8N_CONCURRENCY_PRODUCTION_LIMIT=5
```

### Server Grande (8GB+ RAM, 4+ CPU)
```
EXECUTIONS_TIMEOUT=14400
EXECUTIONS_DATA_MAX_AGE=2160
N8N_PAYLOAD_SIZE_MAX=128
DB_POSTGRESDB_POOL_SIZE=8
N8N_CONCURRENCY_PRODUCTION_LIMIT=10
```

### Massima Sicurezza
```
N8N_BLOCK_ENV_ACCESS_IN_NODE=true
N8N_BLOCK_FILE_ACCESS_TO_N8N_FILES=true
N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
N8N_GIT_NODE_DISABLE_BARE_REPOS=true
N8N_COMMUNITY_PACKAGES_ENABLED=false
N8N_UNVERIFIED_PACKAGES_ENABLED=false
NODES_EXCLUDE=["n8n-nodes-base.executeCommand"]
```

### Massime Prestazioni (ma meno sicuro)
```
EXECUTIONS_DATA_SAVE_ON_SUCCESS=none
EXECUTIONS_DATA_SAVE_ON_PROGRESS=false
N8N_DIAGNOSTICS_ENABLED=false
N8N_BLOCK_ENV_ACCESS_IN_NODE=false
```

---

## DOMANDE FREQUENTI

**"Quale database usare?"**
- Uso personale o test: SQLite va bene
- Uso serio (lavoro, progetti importanti): PostgreSQL

**"Quanta RAM serve?"**
- Uso leggero (pochi workflow): 2GB vanno bene
- Uso normale: 4GB consigliati
- Uso intenso: 8GB o più

**"Devo attivare Queue Mode?"**
- NO se stai iniziando
- NO se hai meno di 4GB RAM
- SÌ se i workflow si bloccano spesso
- SÌ se hai molti utenti simultanei

**"Come proteggo i dati sensibili?"**
- Usa sempre HTTPS (N8N_PROTOCOL=https)
- Attiva N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
- Usa PostgreSQL con password forte
- Se possibile, blocca accesso ambiente e file

**"Quanto spazio disco serve?"**
- Dipende da quante esecuzioni tieni:
- 7 giorni di storico: circa 1-5GB
- 30 giorni: circa 5-20GB
- 6 mesi: 50GB+

**"Come evito che il disco si riempia?"**
- Lascia EXECUTIONS_DATA_PRUNE=true
- Regola EXECUTIONS_DATA_MAX_AGE in base allo spazio
- Monitora lo spazio ogni tanto

---

## NOTE TECNICHE FINALI

### Suffisso _FILE
Se hai password in file separati invece che nelle variabili:
```
DB_POSTGRESDB_PASSWORD_FILE=/percorso/al/file/password
```

### Maiuscole vs minuscole per proxy
Le variabili minuscole (`http_proxy`) hanno priorità su quelle maiuscole (`HTTP_PROXY`).
