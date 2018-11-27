# Schema generale dell'infrastruttura

## prerequisiti
Per lanciare ansible occorre aver fatto lo scambio delle chiavi ssh sui server 

## iaas - infrastructure as a service
Contiene i playbook di ansible per la creazione dell'infrastruttura.
Per eseguire i comandi di docker attraverso ansuible deve essere installato il pacchetto docker-py di python
```
pip install docker-py
```

Il file __b_db.yaml__ contiene l'esempio funzionante del lancio di un container postgres su un server come definito in _servers_.
La prova è stata fatta lanciando il playbook dal 10.10.99.135 e creando il container sul 10.10.99.136
