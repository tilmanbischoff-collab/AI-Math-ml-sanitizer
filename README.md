# MathML-Inline-Interpreter

Der MathML-Inline-Interpreter ist eine einzelne HTML-Anwendung zum Bereinigen,
Aufbereiten und Prüfen von gemischtem HTML-, Text- und MathML-Inhalt. Sie läuft
direkt im Browser und benötigt keinen Webserver und keine Installation.

Die Anwendung befindet sich in:

```text
MathML-sanitizer.html
```

## Funktionen

- Eingabe von reinem Text, HTML und MathML
- strukturelle Bereinigung über HTML- und MathML-Allowlists
- Entfernung aktiver Inhalte, Ereignisattribute und nicht erlaubter Elemente
- Korrektur von ungültigem `<text>` innerhalb von MathML zu `<mtext>`
- Umwandlung von erklärendem MathML-Text in normalen HTML-Fließtext
- Beibehaltung mathematischer Segmente als Inline-MathML
- Erhaltung bewusst gesetzter HTML-Umbrüche mit `<br>`
- responsive Vorschau mit frei einstellbarer Breite
- abgedockte Vorschau in einem eigenen Browserfenster
- kompakte Zusammenfassung der vorgenommenen Bereinigungen
- Kopieren des gereinigten und lesbar formatierten Codes
- Export einer eigenständigen HTML-Testseite mit eingebettetem MathJax

## Lokaler Start

1. Den Ordner `C:\AI-Work\math-ml` im Datei-Explorer öffnen.
2. `MathML-sanitizer.html` doppelt anklicken.
3. Falls Windows nach einer Anwendung fragt, einen aktuellen Browser auswählen,
   beispielsweise Edge, Chrome oder Firefox.

Alternativ kann die Datei in ein geöffnetes Browserfenster gezogen oder über
`Strg+O` ausgewählt werden. Die Adresszeile zeigt beim lokalen Betrieb eine
Adresse nach diesem Muster:

```text
file:///C:/AI-Work/math-ml/MathML-sanitizer.html
```

## MathJax laden

Für die mathematische Darstellung benötigt die Anwendung das kombinierte
MathJax-Modul `mml-chtml.js`. Beim Start werden die folgenden Quellen in dieser
Reihenfolge geprüft:

1. lokale Datei `mathjax/mml-chtml.js` relativ zur HTML-Datei
2. fest versionierte MathJax-Datei vom jsDelivr-CDN

Für einen vollständig lokalen Vorschaubetrieb kann der Projektordner daher so
aufgebaut werden:

```text
math-ml/
├── MathML-sanitizer.html
├── README.md
└── mathjax/
    └── mml-chtml.js
```

Ist keine lokale MathJax-Datei vorhanden, benötigt der Browser beim Start eine
Internetverbindung. Einschränkungen des Browsers für lokale `file://`-Seiten
können das Laden lokaler Skripte beeinflussen; in diesem Fall wird automatisch
das CDN versucht.

## Bedienung

### 1. Quellcode eingeben

Den zu prüfenden Inhalt in das Feld **HTML, Text und MathML** einsetzen. Zulässig
sind beispielsweise:

```html
<p>
  Der Funktionswert ist
  <math xmlns="http://www.w3.org/1998/Math/MathML" display="inline">
    <mi>f</mi><mo>(</mo><mi>x</mi><mo>)</mo>
  </math>.<br>
  Danach beginnt eine neue Zeile.
</p>
```

Reiner Text wird automatisch in Absätze umgewandelt. Einzelne Zeilenumbrüche
werden dabei als `<br>` übernommen; Leerzeilen trennen Absätze.

### 2. Bereinigen und anzeigen

Mit **Vorschau aktualisieren** wird der Inhalt:

1. geparst,
2. anhand der Allowlists bereinigt,
3. als lesbarer Code formatiert,
4. in der Vorschau durch MathJax gesetzt.

Unter **Sanitized Code** erscheinen anschließend zwei Bereiche:

- **Gereinigter Code** enthält die kanonische HTML-/MathML-Fassung.
- **Änderungen am Ursprungscode** fasst entfernte, reparierte oder ergänzte
  Elemente und Attribute zusammen.

Die Whitespace-Formatierung des ausgegebenen Codes wird vereinheitlicht.
Bewusst gewünschte feste Zeilenumbrüche müssen als `<br>` im Eingabecode stehen.

### 3. Darstellungsbreite prüfen

Mit dem Schieberegler oberhalb der Vorschau lässt sich die Breite des simulierten
Ausgabebereichs verändern. Dadurch können Inline-Formeln, MathJax-Zeilenumbrüche
und HTML-Fließtext ohne Änderung der Größe des Browserfensters geprüft werden.

**Vorschau abdocken** öffnet denselben Inhalt in einem separaten Fenster. Wird
kein Fenster geöffnet, muss der Browser Pop-ups für die lokale Datei erlauben.

### 4. Gereinigten Code kopieren

**Sanitized Code kopieren** bereinigt stets den aktuellen Inhalt des Eingabefelds
neu und kopiert das Ergebnis in die Zwischenablage. Falls der Browser den direkten
Zugriff auf die Zwischenablage bei einer lokalen Datei nicht erlaubt, verwendet
die Anwendung eine Auswahl-basierte Ersatzmethode.

### 5. Testfall zurücksetzen

**Testfall zurücksetzen** stellt das mitgelieferte Beispiel wieder her und
aktualisiert die Vorschau.

## Eigenständige Testseite exportieren

Mit **Eigenständige HTML-Testseite herunterladen** wird eine einzelne Datei mit
dem Namen `mathml-mobile-test-standalone.html` erzeugt. Sie enthält:

- den aktuell bereinigten Inhalt,
- die für die Darstellung benötigten Styles,
- die vollständige MathJax-Laufzeit.

Für den Export versucht die Anwendung zunächst, MathJax vom CDN zu laden. Soll
der Export ohne Internetverbindung erfolgen, kann zuvor über das Dateifeld ein
lokales `mml-chtml.js` ausgewählt werden. Diese Datei wird nur im Browser gelesen
und in die erzeugte Testseite eingebettet.

Die exportierte Datei kann anschließend ebenfalls per Doppelklick lokal geöffnet
und beispielsweise auf ein Mobilgerät übertragen werden.

> **Sicherheitshinweis:** Nur eine vertrauenswürdige lokale MathJax-Datei
> auswählen. Ihr JavaScript wird unverändert in den Export übernommen und beim
> Öffnen der exportierten HTML-Datei ausgeführt.

## Bereinigungsregeln

Die Anwendung verwendet begrenzte Allowlists für HTML-Elemente,
MathML-Elemente und Attribute. Unter anderem werden:

- `script`, `style`, `iframe`, `object`, `embed`, `link` und `meta` entfernt,
- Ereignisattribute wie `onclick` entfernt,
- nicht erlaubte Attribute verworfen,
- gefährliche Medien- und Vektorelemente vollständig entfernt,
- unbekannte, nicht aktive Elemente entfernt, deren Textinhalt jedoch erhalten,
- fehlende MathML-Angaben wie Namespace, `display` und `overflow` ergänzt,
- `encoding` für MathML-Annotationen beibehalten.

Gemischtes MathML mit erklärendem `<mtext>` auf der obersten Flussebene wird in
normalen HTML-Text und eigenständige Inline-Formelsegmente aufgeteilt. `<mtext>`
innerhalb eines Bruchs, Indexes oder einer Tabelle bleibt erhalten, damit die
mathematische Struktur nicht beschädigt wird.

## Grenzen und Hinweise

- Die Bereinigung im Browser ist eine Referenz- und Testfunktion. Bei einer
  späteren CMS-Integration sollten nicht vertrauenswürdige Inhalte zusätzlich
  serverseitig sanitisiert werden.
- Die Vorschau hängt von MathJax und den Sicherheitsregeln des verwendeten
  Browsers für lokale Dateien ab.
- Das abgedockte Fenster kann durch einen Pop-up-Blocker verhindert werden.
- Der Sanitizer erlaubt bewusst nur einen Teil von HTML und MathML. Nicht in der
  Allowlist enthaltene Konstrukte können entfernt oder vereinfacht werden.
- Die Anwendung speichert Eingaben nicht dauerhaft. Beim Neuladen der Seite
  geht der aktuelle Inhalt verloren.

## Fehlerbehebung

### MathJax konnte nicht geladen werden

- Internetverbindung prüfen, wenn das CDN verwendet werden soll.
- Alternativ `mml-chtml.js` unter `mathjax/mml-chtml.js` ablegen.
- Prüfen, ob Browser-Erweiterungen oder Unternehmensrichtlinien jsDelivr sperren.

### Die abgedockte Vorschau erscheint nicht

- Pop-ups für die lokale HTML-Datei zulassen.
- Danach erneut auf **Vorschau abdocken** klicken.

### Der Export funktioniert offline nicht

- Im Exportbereich eine lokale, vertrauenswürdige Datei `mml-chtml.js` auswählen.
- Anschließend den Download erneut starten.

### Ein gewünschter Zeilenumbruch fehlt

- An der betreffenden Stelle im Eingabecode ausdrücklich `<br>` einfügen.
- Danach **Vorschau aktualisieren** wählen und die Darstellung erneut prüfen.
