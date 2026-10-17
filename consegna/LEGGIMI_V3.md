# Petelia V3 — solo grafica della homepage

Questa versione riparte esclusivamente dal nuovo archivio fornito dall’utente:
`theme_export_peteliashop_com_copia_di_copia_di_petelia_tema_v8_7 (1).zip`.
Non è un aggiornamento integrale della V2. È una nuova copia di prova del tema appena ricevuto, con due file modificati e una sezione aggiunta.

## Vincolo rispettato: carrello e checkout invariati

Tutti i 344 file originali diversi dal banner principale e dal template della homepage sono identici byte per byte allo ZIP ricevuto. Nessun file originale è stato eliminato. I file nella cartella `assets` sono tutti identici, compresi `global.js`, `product-form.js`, `petelia-cart-guard.js`, `cart-drawer.js` e gli script relativi a note e personalizzazioni. Sono identici anche layout, configurazioni, pagine prodotto, snippet, mini-carrello e pagina carrello.

Non è stata trasferita la modifica al recupero dell’HTML del mini-carrello introdotta nella V2. Il nuovo script del banner non contiene richieste cart/checkout, accessi a localStorage/sessionStorage, riferimenti ai componenti Petelia del carrello o gestori `pageshow`.

Questa equivalenza preserva il codice commerciale del tema caricato dall’utente. Non dimostra che eventuali problemi già presenti in quel tema siano risolti. Il problema segnalato tornando dal checkout non è stato riprodotto su Shopify e la sua causa non è stata accertata. Nessuna correzione del carrello viene dichiarata o applicata in questa consegna.

## Diagnosi e ordine degli interventi

1. **Video del primo banner mobile:** nel tema ricevuto il player viene attivato automaticamente senza un gesto dell’utente. Può competere con l’immagine iniziale e con le altre richieste. È stato sostituito questo comportamento con la galleria manuale concordata.
2. **Font del primo banner:** il foglio Google Fonts era bloccante. È ora richiesto senza bloccare il rendering iniziale, mantenendo famiglia Playfair Display, pesi e `display=swap`. Durante il caricamento può comparire brevemente il font di riserva. Con JavaScript disattivato resta il caricamento tradizionale tramite `noscript`.
3. **Banner Invektor:** assente dal nuovo ZIP. È stata aggiunta esattamente la stessa sezione della precedente variante, con le stesse impostazioni e posizione sotto il banner principale/barra promozionale. L’immagine ha varianti responsive e caricamento lazy già presenti nella sezione copiata.

Le conclusioni sono basate sul codice e sui test locali dei componenti. Non sono misure di PageSpeed o dell’esperienza reale degli utenti.

## Modifiche applicate

| File | Intervento |
| --- | --- |
| `sections/countdown-timer.liquid` | Galleria mobile immagine/video con swipe, due indicatori, frecce semitrasparenti, video attivato solo selezionando la seconda slide; caricamento non bloccante del font. |
| `sections/petelia-automatico-strip.liquid` | Nuovo file, identico byte per byte alla sezione Invektor della variante precedente. Nessuno script JavaScript. |
| `templates/index.json` | Solo inserimento della sezione Invektor e della sua voce nell’ordine delle sezioni. Tutte le impostazioni e i contenuti già presenti sono invariati. |

### Banner principale

- L’immagine iniziale rimane responsive con `loading="eager"` e `fetchpriority="high"`.
- Il video mobile resta dentro un template inerte: nessuna richiesta MP4 del banner prima della selezione della seconda slide.
- La navigazione avviene tramite swipe orizzontale, frecce, indicatori o tastiera. Lo scorrimento verticale della pagina resta disponibile.
- Se Shopify offre un MP4 con altezza non superiore a 720 px e larghezza non superiore a 1280 px, il banner mobile sceglie la variante con area maggiore entro questi limiti. In assenza di una variante idonea resta la sorgente generata da Shopify: non viene inventato né ricodificato un file video locale.
- Tornando all’immagine, uscendo dall’area visibile o cambiando scheda, il video viene messo in pausa. Riaprendo la seconda slide, il player viene riutilizzato.
- Se la riproduzione automatica viene rifiutata dal browser compare il pulsante «Riproduci il video». In caso di errore del media viene mostrata l’immagine alternativa, se configurata, oppure la prima immagine.
- Nessuna rotazione automatica della galleria mobile e nessuna pulsazione continua delle frecce. Sono rispettate le preferenze di movimento ridotto.
- Altezza del banner e destinazioni «PER LEI» / «PER LUI» mantenute. I controlli occupano la parte bassa del banner; l’area dei pulsanti è rialzata di 22 px per evitare sovrapposizioni.
- La configurazione desktop attuale mantiene l’immagine statica senza controlli mobili. L’eventuale attivazione esplicita di video desktop nell’editor conserva il comportamento desktop già previsto.
- Video scelto dall’utente preservato: `VID-20261010_151241.mp4`.
- Scadenza countdown preservata: `2026-10-18T23:59:59`, esattamente come nel nuovo ZIP; nessuna modifica all’interpretazione del fuso orario del countdown.

### Banner Invektor

Sono mantenuti immagine di riserva, colori, testo «Invektor. Il nostro modello automatico.», pulsante «Il primo automatico», altezza mobile di 64 px e desktop di 144 px, e spazio esterno di 0 px.

La destinazione del banner è **https://peteliashop.com**, esattamente come nel file della variante precedente. Il file precedente contiene questo URL nel codice e non usa il campo link dell’editor: anche questo comportamento è stato lasciato identico. Una destinazione prodotto diversa richiederebbe una modifica separata.

## Altre opportunità grafiche individuate — non applicate

| Priorità | Elemento | Evidenza nel nuovo tema | Intervento da valutare |
| --- | --- | --- | --- |
| Alta | Video in fondo alla homepage, sezione `video-hero` | Il player MP4 è già nell’HTML con `autoplay: true`. Il rinvio della chiamata JS a `play()` non garantisce che il browser rinvii il download. | Rinviare il montaggio del player fino alla vicinanza della sezione, con poster statico e pausa fuori schermo. Intervento da concordare; `video-hero.liquid` e `global.js` sono rimasti identici. |
| Media | Tre immagini delle categorie uomo/donna/gioielli, sezione `rich-text` | URL CDN originali senza `srcset`, dimensioni HTML o loading esplicito. | Varianti responsive per le piccole miniature, dimensioni dichiarate e lazy loading quando opportuno. Nessuna modifica applicata. |
| Media | `pacco_regalo.gif`, sezione `image-with-text-overlay` | GIF richiesta a dimensione originale e visualizzata a circa 2 em, senza loading esplicito. | Alleggerire l’animazione conservandone l’aspetto; valutare caricamento differito. L’originale remoto non è incluso nello ZIP. Nessuna modifica applicata. |
| Bassa | Icona WhatsApp nel Liquid della homepage | Un intero foglio Font Awesome remoto viene richiesto per una sola icona. | Sostituire soltanto l’icona con lo stesso SVG. Non applicato in questa versione: anche quel Liquid è stato preservato. |

Non sono stati misurati pesi attuali dei media remoti né risparmi effettivi. Per un’eventuale ricodifica video, mantenere MP4 H.264 con compatibilità mobile, senza traccia audio se inutile, e verificare una variante entro 720 px di altezza / 1280 px di larghezza. Per immagini statiche usare varianti WebP/AVIF servite dal CDN, conservando trasparenze e nitidezza; non convertire una GIF animata in un’immagine statica senza accordo.

## Verifiche eseguite

- Archivio ricevuto letto e verificato; tutti i file della baseline locale confrontati con i byte dello ZIP caricato.
- Confronto SHA-256 per ogni file prima/dopo, incluso nel manifest `validation.json`: 346 file iniziali, 347 finali; 344 originali identici, 2 modificati, 1 aggiunto, 0 eliminati.
- Tutti i file assets, gli altri template, le altre sezioni, gli snippet, i layout e la configurazione sono identici.
- Configurazione homepage confrontata semanticamente: rimossa la sola nuova voce Invektor, il JSON coincide con l’originale ricevuto.
- Sintassi verificata per i 44 JavaScript esistenti e per lo script inline del banner.
- Parsing dei 57 file JSON e di 74 schema Liquid con JSON standard; lo schema già esistente di `sections/header.liquid` contiene una virgola finale accettata dal parser permissivo. Quel file è identico al tema ricevuto e non è stato corretto.
- Parsing Liquid rigoroso superato per entrambe le sezioni aggiunta/modificata.
- Theme Check: 83 segnalazioni nella baseline e 84 nella copia finale; **nessun nuovo errore**. L’incremento è la seconda segnalazione RemoteAsset dello stesso font remoto nel fallback `noscript`; le altre segnalazioni già presenti restano invariate.
- **18 controlli locali Chromium superati**, inclusi nessuna richiesta del video del banner per un intero precedente ciclo di 12,5 s, apertura a richiesta, variante MP4 simulata 720p, swipe touch, scorrimento verticale, tastiera, pause, riuso player, riproduzione bloccata, media non disponibile, assenza di media secondario, desktop, ridimensionamento, scaricamento sezione nell’editor, movimento ridotto, assenza di JavaScript, dimensioni/CTA e Invektor tra 320 e 1440 px.

Questi sono test di componenti con dati e media sintetici. Non sono una riproduzione completa del negozio né un test di checkout reale. Nessuna misura PageSpeed, visita in browser interno Meta, prova di PayPal o pubblicazione è stata eseguita. Non viene promesso un determinato punteggio.

## Prima della pubblicazione

1. Importare `PETELIA_V3_SOLO_HOMEPAGE.zip` come **nuovo tema non pubblicato**, distinto dalla precedente V2.
2. Controllare sul telefono la prima immagine, il caricamento iniziale senza video del primo banner, swipe/frecce/indicatori, avvio del video solo sulla seconda slide, ritorno alla prima, pausa quando si scorre oltre il banner.
3. Verificare su desktop immagine, altezza, countdown, pulsanti, sezione Invektor e contenuti sottostanti; controllare il banner anche con movimento ridotto e risparmio energetico su iPhone/Android.
4. Aprire una landing dal browser interno Instagram/Facebook e verificare immagine e avvio manuale del video se la riproduzione viene bloccata.
5. Verificare il caso commerciale segnalato anche sulla copia di prova: prodotto con extra e testi → checkout → indietro → prodotto, extra, testi e totale presenti → riapertura checkout. Se il problema si presenta anche su questa copia, fermare la pubblicazione e conservarne evidenza: il codice commerciale qui è esattamente quello del nuovo ZIP.
6. Per confrontare PageSpeed, usare configurazioni e media fermi, stesso tipo di URL e almeno tre prove alternate per tema, confrontando mediane di FCP/LCP/TBT/CLS oltre al punteggio. Le anteprime Shopify includono risorse aggiuntive.

Il tema pubblicato non è stato modificato. I documenti, i test e il diff sono consegnati separatamente e non vengono inseriti nel tema Shopify.
