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
Anche il _Dockerfile_ è contenuto nel repository.
3. L'__immagine__ di docker: viene costruita automaticamente attraverso il collegamento del repository di github con il repository di docker, detto anche _docker trusted registry DTR_; in realtà questo repository è pubblico e consente a chiunque di scaricare l'immagine docker ed eseguire l'applicazione. Tuttavia, chi esegue l'applicazione deve fornire quei parametri indispensabili per l'applicazione stessa, che sono generalmente conservati nelle variabili d'ambiente (e che devono essere conosciute e adeguate all'architettura dove viene eseguito il codice, ad esempio nomi di database, indirizzi ip, password di database, ecc.)

## ciclo di sviluppo : semplice
1. creazione del repository su github e di un file (README.md) che descrive cosa fa l'applicazione, quali sono i prerequisiti, ecc.
2. clone del repository in locale (_fork_): attraverso il comando di git si può copiare quanto è presente online in modo da poter fare le modifiche (si veda __GIT__ per i dettagli), ad esempio:
```
$ git clone https://github.com/ARPASMR/<nome del repository>
$ cd <nome del repository>
$ ls -la
_elenco dei file del repository_
```
Prima di fare questo comando occorre avere chiaro che il comando creerà una cartella nella cartella dove viene lanciato con il <nome del repository>. Lavorare in questa cartella vuol dire creare e/o modificare i file dell'applicazione.
Se si lavora sotto Windows probabilmente il comando di _git clone_ verrà eseguito attraverso l'interfaccia grafica e sarà un po' più complicato ricordarsi in quale cartella si vuole lavorare.

__Suggerimento__ si suggerisce di creare una cartella _Projects_ nella cartella _/home/meteo_ di un server oppure nella cartella _C:\Users\<nome utente>\Documents and Settings_

3. una volta che si è copiato il codice si può eseguire con l'interprete scripting corrispondente: se non c'è alcun codice si può crearlo nella cartella del repository; ad esempio:
```
$ pwd
$ /home/meteo/Projects/<nome del repository>
$ ls
$ README.md
$ vim <mia applicazione>.R
_ scrivo tutto quello che mi serve_
```
4. commit: eseguo il commit e il push delle modifiche sul repository (vedere __GIT__)
5. creazione del _Dockerfile_
Una volta che ho creato sufficiente codice e voglio testarne l'esecuzione passo a definire il _Dockerfile_; nel _Dockerfile_ verranno contenute tutte le informazioni necessarie a docker per creare un'immagine autoconsistente del codice, dei prerequisiti (es. il programma R che esegue il codice), le variabili d'ambiente (no password in chiaro!), la struttura delle directory, ecc. Il più semplice _Dockerfile_ è il seguente:
```
FROM arpasmr/r-base
WORKDIR /usr/src/myapp
COPY <mia applicazione>.R ./
```
La prima riga dice a docker di prendere un immagine di base che contiene già installate tutte le librerie che servono; possono essere immagini nel _DTR_ oppure immagini _ufficiali_ (es. docker.io/debian, l'immagine _latest_ di debian contenuta nel repository generale di docker e garantita da debian);
la seconda riga dice a docker di creare una directory e di farla diventare la directory da cui verrà eseguito il codice;
la terza riga dice a docker di copiare il codice di R nella _workdir_
6. creazione dell'__immagine__: si può procedere con un build locale, come ad esempio:
```
$ docker build -t <mia immagine> .
```
oppure, cosa molto più comoda, lasciando che della creazione dell'immagine se ne occupi dockerhub. Per questo occorre _legare_ il repository di github a quello di dockerhub: fatto la prima volta poi non sarà più necessario.
7. una volta creata l'immagine e messa l'immagine nel _DTR_ sarà sufficiente eseguire il comando __da un qualunque server in cui sia installato docker__:
```
$ docker run -it --rm arpasmr/<nome dell'immagine> /bin/bash
<id_del_container>$ pwd
<id_del_container>$ /usr/src/myapp
<id_del_container>$ ls
<id_del_container>$ <mia applicazione>.R
```
In pratica si è chiesto a docker di utilizzare l'immagine arpasmr/<nome dell'immagine> e di lanciare una shell di bash al suo interno. A questo punto per eseguire lo script posso semplicemente eseguire il comando:
```
<id_del_container>$ Rscript <mia applicazione>.R
<id_del_container>$ _ output _
```
7. se voglio modificare il codice R (o python, o qualunque) mi basterà tornare nel _fork_ di git, eseguire le modifiche, eseguire di nuovo il commit/push
8. git, acquisite le modifiche, dirà a dockerhub di creare una nuova __immagine__
9. posso scaricare localmente la nuova immagine:
```
docker pull arpasmr/<mia immagine>
```
ed eseguire nuovamente il passo 7.
