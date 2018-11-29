# POSTGRES
Per accedere ai database IRIS
```
psql -U postgres -d iris_base -W
```
__comandi utili__
- \d elenca le tabelle (in generale tutte le relazioni)
- \l elenca tutti i database
- \q esce da psql
- \? help dei comandi

Per effettuare il dump dello schema (ma non dei dati), da _sinergico-03_
```
/usr/pgsql-9.3/bin/pg_dump -f iris_base_dump -s -U postgres iris_base
```
- lo schema viene salvato nel file _iris_base_dump_

Per il restore del DATABASE
```
createdb -T template0 iris_base
psql --set ON_ERROR_STOP=on iris_base < iris_base_dump
```
