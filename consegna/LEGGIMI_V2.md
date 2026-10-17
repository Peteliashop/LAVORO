# Petelia V8.7 — Galleria mobile V2, copia di prova

Preparata il 17 ottobre 2026 sulla nuova esportazione fornita dall'utente: `theme_export_peteliashop_com_petelia_v8_7_performance_test_2026.zip`.

**Questa consegna non modifica né pubblica il negozio.** Caricare lo ZIP del tema come nuova copia non pubblicata e verificare i flussi prima della pubblicazione. Non è stato eseguito un nuovo PageSpeed della V2 né un checkout reale: le prove sono locali, con oggetti Shopify e media sintetici.

## Base aggiornata preservata

Rispetto alla prima consegna, nello ZIP ricevuto cambiano soltanto:

- il video del banner, ora `shopify://files/videos/VID-20261010_151241.mp4`;
- la destinazione codificata della strip Invektor, corretta dall'utente in `https://peteliashop.com`.

Entrambe le modifiche sono conservate. Restano le ottimizzazioni precedenti di immagini responsive, galleria prodotto, collezioni e video istituzionale inferiore. La V2 cambia soltanto **3 file su 347**, senza aggiungere o rimuovere file del tema.

## Modifiche

### 1. Banner mobile: `sections/countdown-timer.liquid`

- La prima immagine rimane eager e con fetchpriority high, senza la precedente alternanza automatica su mobile.
- Due contenuti navigabili con swipe orizzontale, pallini selezionabili, frecce semitrasparenti e tastiera. La navigazione si ferma agli estremi; la freccia non utilizzabile resta attenuata. Nessuna pulsazione continua.
- Fino all'apertura della seconda slide, il video rimane nel template inerte: nessuna creazione del player e nessuna richiesta MP4 del banner nei test locali, anche oltre il precedente ciclo di 12 secondi.
- Aprendo la seconda slide, il video mantiene muted, loop e playsinline. Torna in pausa quando si rientra nella prima slide, si lascia il banner o si nasconde la pagina. Riaprendo la slide si riusa il player.
- Quando Shopify fornisce una sorgente MP4 con altezza <=720 e larghezza <=1280, viene scelta la più grande che rispetta questi limiti per il player mobile. Se non esiste, resta la selezione originale prodotta da video_tag. Non viene inventata una transcodifica o modificato il file da 12 MB. La disponibilità e qualità delle varianti reali vanno verificate nell'anteprima.
- Se il browser rifiuta play(), compare il pulsante “Riproduci il video”. Se il media genera un errore, si usa la seconda immagine eventualmente configurata, altrimenti si ritorna alla prima immagine. Gli indicatori e gli annunci accessibili seguono lo stato effettivo.
- Dimensioni esterne del banner e rapporto della prima immagine conservati nei viewport provati. Su mobile gli elementi sovrapposti vengono alzati di 22 px per fare spazio ai pallini, senza sovrapporli ai pulsanti PER LEI/PER LUI.
- Titoli, promozione, scadenza, CTA, destinazioni, sorgenti delle immagini e selezione del nuovo video preservati. Countdown invariato nella logica; le impostazioni attuali mantengono la scadenza 10/10/2026 e il suo messaggio alternativo preesistente.
- La configurazione desktop attuale resta con immagine statica e senza controlli mobile. Se il commerciante seleziona esplicitamente il video per desktop/all, si conserva l'attivazione desktop dopo disponibilità dell'immagine e l'animazione configurata.
- Gestione reload/unload dell'editor e preferenza reduced-motion verificata. Senza JavaScript rimane disponibile la prima immagine; i controlli inattivi sono nascosti.

### 2. Icona WhatsApp: `templates/index.json`

Modificato solo il valore custom Liquid del blocco WhatsApp. Il foglio Font Awesome esterno e il relativo uso del font sono sostituiti dal glifo SVG equivalente ufficiale Font Awesome Free 5.15.3, con attribuzione CC BY 4.0. Il tema usa Font Awesome soltanto per questa icona nei file ispezionati.

Numero, testo, destinazione WhatsApp, colori, fondo bianco e proporzioni dell'icona restano gli stessi. Tutte le altre impostazioni e l'ordine delle sezioni sono uguali alla nuova esportazione. Non sono stati rimossi chat, recensioni, pixel, app, consenso o librerie commerciali.

### 3. Recupero mini-carrello: `assets/petelia-cart-guard.js`

I PDF precedenti mostrano l'errore `Petelia return refresh: Error: Markup del carrello non disponibile.` anche nel tema originale. La causa reale sul server Shopify non è stata riprodotta da questa sessione, perché l'accesso resta bloccato dalla policy di rete dell'ambiente.

La modifica è un recupero limitato e di sola lettura:

1. Si legge la risposta delle sezioni con il flusso già esistente.
2. Se manca un mini-cart valido, si richiede la singola sezione usando l'endpoint già dichiarato dal drawer del tema, conservando l'eventuale preview_theme_id e il percorso relativo corretto.
3. Si accetta HTML contenente il form mini-cart, con wrapper Shopify oppure come frammento. Si rifiutano pagine HTML intere, sezioni vuote e risposte 404.
4. Il drawer viene sostituito solo dopo la validazione. Se anche il recupero fallisce, il vecchio contenuto rimane e l'errore continua a essere segnalato: non viene cancellato o nascosto.

Questa modifica copre risposte parziali o prive del wrapper atteso; non garantisce di correggere qualunque errore dell'API Shopify. Non aggiunge scritture al carrello. **La logica di mutazione, servizi, quantità, rimozione gruppi e checkout, da mutationQueue alla fine del file, è identica byte per byte alla base.** Anche mappa dei servizi e riconoscimento delle proprietà rimangono identici.

Non sono stati cambiati prezzi, ID varianti, testi incisioni/NFC, regole di consenso, eventi Meta o modalità di pagamento. I file product-form, mini-cart, cart-drawer e gli altri template commerciali restano invariati.

## Verifiche completate

| Verifica | Esito |
|---|---|
| Base confrontata byte per byte con lo ZIP aggiornato | Passata, 347 file |
| File aggiunti/rimossi | Nessuno |
| File modificati | 3 |
| Preservazione nuovo video, link Invektor e altre impostazioni homepage | Passata |
| JSON standalone | 57/57 letti correttamente |
| JSON schema delle sezioni | 75/75 letti correttamente |
| Parser Liquid/HTML strict del file Liquid modificato | Passato |
| Sintassi JavaScript degli asset | 44/44 passati |
| Sintassi JavaScript inline del banner | Passata |
| Theme Check 3.30.1 | 79 segnalazioni prima, 79 dopo; nessun nuovo errore |
| Prove browser locali Chromium | **31/31 passate**, nessuna saltata |
| Combinazioni extra nel controllo commerciale | **64 casi** all'interno della suite: 32 combinazioni con testi/pending e quantità 2 |
| Checkout simulato locale | Navigazione dopo riparazione riuscita; navigazione impedita con scritture rifiutate e pulsante riabilitato |
| Prova di checkout/pagamento/Meta reale | Non eseguita |
| Nuovo PageSpeed/Lighthouse su storefront V2 | Non eseguito |
| Pubblicazione o caricamento su Shopify | Non eseguiti |

Sono stati provati touch swipe reale emulato, scorrimento verticale, frecce/pallini/tastiera, errore video, rifiuto autoplay, pausa/ripresa fuori schermo, breakpoint desktop/mobile, pulizia editor, senza JS, reduced-motion e geometria a 320/390/430/768 px. I controlli preservati su PDP, card e video inferiore sono stati rieseguiti. Per il carrello sono stati provati risposta standard, frammento, sezione mancante con fallback, fallback 404/pagina intera, chiamate contemporanee e rimozione di un gruppo conservando un altro orologio personalizzato.

I test usano codice reale del tema ma server e dati sintetici. Non sostituiscono Safari/iPhone, browser interni Meta, Shopify, il catalogo e le integrazioni reali. Le immagini e il video sintetici usati dai test non sono inclusi nello ZIP del tema.

## Punti rimasti aperti

- **GIF pacco regalo non ricompresso:** lo ZIP contiene soltanto il riferimento CDN, non il file animato. La rete non permette di recuperarlo; non sono stati ricostruiti fotogrammi dai PDF o cambiata l'animazione con una statica. Resta il file già richiesto in lazy. Per un successivo alleggerimento serve il GIF originale, oppure un accesso consentito al CDN; peso indicato nei PDF precedenti circa 507 KiB.
- Il video mobile viene caricato su richiesta e usa la variante limitata quando disponibile; il nuovo file dell'utente è preservato. Il peso reale della sorgente scelta da Shopify e l'eventuale qualità inferiore della variante vanno misurati nella nuova anteprima. Nessun punteggio 83/90/100 o aumento delle vendite viene garantito.
- Restano i diagnostici preesistenti di Theme Check: parser in login/indirizzi/js-variables, schemi legacy/templates e traduzioni, price-money.js del gift card mancante, filtro limit non documentato e altri avvisi. Non sono stati disabilitati controlli per ottenere un risultato verde.
- Gli errori web-pixel, Shop Pay e telemetria nei PDF richiedono verifica nel contesto Shopify. Non sono stati aggiunti pixel né disattivate integrazioni. Questa copia non certifica Meta/CAPI o PayPal.
- Il confronto PageSpeed va ripetuto dopo importazione, mantenendo fermi video e impostazioni e confrontando più run omogenei, inclusi i risultati peggiori. L'anteprima aggiunge risorse proprie.

## Controlli prima di pubblicare

1. Importare **PETELIA_V8_7_GALLERIA_MOBILE_V2_2026-10-17.zip** come nuovo tema non pubblicato. Conservare la copia precedente come rollback.
2. Confermare selezione di VID-20261010_151241.mp4 e immagini attuali. Su mobile attendere senza toccare: la foto deve restare ferma, le CTA utilizzabili e il video del banner non deve partire in Network. Aprire la seconda slide con swipe, freccia e pallino; tornare indietro e verificare la pausa.
3. Provare iPhone e Android, modalità risparmio energia, rotazione, ritorno alla scheda e apertura in Instagram/Facebook. Verificare qualità del video 720p, poster e pulsante Riproduci quando necessario. Controllare che i pallini non coprano le CTA e che lo scroll verticale resti naturale.
4. Confrontare desktop, strip Invektor, collezioni, WhatsApp, recensioni e video in fondo. La variazione della galleria riguarda il mobile del banner superiore.
5. Provare Mare Nostrum e Silva Viridis: foto iniziale, varianti, miniature, zoom, Personalizza, confezione, shopper, incisioni singole/combo, NFC, testi compilati/pending e quantità 2. Provare due orologi uguali con scelte differenti.
6. Verificare drawer aggiornato, quantità, rimozione, note, riepilogo extra e totali. Aprire il checkout e tornare indietro, controllando l'eventuale errore di refresh. Collaudare PayPal/pagamenti con le modalità di prova autorizzate dal proprietario, senza eseguire ordini involontari.
7. Controllare consenso accettato/rifiutato e gli eventi negli strumenti Shopify/Meta, senza introdurre duplicazioni. Effettuare PageSpeed homepage e PDP sul tema corretto, conservando almeno tre esecuzioni comparabili.

Il report tecnico include diff, manifest SHA-256, Theme Check base/V2, esiti browser, inventario aggiornato e strumenti di verifica. Il file di documentazione non è un tema da caricare su Shopify.
