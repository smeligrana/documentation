Come coollegarsi ad mongodb come admin 
```
mongosh -u admin -p password123 --authenticationDatabase admin
```
Selezionare il database su cui vuoi che l'utente lavori se non esiste lo crea
```
use test_db
```

Crea l'utente con i permessi di lettura e scrittura solo su questo database
```
db.createUser({
  user: "sviluppatore1",
  pwd: "PasswordSicura456",
  roles: [
    { role: "readWrite", db: "test_db" }
  ]
})
```

Crea un primo documento nella collection users
```
db.users.insertOne({ nome: "Mario", ruolo: "Admin" })
```

|Ruolo||Cosa può fare?|
|------------ |------------ |
|read|Può solo leggere i dati.|
|------------ |------------ |
|readWrite|Può leggere e modificare i dati (il più usato per le app).|
|------------ |------------ |
|dbAdmin|Può gestire indici e statistiche, ma non legge i dati.|
|------------ |------------ |
|userAdmin|Può creare altri utenti sul database specifico.|
|------------ |------------ |
|root,Permessi totali su tutto il sistema (da usare con cautela).|

