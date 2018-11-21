# Ciclo di sviluppo

## spiegazione generale
Il ciclo di sviluppo del codice è orientato al _paradigma CI/CD_
In altri termini il ciclo di sviluppo del codice (o _Development Pipeline DP_) è pensato in modo che il passaggio dallo sviluppo alla produzione avvenga nel minor tempo possibile e che i rilasci del codice siano frequenti.
Per fare questo occorre tenere presente che:
1. il codice è gestito sotto github nel repository generale ARPASMR;
2. il codice è accompagnato da un file, _Dockerfile_, che specifica quali sono le attività che vengono fatte automaticamente per la containerizzazione di quel codice;
3. il codice può essere accompagnato da un file bash che determina le modalità di esecuzione del codice; questo file bash viene lanciato esplicitamente o implicitamente (se si chiama entrypoint.sh) nel momento dell'esecuzione del container;
_TOBE_ il codice viene automaticamente eseguito in un ambiente di _Continuous Deployment_

## quali sono gli elementi di base
Gli elementi di base sono:
1. il __codice__ (o i codici) che eseguono determinate attività; possono essere in R, Python, perl o qualunque tipo di linguaggio, in genere di scripting; tutti i file sono contenuti nel repository (es. ARPASMR/fulmini)
2. il ___Dockerfile___ che contiene le informazioni per la creazione dell'immagine docker che esegue le attività nel codice; l'applicazione verrà eseguita attraverso il comando di docker run, come ad esempio:
```
docker run -it <nome dell'immagine> -e "<variabile d'ambiente>=<valore della variabile"
```
Anche il _Dockerfile_ è contenuto nel repository
3. L'__immagine__ di docker: viene costruita automaticamente attraverso il collegamento del repository di github con il repository di docker, detto anche _docker trusted registry DTR_; in realtà questo repository è pubblico e consente a chiunque di scaricare l'immagine docker ed eseguire l'applicazione. Tuttavia, chi esegue l'applicazione deve fornire quei parametri indispensabili per l'applicazione stessa, che sono generalmente conservati nelle variabili d'ambiente (e che devono essere conosciute e adeguate all'architettura dove viene eseguito il codice, ad esempio nomi di database, indirizzi ip, password di database, ecc.)

## ciclo di sviluppo : semplice
