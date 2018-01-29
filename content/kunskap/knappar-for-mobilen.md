---
author: efo
category: javascript
revision:
  "2018-01-18": (A, efo) Första utgåvan inför kursen linux V18.
...
Knappar för mobilen
==================================
[FIGURE src=image/webapp/button.jpg?w=c5 class="right"]

Vi har sedan tidigare gjort CSS kod för navigation och typografi och vi ska i denna övning bygga ytterligare komponenter för våra appar. I slutet av övningen ska vi titta på hur vi kan strukturera CSS koden, både som ren CSS men även med hjälp av SASS en CSS-preprocessor.

<!--more-->



Introduktion {#intro}
--------------------------------------
Knappar är en viktig del av de flesta CSS ramverk och kanske en av de mera omdiskuterade delar. Speciellt introduktionen av Flat Design i Apples iOS 7 och Googles Material Design visade på hur bra decor ibland kan vara dålig design. Problemet med Flat Design är att användaren ofta har problem att urskilja vad som går att klicka på och inte. I dessa två artiklar kan intresserade läsa mer: [The Problem with Flat Design According to a UX Expert](https://www.fastcodesign.com/3058094/the-problem-with-flat-design-according-to-a-ux-expert) och [Tablet Usability](https://www.nngroup.com/articles/tablet-usability/).

En annan utmaning när man designar för små mobila enheter är att man inte har samma precision när man använder händerna, som med en gammal hederlig dator mus. Därför är det viktigt att designa knappar och andra komponenter så de är lätta att interagera med trots avsaknaden av precision.



En knapp {#intro}
--------------------------------------
Vi börjar med att definiera en klass för knappar `.button`. Vi vill att denna klassen ska kunna användas för flera olika HTML-element som till exempel `a`, `button` och `input[type=submit]`. I kod exemplet nedan har vi en grundstruktur med `index.html`, `normalize.min.css` och `style.css`. Jag har lagt till tre stycken element som vi ska designa i denna övning genom att använda klassen `.button`.

```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <title>Button</title>

    <link rel="stylesheet" href="normalize.min.css" />
    <link href="https://fonts.googleapis.com/css?family=Source+Sans+Pro" rel="stylesheet">
    <link rel="stylesheet" href="style.css" />
</head>
<body>
    <main class="container">
        <a href="#" class="button">anchor</a><br><br>
        <button class="button">button</button><br><br>
        <input type="submit" value="input[type=submit]" class="button"><br><br>
    </main>
</body>
</html>
```

```css
/* style.css */

body {
    font-family: 'Source Sans Pro', sans-serif;
}

.container {
    padding: 1rem;
    line-height: 1.4;
    font-size: 1.2rem;
}
```

Trots att vi använder `normalize.min.css`, som nollställer grundstilen i webbläsarna, ser vi ändå en liten skillnad om vi jämför Chrome med Firefox.

[FIGURE src=image/webapp/screenshot-button-chrome.png?w=c7 class="right" caption="Tre grundelement i Chrome"]
[FIGURE src=image/webapp/screenshot-button-firefox.png?w=c7 caption="Tre grundelement i Firefox"]

För att få de tre grundelement till att vara likadana skapar vi en tunn ram runt alla tre, tar bort understrykningen för `a`-elementet, sätter textfärgen till mörkgrå och bakgrundsfärgen till vit.

```css
.button {
    color: #333;
    background-color: #fff;
    border: 1px solid #ccc;
    text-decoration: none;
    box-sizing: border-box;
    font-family: 'Source Sans Pro', sans-serif;
    font-size: 1.4rem;
}
```

För att få mer yta att trycka på i knapparna lägger vi till `padding` och vi centrerar texten i mitten av knappen. Vi justerar radavståndet då detta skiljer sig åt för de olika elementen. För att få till ett lite mjukare utseende på knapparna lägger vi till rundade hörn.

```css
.button {
    color: #333;
    background-color: #fff;
    border: 1px solid #ccc;
    text-decoration: none;
    box-sizing: border-box;
    font-family: 'Source Sans Pro', sans-serif;
    font-size: 1.4rem;
    padding: 0.4rem 1rem;
    text-align: center;
    border-radius: 0.2rem;
    line-height: 1.4;
}
```

Nu har vi nått som liknar knapparna på iOS och Android, men viss kritik riktas mot design filosofin Flat Design, som vi såg ovan. Om man vill designa sin webb-app specifikt för ett mobil operativsystem kan det vara bra att följa designriktlinjer från det operativsystemet. Vi kommer dock fortsätta med vår design då vi vill ha knappar, som ser ut som de går att trycka på och vi vill även lägga till färg. Vi lägger en gradient in i knapparna som får det att se ut som knappen är i 3D.

```css
.button {
    color: #333;
    background-color: #fff;
    background-image: linear-gradient(#fff, #eee);
    border: 1px solid #ccc;
    text-decoration: none;
    box-sizing: border-box;
    font-family: 'Source Sans Pro', sans-serif;
    font-size: 1.4rem;
    padding: 0.4rem 1rem;
    text-align: center;
    border-radius: 0.2rem;
    line-height: 1.4;

}
```

Även om vi primärt designar för mobila enheter är det alltid läckert om det även är bra för enheter där man har en muspekare. Så vi lägger till två stycken pseudo-klasser.

```css
.button:hover {
    background-color: #ddd;
    background-image: none;
}

.button:active {
    background-color: #dcdcdc;
    background-image: none;
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.15);
}
```

Vi har nu en grunddesign för våra knappar, alla tre element ser likadana ut och de inbjuder till att bli klickade.

[FIGURE src=image/webapp/screenshot-button-base.png?w=c7 caption="Grunddesign för knappar"]

En sak vi vill göra med knappar på mobila enheter är att de ska kunna fylla ut hela sidans bredd. Detta gör att de är lätta att klicka på oavsett om man är vänster- eller högerhänt. Då vi har definierat att knapparna använder sig av `box-sizing: border-box;` är detta enkelt genom att bara sätta bredden till 100%.

```css
.full-width-button {
    width: 100%;
}
```



Färger {#colors}
--------------------------------------
För att knappar kan signalera olika funktioner introducerar vi färger. En röd knapp betyder fara, en gul varning, blå eller grön att allt är lugnt. Vi lägger till ytterligare klasser då vi inte vill ändra på grunddesignen. Ett exempel på dessa klasser syns i kod exemplet nedan för en blå knapp. Notera att färgen på typsnittet har ändrats då vi inte vill ha ett mörkt typsnitt på en mörk bakgrund. En blå knapp definieras nu som `<button class="button blue-button">button</button>`.

```css
.blue-button {
    color: #fff;
    background-color: #2049e1;
    background-image: linear-gradient(#2049e1, #0039d0);
    border-color: #0029c0;
}

.blue-button:hover {
    background-color: #0029c0;
    background-image: none;
}

.blue-button:active {
    background-color: #0029c0;
    background-image: none;
    box-shadow: inset 0 2px 4px #0019b0;
}
```



Struktur i CSS {#struktur}
--------------------------------------
Nu börjar vi få en hel del CSS kod där olika komponenter ligger blandat i samma CSS fil. För att strukturera upp detta kan det vara en fördel att dela upp koden i olika moduler. Ett enkelt och fullt tillräckligt sätt är att dela upp CSS-koden i olika filer och importera alla filer i `index.html`.

Ett smidigare och mer kraftfullt sätt är att använda sig av en CSS-preprocessor. Fördelen med en CSS-preprocessor är inte bara att man kan samla koden i moduler och exportera en enda CSS fil, men finns inbyggda funktioner som underlättar vid hantering av färg, typsnitt och import av olika moduler.

Vissa har i kursen design träffat på LESS, det går alldeles utmärkt att använda sig av LESS även i denna kursen, men i följande exempel används [SASS](http://sass-lang.com/). För att installera SASS följ instruktionerna på [installationssidan](http://sass-lang.com/install).

Följande är en kort introduktion till import, variabler och färghantering i SASS. För mer avancerade funktioner rekommenderas [SASS dokumentationen](http://sass-lang.com/documentation/file.SASS_REFERENCE.html).

För att importera CSS och SASS filer och typsnitt används `@import` och variabler skrivs med ett dollar-tecken framför. Så i följande kodexempel importeras typsnittet Merriweather och det skapas en variabel för brödtext. Variabeln används sedan för att ge `p`-element ett typsnitt.

```scss
@import url('https://fonts.googleapis.com/css?family=Merriweather');

$font-body: 'Merriweather', serif;

p {
    font-family: $font-body;
}
```

När vi skapade våra knappar använde vi variationer av samma färg för ramen och skuggningen. Med CSS-preprocessors är det busenkelt att räkna fram dessa variationer av färgerna. Med funktioner som `lighten` och `darken` kan vi ljusa upp eller mörka våra grundfärger enligt exemplet nedan där vi i gradienten gör färgen `$blue` 10% mörkare och ramen görs 20% mörkare.

```scss
$blue: #0074d9;

.blue-button {
    background-color: $blue;
    background-image: linear-gradient($blue, darken($blue, 10%));
    border-color: darken($blue, 20%);
}
```

För att strukturera CSS-koden börjar vi med att skapa en fil `base.scss` där vi importerar alla moduler med hjälp av till exempel `@import 'navigation'`. Det är denna fil vi använder när vi sedan ska kompilera SASS till CSS. Jag använder `.scss`-filer då jag gillar syntaxen bättre då den påminner om CSS och ger möjlighet för att återanvända befintlig CSS. Men det är fritt fram att använda `.sass` syntax, om ni tycker om den.

Den resulterande `base.scss` blir en samling `@import`, som exemplet visar nedan. Notera att jag inte har med filändelsen på alla `.scss`-filer, detta då SASS automatisk hittar SASS-filerna.

```scss
@import url('https://fonts.googleapis.com/css?family=Merriweather|Source+Sans+Pro');

@import 'normalize.min.css';
@import 'style/variables';
@import 'style/container';
@import 'style/navigation';
@import 'style/typography';
@import 'style/button';
```

När vi sedan vill skapa en CSS fil av SASS koden används följande kommando på kommandoraden.

```bash
sass base.scss style.css
```

Om man istället vill skapa en komprimerad version av CSS koden kan man använda följande kommando.

```bash
sass base.scss style.min.css --style compressed
```



Avslutningsvis {#avslutning}
--------------------------------------
Vi har i denna artikeln skapat knappar som är lätta att klicka på och samtidigt inbjuder till att bli klickade på. Vi designade först en grund knapp och med hjälp av andra klasser designade vi knappar som fyller hela skärmens bredd och knappar med andra färger för olika funktioner. Vi har även tittat på ett sätt att strukturera vår CSS kod med CSS-preprocessorn SASS.