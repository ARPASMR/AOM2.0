# Docker how-to
## spiegone
Docker è uno strumento per la gestione e l'esecuzione dei container.
La versione installata in ARPA è di tipo Enterprise con supporto.
Per ottenere il supporto rivolgersi al AOM Capitain

Docker è installato in un ambiente _cluster_ ovvero su 5 macchine di cui 3 _manager_ e 2 _workers_
_Nota: l'infrastruttura UCP può essere soggetta a cambiamenti_

Docker insieme a _dockerhub_ e _Jenkins_ realizza il paradigma _CI/CD_ (Continuous Integration/ Continuous Deployment)

# Infrastruttura ARPA
rete dei database: _database_
denominazione degli host:
_database_.testv.arpa.local per i database nel'ambiente di test
_database_.meteo.arpa.local per i database in produzione
Tutti i server di prova sono nella rete 10.10.99.104
Quelli in produzione 10.10.99.105
(da verificare)

# migrazione: dai servizi monolitici ai microservizi
1. creazione della infrastruttura di rete
2. realizzazione delle repliche dei database
   1. replica di dbMeteo in cluster swarm (MariaDB)
   2. replica di dbAIB in cluster swarm (Postgres)
   3. containerizzazione di wikiss con dB (MariaDb o MySQL)
   4. containerizzazione del dBSyslog (MariaDb o MySQL)
3. migrazione di AIB su cluster e abbandono di Milanone
   1. deploy servizi rasdaman per AIB
4. revamping di Milanone e inserimento nel cluster swarm
5. riscrittura di OI (su dati orari e connessione a dbMeteo) e creazione di microservizio OI. Il microservizio può essere parametrico in modo che diverse istanze dello stesso codice realizzino la stessa funzione che attualmente hanno le diverse versione del codice (da valutare)
6. passaggio dei servizi di libertario in microservizio
7. revamping di libertario e inserimento nel cluster swarm
8. reindirizzamento di meteo.arpalombardia.it su ghost
9. eliminazione da tutto il codice della copia dei file su "ghost esterno"
10. containerizzazione dei servizi per il dBMeteo in sinergico02
11. spostamento/containerizzazione applicazioni da sinergico02 e abbandono di sinergico02

# creazione della rete in modalità overlay attachable
```
docker network create -d overlay --attachable --subnet=10.1.9.0/24 mynet
```

# Deploy di un database
Il database può essere _effimero_ (quando si ferma il container i dati vengono persi) oppure _persistente_ (quando si ferma il container i dati rimangono nell'host su cui docker ha lanciato il container); normalmente si usano database persistenti.

Il database persistente può essere lanciato con un server di una certa versione (es. Postgres 9.3) e rilanciato con un server di un'altra versione  (purchè vi sia compatibilità dei file di dati, cosa in genere garantita).
Dividere i dati dal server facilita le operazioni di migrazione da un sistema ad un altro.

## lancio dell'istanza di postgres sulla rete
```
docker run --name my-postgres --network mynet -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```
## lancio dell'istanza di amministrazione da un altro server
```
docker run -it --rm --network mynet --link my-postgres:postgres postgres psql -h postgres -U postgres
```
