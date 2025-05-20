# utenti-app-test-linux

#✅ README — Configurazione utenti app e test su Linux
##🎯 Obiettivo
Creare l’utente app con home in /opt/app

Creare un file accessibile solo al proprio utente e ad app

Creare l’utente test senza accesso sudo

##📌 1. Creazione utente app con home personalizzata
```bash
sudo useradd -m -d /opt/app app
sudo passwd app
```
-m: crea la directory home se non esiste

-d /opt/app: specifica una home directory personalizzata

passwd: imposta una password per l’utente app

✅ Verifica:

```bash
getent passwd app
ls -ld /opt/app
```
##📌 2. Creazione di un file accessibile solo al proprio utente e ad app
###✅ 2.1 Creazione del file

`sudo touch /opt/app/privato.txt`
###✅ 2.2 Assegnazione del proprietario a app

`sudo chown app /opt/app/privato.txt`
(In alternativa, per permettere accesso anche al tuo utente, puoi creare un gruppo condiviso, vedi nota sotto.)

###✅ 2.3 Impostazione dei permessi stretti (solo proprietario e gruppo)

`sudo chmod 660 /opt/app/privato.txt`
Solo app (e gruppo) può leggere/scrivere il file

Nessun altro utente ha accesso

📎 Nota: Per far accedere anche un altro utente (es. omenicoarde) potresti creare un gruppo condiviso:

```bash

sudo groupadd appshare
sudo usermod -aG appshare app
sudo usermod -aG appshare omenicoarde
sudo chown omenicoarde:appshare /opt/app/privato.txt
sudo chmod 660 /opt/app/privato.txt
```
##📌 3. Creazione utente test senza privilegi sudo

`sudo adduser test`
Segui le istruzioni per inserire la password

Inserisci informazioni facoltative (nome, telefono, ecc.)

✅ Verifica gruppi:

`groups test`
Assicurati che non sia in sudo. Se lo fosse, lo rimuoveresti così:


`sudo deluser test sudo`
Nel tuo caso, il messaggio:


`fatal: The user `test' is not a member of group `sudo`.`
conferma che test non ha accesso sudo.

##✅ Risultato finale
Utente	Home	Accesso sudo	Note
app	/opt/app	❌	File privato.txt accessibile
test	/home/test	❌	Utente normale, senza privilegi sudo

📂 File protetto
Percorso: /opt/app/privato.txt

Accessibile solo a: utente app (e opzionalmente omenicoarde se usi gruppo condiviso)

Permessi: -rw-rw---- (chmod 660)
