# E-commerce Web Application

Progetto universitario per il corso di **Tecnologie e Software per il Web**.
Applicazione web di e-commerce per la vendita di prodotti apistici e attrezzatura per l'apicoltura, realizzata con architettura **MVC** basata su tecnologie Java EE standard.

---

## Descrizione del progetto

L'applicazione permette agli utenti di sfogliare un catalogo di prodotti suddivisi in categorie (*Prodotti dell'alveare* e *Materiale apistico*), aggiungere articoli al carrello, effettuare ordini e consultare lo storico degli acquisti. È presente anche un pannello di amministrazione per la gestione di prodotti, categorie e ordini.

**Funzionalità principali:**
- Registrazione e autenticazione utenti (con hashing della password)
- Catalogo prodotti con filtro per categoria
- Carrello con aggiornamento dinamico via AJAX (senza ricaricamento della pagina)
- Checkout e conferma ordine
- Storico ordini per utenti autenticati
- Pannello admin per gestione prodotti, categorie e ordini
- Gestione delle scorte (quantità disponibile)

---

## Tecnologie utilizzate

### Ambiente di sviluppo

| Strumento | Versione | Utilizzo |
|-----------|----------|---------|
| **Eclipse IDE** | 2024 (Jakarta EE) | IDE principale per sviluppo Java e configurazione del progetto |
| **Apache Tomcat** | v10.1 | Application server per il deployment dei Servlet e delle JSP |
| **MariaDB / MySQL** | 11.6.2 | Database relazionale per la persistenza dei dati |
| **Java SE** | 21 | Linguaggio e runtime principale |

### Back-end

| Tecnologia | Descrizione |
|-----------|-------------|
| **Jakarta Servlet 6.0** | Gestione delle richieste HTTP tramite classi `HttpServlet` con annotazioni `@WebServlet` |
| **JSP (JavaServer Pages)** | Template engine lato server per la generazione dinamica dell'HTML |
| **JDBC** | Accesso al database tramite `java.sql.*` con connection pool custom (`DriverManagerConnectionPool`) |
| **MySQL Connector/J 9.4.0** | Driver JDBC per la connessione a MariaDB/MySQL |
| **Pattern MVC** | Architettura a tre livelli: Servlet (Controller), classi Java (Model/DAO), JSP (View) |
| **Pattern DAO** | Separazione della logica di accesso ai dati tramite classi `*DAO` e bean `*Bean` |
| **Session Management** | Gestione dello stato utente e del carrello tramite `HttpSession` |

### Front-end

| Tecnologia | Descrizione |
|-----------|-------------|
| **HTML5** | Markup semantico delle pagine |
| **CSS3** | Stilizzazione con layout Flexbox/Grid, design responsive |
| **JavaScript (ES5)** | Validazione lato client dei form e gestione AJAX del carrello |
| **AJAX (XMLHttpRequest)** | Aggiornamento del carrello e del counter navbar senza ricaricare la pagina |

### Database

| Tecnologia | Descrizione |
|-----------|-------------|
| **SQL** | Linguaggio per la definizione e manipolazione dei dati |
| **Schema relazionale** | 5 tabelle normalizzate: `utente`, `categoria`, `prodotto`, `ordine`, `dettaglio_ordine` |
| **Deployment descriptor** | Configurazione dell'applicazione tramite `web.xml` (Jakarta EE 6.0) |

---

## Architettura MVC

```
src/main/java/
├── control/          ← Controller (7 Servlet)
│   ├── LoginServlet.java
│   ├── RegistrazioneServlet.java
│   ├── CarrelloServlet.java
│   ├── CheckoutServlet.java
│   ├── LogoutServlet.java
│   ├── ProductManagementServlet.java
│   └── CategoryManagementServlet.java
└── model/            ← Model (Bean + DAO)
    ├── UserBean.java / UserDAO.java
    ├── ProductBean.java / ProductDAO.java
    ├── CategoryBean.java / CategoryDAO.java
    ├── OrderBean.java / OrderDAO.java
    ├── CarrelloBean.java
    └── DriverManagerConnectionPool.java

WebContent/
├── *.jsp             ← View (13 pagine JSP)
├── admin/*.jsp       ← Pannello amministrazione
├── styles/           ← CSS (7 file)
├── scripts/          ← JavaScript (3 file)
└── WEB-INF/
    ├── web.xml
    └── lib/mysql-connector-j-9.4.0.jar
```

---

## Schema del database

Il database `apicoltura_db` è composto da 5 tabelle:

- **`utente`** — dati anagrafici, credenziali (password hashata), indirizzo di spedizione, flag admin
- **`categoria`** — categorie dei prodotti
- **`prodotto`** — nome, descrizione, prezzo, IVA, quantità disponibile, immagine, soft-delete
- **`ordine`** — ordini effettuati dagli utenti con stato e totale
- **`dettaglio_ordine`** — righe d'ordine con quantità e snapshot del prezzo al momento dell'acquisto

Lo script SQL per la creazione e il popolamento del database si trova in [`docs/db.sql`](docs/db.sql).

---

## Avvio del progetto

### Prerequisiti
- **Java SE 21** installato
- **Apache Tomcat 10.1** installato e configurato in Eclipse
- **MariaDB / MySQL** in esecuzione in locale
- **Eclipse IDE for Enterprise Java and Web Developers**

### Configurazione

1. Importare il progetto in Eclipse come *Existing Project into Workspace*
2. Creare il database e caricare lo schema:
   ```sql
   mysql -u root -p < docs/db.sql
   ```
3. Aggiornare le credenziali di connessione al database in `DriverManagerConnectionPool.java`:
   ```java
   private static final String URL = "jdbc:mysql://localhost:3306/apicoltura_db";
   private static final String USER = "root";
   private static final String PASSWORD = "your_password";
   ```
4. Aggiungere Apache Tomcat v10.1 come server in Eclipse (*Servers → New → Apache → Tomcat v10.1*)
5. Eseguire il progetto con *Run As → Run on Server*

