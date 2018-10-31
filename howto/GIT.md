# GIT Tips and tricks
## Spiegone

Git è la piattaforma web che archivia e registra tutte le modifiche del software.

Per usare git occorre avere un client per git che può essere installato sia x Windows che per linux

https://help.github.com/desktop/guides/getting-started-with-github-desktop/installing-github-desktop/

Il metodo di lavoro si compone di tre fasi:
1. **pull**: scarico il repository da github.com/ARPASMR/<nome del repository> (_solo la prima volta che uso quel pv/server/ecc._)
2. **fork**: eseguo le modifiche sul codice
3. **push**: notifico le modifiche a github (_push_)

Questo schema ha diverse varianti che dipendono da quanti collaborano a quel codice. In questi casi è il process-owner che determina la variante da applicare

## fork
Ogni volta che si esegue una copia del repository questo è un "fork". 

Il process owner disciplina come si lavora sui fork, come si fa a creare un branch come si fa una pull request

## branch
Si tratta di una variante _riconosciuta_ del repository. 

Il process owner disciplina come si lavora sui fork, come si fa a creare un branch come si fa una pull request.
Ogni volta che si esegue 1. si scaricano tutti i branch nel proprio fork.


# Esempi
scarico per la prima volta o _clone_
```
git clone https://github.com/ARPASMR/<nome del repository>
```
ho già il codice sulla mia macchina e voglio allinearmi con il repository _fare sempre prima di lavorare sul codice_
```
git pull 
```
aggiungere o aggiornare il codice _fare sempre prima del push_

commentare il codice prima di notificare una variazione _commit: da fare sempre_

notificare le modifiche _push_

# Inizializzare un repository in locale e aggiungerlo ad ARPASMR
_non consigliato_

# Uso di .gitignore

# Come organizzare i file nei repository
