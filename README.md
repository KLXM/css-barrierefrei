# Leitfaden zur Verbesserung der Webzugänglichkeit mit CSS

Based on: [How to Use CSS to Improve Web Accessibility (freeCodeCamp)](https://www.freecodecamp.org/news/how-to-use-css-to-improve-web-accessibility/)

CSS spielt eine entscheidende Rolle bei der Verbesserung der Webzugänglichkeit. Dieser Leitfaden bietet einen umfassenden Überblick über verschiedene CSS-Techniken und Best Practices zur Steigerung der Barrierefreiheit.

## Inhaltsverzeichnis

1. [Voraussetzungen](#voraussetzungen)
2. [Fokus-Stile aktualisieren](#fokus-stile-aktualisieren)
3. [Inhaltsverschiebungen vermeiden](#inhaltsverschiebungen-vermeiden)
4. [Bewegung reduzieren](#bewegung-reduzieren)
5. [Fokus innerhalb für verschachtelte Elemente](#fokus-innerhalb-für-verschachtelte-elemente)
6. [Kontrastoptionen anpassen](#kontrastoptionen-anpassen)
7. [Dunkelmodus aktivieren](#dunkelmodus-aktivieren)
8. [rem-Einheiten für responsive Typografie](#rem-einheiten-für-responsive-typografie)
9. [Viewport-basierte Einheiten für responsive Typografie](#viewport-basierte-einheiten-für-responsive-typografie)
10. [Animationen zur Verbesserung der UX](#animationen-zur-verbesserung-der-ux)
11. [Inhalte je nach Lesegerät ein- und ausblenden](#inhalte-je-nach-lesegerät-ein--und-ausblenden)
12. [Farben und Kontrast](#farben-und-kontrast)
13. [Flexibles Layout und Responsive Design](#flexibles-layout-und-responsive-design)
14. [Formular-Styling für bessere Zugänglichkeit](#formular-styling-für-bessere-zugänglichkeit)
15. [Barrierefreie Tabellen](#barrierefreie-tabellen)
16. [Unterstützung für Bildschirmlupen](#unterstützung-für-bildschirmlupen)
17. [Fazit](#fazit)
18. [Ressourcen](#ressourcen)

## Voraussetzungen

Um diesem Tutorial zu folgen, sollten Sie über grundlegende Kenntnisse in HTML, CSS und etwas JavaScript verfügen.

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

## rem-Einheiten für responsive Typografie

Verwenden Sie `rem`-Einheiten für eine bessere Skalierbarkeit der Schriftgrößen.

```css
html {
  font-size: 16px;
}

h1 {
  font-size: 2.5rem;
}

p {
  font-size: 1rem;
  line-height: 1.5;
}
```

## Viewport-basierte Einheiten für responsive Typografie

Viewport-basierte Einheiten passen sich direkt an die Bildschirmgröße an:

```css
h1 {
  font-size: clamp(1.5rem, 5vw, 3rem);
}

p {
  font-size: calc(1rem + 0.5vw);
}
```

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

## Fazit

CSS spielt eine entscheidende Rolle bei der Verbesserung der Webzugänglichkeit. Durch die Anwendung dieser Techniken auf einer soliden HTML-Grundlage können Sie eine inklusivere und ansprechendere Erfahrung für Benutzer aller Fähigkeiten schaffen. Denken Sie daran, dass Barrierefreiheit ein fortlaufender Prozess ist. Regelmäßige Tests und Anpassungen sind wichtig, um sicherzustellen, dass Ihre Website für alle Benutzer zugänglich bleibt.

## Ressourcen

- [How to Use CSS to Improve Web Accessibility (freeCodeCamp)](https://www.freecodecamp.org/news/how-to-use-css-to-improve-web-accessibility/)
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [WAVE Web Accessibility Evaluation Tool](https://wave.webaim.org/)
- [Axe DevTools - Web Accessibility Testing](https://www.deque.com/axe/)
- [A11Y Project Checklist](https://www.a11yproject.com/checklist/)
- [MDN Web Docs: CSS and Accessibility](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/CSS_and_JavaScript)
- [W3C Web Accessibility Initiative (WAI)](https://www.w3.org/WAI/)
- [Inclusive Components by Heydon Pickering](https://inclusive-components.design/)
