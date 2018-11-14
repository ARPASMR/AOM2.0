# AOM2.0
Ambiente Operativo Meteo 2.0

# Scopo
Qui si trova la documentazione del progetto suddivisa in sezioni

## How-to
Sezione dedicata alle cose pratiche su come fare più rapidamente le cose

## Schema generale
Lo schema generale dell'infrastruttura, dei servizi, delle applicazioni

## Manuali
Documentazione estesa che descrivono l'infrastruttura, i servizi, le applicazioni.

Possono essere linkate direttamente le risorse (es. repositories)

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
