# scrippellapp-landing

Il sito di Scrippellapp, `https://www.scrippellapp.it/`, pubblicato con GitHub Pages (D-80). La repo è pubblica perché GitHub Pages è gratuito solo dalle repo pubbliche. Qui non ci sono codice dell'app né segreti.

Riferimenti: piano di sviluppo (Fase 4, prova P2, e Fase 11) e FDD nella cartella `DOC` della repo `scrippellapp`, marchio in `DOC/MARCHIO` della stessa repo.

## Struttura

```
scrippellapp-landing/
├── index.html              pagina principale
├── privacy.html            informativa sulla privacy, anche per le botteghe
├── termini.html            termini di servizio, con l'accordo art. 28 per l'evento (#accordo)
├── elimina-account.html    eliminazione di account e dati, richiesta da Google Play
├── accessibilita.html
├── invito/index.html       link di invito dell'app (D-110), fuori dai motori di ricerca
├── invito-staging/index.html   lo stesso per l'app di prova, senza il bottone di Play
├── .well-known/
│   ├── apple-app-site-association   le due app di iPhone sui due percorsi di invito
│   └── assetlinks.json     i due pacchetti Android con le impronte SHA-256 delle firme
├── assets/
│   ├── site.css            stili condivisi da tutte le pagine
│   ├── scrippellapp-logo.svg   logo delle pagine interne, da DOC/MARCHIO/logo-completo
│   └── fonts/              Plus Jakarta Sans, licenza in OFL.txt
├── favicon.ico, favicon.svg, favicon-32.png, apple-touch-icon.png,
│   icon-192.png, icon-512.png, icon-maskable-512.png,
│   site.webmanifest, og-image-1200x630.png      copiati da DOC/MARCHIO/web, senza modifiche
├── CNAME                   www.scrippellapp.it
├── .nojekyll               senza, GitHub Pages non pubblica le cartelle che iniziano con il punto
├── robots.txt
└── sitemap.xml
```

Da aggiungere, come nel piano di sviluppo:

- Fase 11: link alle pagine legali dall'app (`config/legal.ts` come Memura).

Le pagine legali sono bozze del 12 ottobre 2026, scritte dal FDD (cap. 15) e dai testi di Memura: prima di pubblicarle le rivede chi segue la parte legale. Ogni pagina ha la data in alto: quando cambia un testo si aggiornano la data e `sitemap.xml`. Il piede è uguale in tutte le pagine; nella pagina corrente il suo link ha `aria-current="page"`.

## Regole

- Colori e font solo dalle variabili in testa a `assets/site.css`. I valori sono quelli di `apps/mobile/src/theme` e di `DOC/MARCHIO/LEGGIMI.md` (palette Carta e Oro).
- Il logo in `index.html` è quello di `DOC/MARCHIO/logo-completo`, scritto dentro la pagina. All'apertura la spirale si stende dal centro come nello `ScrippellaLoader` dell'app (D-94), poi arrivano zucchero e cannella. Con "Riduci movimento" il logo compare subito intero. Se il marchio cambia, si ricopiano i tracciati da `MARCHIO`.
- Solo italiano e solo tema chiaro, come l'app (D-09).
- Niente numeri decisi da noi (scrippelle per giro, giorni dei giri, durata e livelli degli accordi con le botteghe): restano solo i tempi che la legge chiede di scrivere, come la conservazione dei dati (D-118).
- Script solo dentro le pagine e solo tre: in `index.html` uno anima l'esempio dei giri in apertura, si ferma con "Riduci movimento" e quando l'esempio non è sullo schermo, e uno legge le botteghe attive da `/sito/botteghe` del Worker e mette i loghi nei posti tratteggiati, senza link (D-95, Fase 10), e con la stessa risposta mostra il patrocinio del Comune (piccolo nell'intestazione e nel piede, Fase 15) se è acceso dalla console (D-202); quello di poche righe delle pagine di invito legge il codice dal link e lo scrive nella pagina e nel link di Google Play (D-110); in `invito/`, al tocco di "Scarica da App Store", copia negli appunti il link intero, che l'app per iPhone legge al primo avvio (D-246), poi apre lo store anche se la copia non riesce. Il solo servizio esterno è il Worker di Scrippellapp, per i loghi: niente statistiche di terzi, niente cookie, quindi nessun banner. Il font è servito dal sito e non da Google Fonts, così l'indirizzo IP di chi visita non va a terzi.
- L'esempio dei giri in apertura è un disegno, non una schermata dell'app: case, giri e iniziali sono inventati. Il fotogramma fermo scritto nella pagina è quello che vede chi ha "Riduci movimento".
- Percorsi assoluti (`/assets/site.css`), così le pagine nelle sottocartelle, come `invito/`, usano gli stessi file.
- Prima di pubblicare: pagina leggibile su telefono, nessun errore nella console, controllo di accessibilità (axe) senza violazioni.

## Sviluppo locale

```sh
python3 -m http.server 8080
# http://localhost:8080
```

## Pubblicazione

GitHub Pages pubblica il ramo `main`. Si lavora su `development` e si unisce in `main` per pubblicare. Dopo ogni push GitHub ripubblica in meno di un minuto.

### Prima volta

1. Repo `tech-scrippellapp/scrippellapp-landing`: **Settings → Pages → Build and deployment**. Scegliere **Deploy from a branch**, ramo `main`, cartella `/ (root)`.
2. **Custom domain** si compila da solo con `www.scrippellapp.it`, letto dal file `CNAME`.
3. DNS su register.it (Fase 0.1), zona di `scrippellapp.it`:

   | Tipo | Nome | Valore |
   |---|---|---|
   | CNAME | `www` | `tech-scrippellapp.github.io.` |
   | A | `@` | `185.199.108.153` |
   | A | `@` | `185.199.109.153` |
   | A | `@` | `185.199.110.153` |
   | A | `@` | `185.199.111.153` |

   Gli indirizzi sono quelli di GitHub Pages per i domini senza `www`. Prima di inserirli conviene ricontrollarli nella [guida di GitHub](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain).
4. Quando **Settings → Pages** mostra il controllo DNS riuscito, attivare **Enforce HTTPS**. Il certificato può richiedere fino a un giorno.

Con il file `CNAME` presente, l'indirizzo `tech-scrippellapp.github.io/scrippellapp-landing` porta al dominio. Finché il DNS non è configurato, il sito quindi non si apre da nessuna parte. Per provarlo prima, si usa lo sviluppo locale.

### Controlli dopo la pubblicazione

```sh
dig www.scrippellapp.it +short   # tech-scrippellapp.github.io. e quattro indirizzi 185.199.x.x
dig scrippellapp.it +short       # i quattro indirizzi 185.199.108-111.153
```

| Indirizzo | Atteso |
|---|---|
| `https://www.scrippellapp.it/` | la pagina principale |
| `http://www.scrippellapp.it/` | rimanda a https |
| `https://scrippellapp.it/` | rimanda a www |
| `https://www.scrippellapp.it/og-image-1200x630.png` | l'immagine delle anteprime |
| `https://www.scrippellapp.it/robots.txt` e `/sitemap.xml` | i due file |
| `https://www.scrippellapp.it/.well-known/apple-app-site-association` e `/.well-known/assetlinks.json` | 200, senza redirect (`curl -sI`) |
| `https://www.scrippellapp.it/invito/?c=K7M4QX` | la pagina di invito con il codice `K7M4QX` |

Le impronte SHA-256 in `assetlinks.json` (keystore di debug, di caricamento e firma di Google Play) non sono segrete. C'è già quella della keystore di debug dell'app; le altre due le dà Matteo nella Fase 4 (prove P2 e P3).

L'anteprima su WhatsApp si prova incollando il link in una chat. WhatsApp tiene in memoria le anteprime: dopo un cambio dell'immagine conviene provare con `?v=2` in fondo al link.

## Quando l'app è sugli store

- In `index.html` la riga "Gratis, presto su App Store e Google Play" lascia il posto ai badge ufficiali di Apple e Google, presi dalle loro pagine per i badge e collegati alle schede.
- App Store Connect e Google Play Console: sito `https://www.scrippellapp.it/`, informativa `https://www.scrippellapp.it/privacy.html`. Google Play chiede anche l'indirizzo di `elimina-account.html`.
- Google Search Console: proprietà `www.scrippellapp.it` e invio di `sitemap.xml`.
