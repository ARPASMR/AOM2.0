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
