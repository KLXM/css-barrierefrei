# Leitfaden zur Verbesserung der Webzugänglichkeit mit CSS

Translated and extended from: [How to Use CSS to Improve Web Accessibility (freeCodeCamp)](https://www.freecodecamp.org/news/how-to-use-css-to-improve-web-accessibility/)

CSS spielt eine entscheidende Rolle bei der Verbesserung der Webzugänglichkeit. Dieser Leitfaden bietet einen umfassenden Überblick über verschiedene CSS-Techniken und Best Practices zur Steigerung der Barrierefreiheit.

## Inhaltsverzeichnis

1.  [Voraussetzungen](#voraussetzungen)
2.  [Semantisches HTML mit CSS unterstützen](#semantisches-html-mit-css-unterstützen)
3.  [Fokus-Stile aktualisieren](#fokus-stile-aktualisieren)
4.  [Inhaltsverschiebungen vermeiden](#inhaltsverschiebungen-vermeiden)
5.  [Bewegung reduzieren](#bewegung-reduzieren)
6.  [Fokus innerhalb für verschachtelte Elemente](#fokus-innerhalb-für-verschachtelte-elemente)
7.  [Kontrastoptionen anpassen](#kontrastoptionen-anpassen)
8.  [Dunkelmodus aktivieren](#dunkelmodus-aktivieren)
9.  [Typografie und Zugänglichkeit](#typografie-und-zugänglichkeit)
    *   [Flexible Typografie mit `clamp()` und `min()`, `max()`](#flexible-typografie-mit-clamp-und-min-max)
    *   [Line-Height und Lesbarkeit](#line-height-und-lesbarkeit)
    *   [Letter-spacing und Word-spacing](#letter-spacing-und-word-spacing)
    *    [Textumbruch mit `word-break`, `overflow-wrap` und `hyphens`](#textumbruch-mit-word-break-overflow-wrap-und-hyphens)
    *   [Schriftgröße und Hierarchie](#schriftgröße-und-hierarchie)
    *   [Barrierefreie Schriftarten](#barrierefreie-schriftarten)
    *   [Font-Loading Strategien](#font-loading-strategien)
    *   [Variable Fonts](#variable-fonts)
10. [Animationen zur Verbesserung der UX](#animationen-zur-verbesserung-der-ux)
11. [Inhalte je nach Lesegerät ein- und ausblenden](#inhalte-je-nach-lesegerät-ein--und-ausblenden)
12.  [`aria-*`-Attribute und CSS](#aria-attribute-und-css)
13. [Farben und Kontrast](#farben-und-kontrast)
14. [Flexibles Layout und Responsive Design](#flexibles-layout-und-responsive-design)
15. [Formular-Styling für bessere Zugänglichkeit](#formular-styling-für-bessere-zugänglichkeit)
16. [Barrierefreie Tabellen](#barrierefreie-tabellen)
17. [Unterstützung für Bildschirmlupen](#unterstützung-für-bildschirmlupen)
18. [Einsatz von `clip-path` und `shape-outside` mit Bedacht](#einsatz-von-clip-path-und-shape-outside-mit-bedacht)
19. [Layout-Flexibilität mit `grid` und `flexbox` vertiefen](#layout-flexibilität-mit-grid-und-flexbox-vertiefen)
20. [Einsatz von `z-index` mit Vorsicht](#einsatz-von-z-index-mit-vorsicht)
21. [CSS-basierte Tooltips und Overlays](#css-basierte-tooltips-und-overlays)
22. [Zugänglichkeit bei der Implementierung von SVG](#zugänglichkeit-bei-der-implementierung-von-svg)
23. [Einheitliche Stile](#einheitliche-stile)
24. [Ladezeit & Performance](#ladezeit--performance)
25. [Testen der Zugänglichkeit](#testen-der-zugänglichkeit)
26.  [Priorisierung](#priorisierung)
27.  [Updates](#updates)
28. [Ausnahme- und Sonderfälle](#ausnahme--und-sonderfälle)
29. [Fazit](#fazit)
30. [Ressourcen](#ressourcen)

## Voraussetzungen

Um diesem Tutorial zu folgen, sollten Sie über grundlegende Kenntnisse in HTML, CSS und etwas JavaScript verfügen.

## Semantisches HTML mit CSS unterstützen

Semantisches HTML ist die Grundlage für Barrierefreiheit. CSS unterstützt diese Strukturen, indem es das visuelle Erscheinungsbild von Elementen wie `<nav>`, `<article>`, `<aside>`, `<section>` anpasst und hervorhebt. CSS sollte semantische Elemente nicht ersetzen, sondern sie visuell verstärken, um die Bedeutung des Inhalts zu kommunizieren.

## Fokus-Stile aktualisieren

Der Browser bietet standardmäßige Fokus-Stile für interaktive Elemente. Diese können jedoch manchmal nicht ideal für Ihr Designsystem sein oder schwer zu erkennen sein.

```css
input:focus, button:focus {
  outline: 2px solid #007BFF;
  outline-offset: 2px;
  box-shadow: 0 0 0 4px rgba(0, 123, 255, 0.25);
}
```

## Inhaltsverschiebungen vermeiden

Inhaltsverschiebungen können für Benutzer frustrierend sein, insbesondere für solche mit kognitiven Einschränkungen oder die Bildschirmvergrößerungen verwenden.

```css
.content-container {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}

img {
    max-width: 100%;
    height: auto;
    aspect-ratio: 16/9;
    object-fit: cover;
}
```

Für dynamisch geladene Inhalte:

```css
.new-content {
    transition: margin 0.3s ease-in-out, opacity 0.3s ease-in-out;
}
```

## Bewegung reduzieren

Schnelle Animationen können für Benutzer mit Bewegungsempfindlichkeit störend sein. Verwenden Sie `prefers-reduced-motion`, um Animationen anzupassen.

```css
@media (prefers-reduced-motion: no-preference) {
  .animated {
    transition: transform 0.3s ease-in-out;
  }
}

@media (prefers-reduced-motion: reduce) {
  .animated {
    transition: none;
  }
}
```

Hier ist ein Beispiel für die Reduzierung von Bewegung:

[CodePen: Reduced Motion Example](https://codepen.io/leezee/embed/preview/PorrrQW?default-tab=result&editable=true)

## Fokus innerhalb für verschachtelte Elemente

Verwenden Sie `:focus-within`, um übergeordnete Elemente hervorzuheben, wenn ein untergeordnetes Element fokussiert wird.

```css
form:focus-within {
   box-shadow: 0 0 0 2px #007BFF;
}
```

## Kontrastoptionen anpassen

Passen Sie den Kontrast basierend auf den Benutzereinstellungen an.

```css
@media (prefers-contrast: more) {
    body {
        background-color: #000;
        color: #fff;
    }
    a {
        color: #ff0;
    }
}

@media (prefers-contrast: less) {
    body {
        background-color: #f0f0f0;
        color: #333;
    }
}
```

Hier ist ein Beispiel für Kontrastanpassungen:

[CodePen: Contrast Preferences Example](https://codepen.io/leezee/embed/preview/dyBBxgV?default-tab=result&editable=true)

## Dunkelmodus aktivieren

Implementieren Sie einen Dunkelmodus mit CSS-Variablen und `prefers-color-scheme`.

```css
:root {
  --bg-color: #fff;
  --text-color: #000;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg-color: #000;
    --text-color: #fff;
  }
}

body {
  background-color: var(--bg-color);
  color: var(--text-color);
}
```

## Typografie und Zugänglichkeit

Zusätzlich zu den bereits erwähnten `rem` und Viewport-basierten Einheiten (vw, vh, etc.) gibt es weitere Aspekte der Typografie, die die Zugänglichkeit verbessern können:

### Flexible Typografie mit `clamp()` und `min()`, `max()`

Mit `clamp()` kann eine Schriftgröße definiert werden, die zwischen einem minimalen und einem maximalen Wert liegt, wobei die aktuelle Viewportgröße die tatsächliche Größe bestimmt. Mit `min()` und `max()` kann die kleinere bzw. die größere Schriftgröße angegeben werden. Diese Funktionen verbessern die Lesbarkeit, besonders auf verschiedenen Bildschirmgrößen.

```css
h1 {
  font-size: clamp(1.5rem, 4vw, 3rem); /* Schriftgröße zwischen 1.5rem und 3rem, skaliert mit 4vw */
}

p {
  font-size: min(max(1rem, 3vw), 2rem); /* Schriftgröße zwischen 1rem (mindestens 3vw) und 2rem. */
}
```

### Line-Height und Lesbarkeit

Eine ausreichende Zeilenhöhe (`line-height`) ist wichtig für eine gute Lesbarkeit. Es wird empfohlen, einen Wert von mindestens 1.5 für `line-height` zu verwenden, um genügend Raum zwischen den Textzeilen zu schaffen.

```css
p {
  font-size: 1rem;
  line-height: 1.6; /* Erhöhte Zeilenhöhe für bessere Lesbarkeit */
}
```

### Letter-spacing und Word-spacing

`letter-spacing` und `word-spacing` können einen Einfluss auf die Lesbarkeit haben und sollten sinnvoll eingesetzt werden. Bei großen Texten ist es oft sinnvoll, die `letter-spacing` leicht zu erhöhen.

```css
p {
  letter-spacing: 0.02em;
}
```

### Textumbruch mit `word-break`, `overflow-wrap` und `hyphens`

Lange Wörter oder URLs können mit `word-break: break-word;` umbrochen werden, um ein Überlaufen des Containers zu verhindern. `overflow-wrap: anywhere;` erzielt ein ähnliches Ergebnis. Die Silbentrennung mit `hyphens: auto;` strukturiert den Text besser.

```css
p {
  word-break: break-word; /* Umbruch von langen Wörtern */
  overflow-wrap: anywhere; /* Alternativer Umbruch für lange Wörter */
  hyphens: auto; /* Aktiviert automatische Silbentrennung */
}
```

### Schriftgröße und Hierarchie

Eine klare Hierarchie der Überschriften (`h1` bis `h6`) ist wichtig, um Inhalte übersichtlich zu strukturieren und die semantische Bedeutung der Elemente zu verdeutlichen.

### Barrierefreie Schriftarten

Einige barrierefreie Schriftarten, die sich durch ihre gute Lesbarkeit auszeichnen:

*   **Sans-Serif-Schriften:** Open Sans, Lato, Roboto, Montserrat, Helvetica, Arial. Diese Schriften sind in der Regel besser lesbar als Serifenschriften.
*   **Monospace-Schriften (für Code):** Fira Code, Source Code Pro. Diese Schriften eignen sich gut für Codebeispiele und sorgen für eine klare Darstellung der Zeichen.
    Gut lesbare Schriften haben oft klare, einfache Formen und ausreichend große Abstände zwischen Buchstaben. Es ist empfehlenswert, verschiedene Schriftarten mit den Benutzern zu testen, um die optimale Wahl für die jeweilige Zielgruppe zu finden.

### Font-Loading Strategien

Die Problematik von Font-Loading (FOIT, FOUT) kann durch das `font-display`-Attribut behoben werden. `font-display: swap;` lädt den Text mit einer Fallback-Font und tauscht es erst aus, wenn die gewünschte Schriftart verfügbar ist.

```css
@font-face {
    font-family: 'MeinFont';
    src: url('mein-font.woff2') format('woff2'),
        url('mein-font.woff') format('woff');
    font-display: swap;
}
p {
    font-family: 'MeinFont', sans-serif;
}
```

### Variable Fonts

Variable Fonts sind eine flexible Alternative zu traditionellen Font-Dateien, welche über den `font-variation-settings`-Eigenschaft an verschiedenste Anforderungen angepasst werden kann.

## Animationen zur Verbesserung der UX

CSS-Animationen können die Zugänglichkeit verbessern, wenn sie sinnvoll eingesetzt werden:

```css
.button {
  transition: transform 0.3s ease, background-color 0.3s ease;
}

.button:hover, .button:focus {
  transform: scale(1.05);
  background-color: #0056b3;
}
```

## Inhalte je nach Lesegerät ein- und ausblenden

### Visuell verstecken, aber für Screenreader zugänglich machen

```css
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

### Inhalte basierend auf Medienabfragen ein-/ausblenden

```css
@media screen and (max-width: 600px) {
  .desktop-only {
    display: none;
  }
}

@media print {
  .no-print {
    display: none;
  }
}
```

## `aria-*`-Attribute und CSS

`aria-*`-Attribute werden oft parallel zu CSS verwendet, um die Zugänglichkeit weiter zu verbessern (z.B. `aria-hidden`, `aria-label`, `aria-expanded`). Sie ermöglichen es, interaktive Elemente wie Buttons, die Menüs öffnen/schließen, besser zu gestalten und ihren Status an Screenreader zu kommunizieren.

**Beispiel:** Ein Button zum Öffnen/Schließen eines Menüs:

```html
<button aria-expanded="false" aria-label="Menü öffnen" class="menu-button">Menü</button>
```

```css
.menu-button {
  /* CSS für das Menü-Button-Design */
}
/* Wenn der Button geklickt wurde  */
.menu-button[aria-expanded="true"]{
  /* Angepasste Styles hier*/
}
```

## Farben und Kontrast

Stellen Sie sicher, dass Ihre Farbkombinationen ausreichend Kontrast bieten:

```css
.high-contrast {
  color: #000;
  background-color: #fff;
}

.link {
  color: #0000EE;
  text-decoration: underline;
}

.link:hover, .link:focus {
  text-decoration: none;
  outline: 2px solid currentColor;
}
```

## Flexibles Layout und Responsive Design

Verwenden Sie flexible Layouts für bessere Anpassung an verschiedene Bildschirmgrößen:

```css
.container {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.item {
  flex: 1 1 300px;
}

@media (max-width: 600px) {
  .item {
    flex-basis: 100%;
  }
}
```

## Formular-Styling für bessere Zugänglichkeit

Verbessern Sie die Zugänglichkeit von Formularen:

```css
label {
  display: block;
  margin-bottom: 0.5rem;
}

input, textarea, select {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

input:focus, textarea:focus, select:focus {
  outline: 2px solid #007BFF;
  outline-offset: 2px;
}

.error-message {
  color: #d00;
  font-size: 0.9rem;
  margin-top: 0.25rem;
}
```

## Barrierefreie Tabellen

Verbessern Sie die Lesbarkeit von Tabellen:

```css
table {
  border-collapse: collapse;
  width: 100%;
}

th, td {
  border: 1px solid #ddd;
  padding: 0.5rem;
  text-align: left;
}

th {
  background-color: #f2f2f2;
  font-weight: bold;
}

@media (max-width: 600px) {
  table, thead, tbody, th, td, tr {
    display: block;
  }

  thead tr {
    position: absolute;
    top: -9999px;
    left: -9999px;
  }

  tr {
    margin-bottom: 1rem;
  }

  td {
    border: none;
    position: relative;
    padding-left: 50%;
  }

  td:before {
    content: attr(data-label);
    position: absolute;
    left: 6px;
    width: 45%;
    padding-right: 10px;
    white-space: nowrap;
    font-weight: bold;
  }
}
```

## Unterstützung für Bildschirmlupen

Optimieren Sie Ihr Layout für Benutzer von Bildschirmlupen:

```css
.zoomable-content {
  max-width: 100%;
  overflow-x: auto;
}

.zoomable-image {
  max-width: none;
  cursor: zoom-in;
}
```

## Einsatz von `clip-path` und `shape-outside` mit Bedacht

`clip-path` und `shape-outside` können zwar visuelle Effekte erzeugen, sollten aber so eingesetzt werden, dass der Inhalt weiterhin zugänglich ist. Texte dürfen nicht abgeschnitten oder unsichtbar werden. Achten Sie darauf Alternativen für Textüberblendungen anzubieten.

## Layout-Flexibilität mit `grid` und `flexbox` vertiefen

Neben grundlegenden Beispielen sollten komplexere Layouts mit `grid` und `flexbox` gezeigt werden, um deren Beitrag zur Barrierefreiheit zu veranschaulichen. Diese Technologien helfen, das Layout für verschiedene Bildschirmgrößen anzupassen, z.B. responsive Navigationen.

## Einsatz von `z-index` mit Vorsicht

`z-index` kann dazu führen, dass wichtige Inhalte überlagert und damit unsichtbar werden. Es ist wichtig, es richtig einzusetzen und unnötige `z-index` Werte zu vermeiden, um die Wartbarkeit zu gewährleisten.

## CSS-basierte Tooltips und Overlays

Zugängliche Tooltips und Overlays können mit reinem CSS erstellt werden, ohne JavaScript zu benötigen. Es ist wichtig, die Tastaturzugänglichkeit für diese Elemente zu berücksichtigen.

## Zugänglichkeit bei der Implementierung von SVG

Wenn SVGs verwendet werden, sollten `aria-label` und `aria-labelledby` genutzt werden, um den SVGs eine semantische Bedeutung zu geben. Bei dekorativen SVGs sollte `role="img"` und `focusable="false"` gesetzt werden.

## Einheitliche Stile

Ein einheitliches Styling erhöht die Usability, da sich Nutzer auf vertraute Muster verlassen können. Es ist wichtig, nicht zu viele verschiedene Stile zu verwenden und einen Styleguide zu verwenden.

## Ladezeit & Performance

CSS-Code sollte optimiert werden, um eine schnelle Ladezeit zu gewährleisten. Dazu gehört das Minifizieren von CSS, das Zusammenfassen von CSS-Dateien usw.

## Testen der Zugänglichkeit

Es ist wichtig die Zugänglichkeit der Webseite mit verschiedenen Tools und Techniken zu testen, wie z.B. mit Screenreadern, Tastaturbedienung, Kontrastprüfung und Browsererweiterungen.

## Priorisierung

Es sollte klar sein, welche Aspekte der Zugänglichkeit besonders wichtig sind (z.B. Kontrast, Fokus, Reduzierung von Bewegung) und wo man anfangen sollte.

## Updates

Es ist wichtig zu erwähnen, dass dieser Leitfaden regelmäßig aktualisiert werden muss, da sich Webstandards und Technologien ständig weiterentwickeln.

## Ausnahme- und Sonderfälle

Sonderfälle wie komplexe Tabellen, Karten oder Diagramme erfordern oft spezielle CSS-Techniken. Darauf sollte eingegangen werden.

## Fazit

CSS spielt eine entscheidende Rolle bei der Verbesserung der Webzugänglichkeit. Durch die Anwendung dieser Techniken auf einer soliden HTML-Grundlage können Sie eine inklusivere und ansprechendere Erfahrung für Benutzer aller Fähigkeiten schaffen. Denken Sie daran, dass Barrierefreiheit ein fortlaufender Prozess ist. Regelmäßige Tests und Anpassungen sind wichtig, um sicherzustellen, dass Ihre Website für alle Benutzer zugänglich bleibt.

## Ressourcen

*   [How to Use CSS to Improve Web Accessibility (freeCodeCamp)](https://www.freecodecamp.org/news/how-to-use-css-to-improve-web-accessibility/)
*   [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
*   [WAVE Web Accessibility Evaluation Tool](https://wave.webaim.org/)
*   [Axe DevTools - Web Accessibility Testing](https://www.deque.com/axe/)
*   [A11Y Project Checklist](https://www.a11yproject.com/checklist/)
*   [MDN Web Docs: CSS and Accessibility](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/CSS_and_JavaScript)
*   [W3C Web Accessibility Initiative (WAI)](https://www.w3.org/WAI/)
*   [Inclusive Components by Heydon Pickering](https://inclusive-components.design/)
*   [Konkrete Beispiele von "best practice" Seiten (hier Links einfügen)]
*   [Spezielle Web Accessibility Tutorials (z.B. zum Thema Tabellen, Formulare, etc.) (hier Links einfügen)]
