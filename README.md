<!--
author:   André Dietrich, Sebastian Zug

email:    andre.dietrich@ovgu.de

version:  0.0.1

language: de

narrator: Deutsch Female

comment:  Demo für den 25. sächsischen Schulinformatiktag.


script:   https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js

@Rextester.__eval
<script>
//var result = null;
var error  = false;

console.log = function(e){ send.lia("log", JSON.stringify(e), [], true); };

function grep_(type, output) {
  try {
    let re_s = ":(\\d+):(\\d+): "+type+": (.+)";

    let re_g = new RegExp(re_s, "g");
    let re_i = new RegExp(re_s, "i");

    let rslt = output.match(re_g);

    let i = 0;
    for(i = 0; i < rslt.length; i++) {
        let e = rslt[i].match(re_i);

        rslt[i] = { row : e[1]-1, column : e[2], text : e[3], type : type};
    }
    return [rslt];
  } catch(e) {
    return [];
  }
}

$.ajax ({
    url: "https://rextester.com/rundotnet/api",
    type: "POST",
    timeout: 10000,
    data: { LanguageChoice: @0,
            Program: `@input`,
            Input: `@1`,
            CompilerArgs : @2}
    }).done(function(data) {
        if (data.Errors == null) {
            let warnings = grep_("warning", data.Warnings);

            let stats = "\n-------Stat-------\n"+data.Stats.replace(/, /g, "\n");

            if(data.Warnings)
              stats = "\n-------Warn-------\n"+data.Warnings + stats;

            send.lia("log", data.Result+stats, warnings, true);
            send.lia("eval", "LIA: stop");

        } else {
            let errors = grep_("error", data.Errors);

            let stats = "\n-------Stat-------\n"+data.Stats.replace(/, /g, "\n");

            if(data.Warning)
              stats = data.Errors + data.Warnings + stats;
            else
              stats = data.Errors + data.Warnings + stats;

            send.lia("log", stats, errors, false);
            send.lia("eval", "LIA: stop");
        }
    }).fail(function(data, err) {
        send.lia("log", err, [], false);
        send.lia("eval", "LIA: stop");
    });

"LIA: wait"
</script>
@end


@Rextester.eval: @Rextester.__eval(6, ,"-Wall -std=gnu99 -O2 -o a.out source_file.c")

@Rextester.eval_params: @Rextester.__eval(6, ,"@0")

@Rextester.eval_input: @Rextester.__eval(6,`@input(1)`,"-Wall -std=gnu99 -O2 -o a.out source_file.c")


script:   https://cdnjs.cloudflare.com/ajax/libs/processing.js/1.6.6/processing.min.js


@processing
<script>
let container = document.getElementById('sketch-container-@0');

container.innerHTML = "";

let canvas = document.createElement('canvas');
canvas.id = 'sketch-@0';

container.appendChild(canvas);

if(!window["processing"]){
    window["processing"] = {};
    window["running"] = null;
}

if(window["running"]) {
  window["processing"][window["running"]].exit();

}

let sketch = Processing.compile(`@input`);

window["processing"]["@0"] = new Processing(canvas, sketch);
window["running"] = "@0";
</script>

<div id="sketch-container-@0"></div>

<script>
if(window["running"]) {
    window["processing"][window["running"]].exit();
    document.getElementById("sketch-container-"+window["running"]).innerHTML = "";

    window["running"] = null;
}
</script>
@end


script:   https://cdn.jsdelivr.net/chartist.js/latest/chartist.min.js

link: https://cdn.jsdelivr.net/chartist.js/latest/chartist.min.css


@Chartist
<div class="ct-chart ct-golden-section" id="chart@0">
</div>
<script>
  $.getScript("https://cdn.jsdelivr.net/chartist.js/latest/chartist.min.js", function(){
    let x = new Chartist.Line('#chart@0', {@1});
  });
</script>

@end

@red: <b style="color: red"> @0</b>

-->

# LiaScript: Demo


                              {{0-1}}
> __1.__ Nutzen Sie die oberen Pfeile um im Kurs weiter zugehen.
>
> __2.__ Schalten Sie den Ton für die Sprachausgabe ein.


                             --{{1}}--
LiaScript ist eine Erweiterung der Auszeichnungssprache
[Markdown](https://de.wikipedia.org/wiki/Markdown). Das ursprüngliche  Ziel von
Markdown war es, die Ausgangsform auch ohne weitere Konvertierung für einen
Menschen leicht lesbar und damit auch editierbar zu machen.


                              {{1-2}}
```markdown Markdown-Beispiel.md
# Überschrift

Absätze werden durch eine Leerzeile voneinander getrennt.

2ter Absatz. *Kursiv*, **fett**, und `Monospace`.
Ungeordnete Listen sehen wie folgt aus:

* dieses
* jenes
* und noch weiteres

## Abschnitt: Zitate

> Blockzitate werden
> folgendermaßen definiert...
>
> ... und können auf mehrere Absätze
> aufgeteilt werden, wenn Sie mögen.

## Abschnitt: Tabellen

| Tabellen      |       Sind       |  Cool |
| ------------- |:----------------:| -----:|
| Spalte 3 ist  | rechtsbündig     | 1600€ |
| Spalte 2 ist  | zentriert        |   12€ |
| Zebrastreifen | sind ordentlich  |    1€ |

### Unterabschnitte ...
```

                             --{{2}}--
LiaScript setzt auf dieser Notation auf, erweitert diese um viele multimediale
Elemente und erlaubt es beliebige JavaScript-Funktionalität einzubinden. Der
Fokus von LiaScript lag bei der einfachen Entwicklung von online-Kursen für
jedermann. Diese sollten einerseits über einen Screencast-Charakter verfügen,
gemixt mit dem Stil einer PowerPoint-Präsentation, aber auch interaktive
Elemente einbinden, wie Quizze, Umfragen, Animationen, online Programmierung,
etc. Es sollte einfach um zusätzliche Funktionalität erweiterbar sein und durch
ein integriertes Macro-System auch wiederverwendbare Komponenten kapseln.


                               {{2}}
* Projekt-Webseite:
  https://LiaScript.github.io
* Dieser Kurs auf:
  * GitHub --> https://github.com/LiaScript/Demo
  * LiaScript --> https://liascript.github.io/course/?https://raw.githubusercontent.com/liaScript/demo/master/README.md#1


## Erweiterungen

                             --{{0}}--
Auf den folgenden Seiten sollen kurz einige Erweiterungen vorgestellt werden.
Für eine ausführliche Erläuterung zu allen Möglichkeiten sei jedoch auf die
[LiaScript Dokumentation](https://liascript.github.io/course/?https://raw.githubusercontent.com/liaScript/docs/master/README.md#1)
auf der Projekt-Seite verwiesen.

![knowledge hit](https://media.giphy.com/media/jCL5JbYPYQz96/giphy.gif)<!--width="100%"-->


### Multimedia

                             --{{0}}--
Markdown unterstützt von Haus aus zwei Typen von Verweisen, dazu zählen
einerseits einfache Links auf externe oder interne Seiten, die wie folgt,
definiert werden:


                              {{0-1}}
*******************************************************************************

__Markdown-Code: (Verweis)__

``` markdown
* Direkter Verweis:
  https://LiaScript.github.io

* Formatierter Verweis:
  [Link zu LiaScript](https://LiaScript.github.io)
```

__Resultat:__

* Direkter Verweis: https://LiaScript.github.io
* Formatierter Verweis: [Link zu LiaScript](https://LiaScript.github.io)

*******************************************************************************

                             --{{1}}--
Bilder werden wie Verweise behandelt, nur das hier ein Ausrufezeichen vor den
formatierten Verweis gestellt werden muss:

                              {{1-2}}
*******************************************************************************

__Markdown-Code: (Bild)__

``` markdown
 ![Schule im Jahr 2000](https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/France_in_XXI_Century._School.jpg/1024px-France_in_XXI_Century._School.jpg)<!--width="100%"-->
```

__Resultat:__

![Schule im Jahr 2000](https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/France_in_XXI_Century._School.jpg/1024px-France_in_XXI_Century._School.jpg)<!--
  width="100%"
-->

*******************************************************************************


                             --{{2}}--
Zum Einfügen von Audio-Elementen in LiaScript muss nur ein Fragezeichen
vorangestellt werden, welches mit etwas Fantasie auch als stilisiertes Ohr
interpretiert werden kann.

                              {{2-3}}
*******************************************************************************

__LiaScript-Code: (Audio)__

``` markdown
 ?[Joy to the world](http://www.orangefreesounds.com/wp-content/uploads/2018/11/Joy-to-the-world-song.mp3?_=1)
```

__Resultat:__

?[Joy to the world](http://www.orangefreesounds.com/wp-content/uploads/2018/11/Joy-to-the-world-song.mp3?_=1)

*******************************************************************************


                             --{{3}}--
Und kombiniert man Bild und Ton, so kann man auch Videos einfügen. Diese beiden
Optionen sind sonst unter Markdown nicht gegeben. Der Vorteil bei dieser
Darstellung ist auch, dass die Referenzen mit einem anderen Markdown-Viewer
immer noch als Verweise dargestellt werden.


                              {{3-4}}
*******************************************************************************

__LiaScript-Code: (Video)__

``` markdown
!?[Das eLab-Projekt auf YouTube](https://www.youtube.com/embed/bICfKRyKTwE)<!--
  width="560px"
  height="315px"
-->
```

__Resultat:__

!?[Das eLab-Projekt auf YouTube](https://www.youtube.com/embed/bICfKRyKTwE)<!--width="560px" height="315px"-->

*******************************************************************************

                             --{{4}}--
Dem aufmerksamen Betrachter wird nicht entfallen sein, dass in manchen
Beispielen HTML-Kommentare mit zusätzlichen HTML-Annotationen enthalten waren.
Dies ist eine Möglichkeit, jedes Element, sei es Text, Bild, Tabelle oder Video,
noch mit weiteren Eigenschaften zu versehen. Ein vorangestellter Kommentar
definiert die Attribute für einen ganzen Block, während ein hinten angestelltes
nur die Eigenschaften des vorangegangenen Elementes verändert.


                               {{4}}
*******************************************************************************

__LiaScript-Code: (Annotationen)__

``` markdown
<!--style="color: red"-->
Dieser ganze Absatz ist in Rot gefasst ;-)<!--
class="animated infinite bounce"
style="animation-delay: 3s;"
-->, mit Ausnahme diese Smileys, dass nach drei
Sekunden zu hüpfen beginnt.
```

__Resultat:__

<!--style="color: red"-->
Dieser ganze Absatz ist in Rot gefasst ;-)<!--
class="animated infinite bounce"
style="animation-delay: 3s;"
-->, mit Ausnahme diese Smileys, dass nach drei
Sekunden zu hüpfen beginnt.

*******************************************************************************

                             --{{5}}--
Damit lassen sich auch komplexe Animationsabläufe definieren, während der Text
mit einem anderen Markdown-Viewer noch lesbar bleibt, da Kommentare von diesen
einfach ignoriert werden.


### ASCII-Art 1

                             --{{0}}--
Aus unserer Erfahrung wissen wir, dass Bilder häufig zur Darstellung von
einfachen Signalverläufen, Trends, Funktionen, also Diagrammen genutzt werden.
Diese müssen meist aufwendig mit Excel, Gnuplot, Matlab oder anderen Werkzeugen
erstellt und exportiert werden. LiaScript bietet hierzu auch die Möglichkeit
Diagramme auch direkt im Text zu kodieren, diese lassen sich leicht anpassen und
verändern und man muss nicht sein "Werkzeug" wechseln.

``` HTML
                       Der Titel ist optional
    1.9 |    DotS
        |                 ***
      y |               *     *
      - | r r r r r r r*r r r r*r r r r r r r
      A |             *         *
      c |            *           *
      h | B B B B B * B B B B B B * B B B B B
      s |         *                 *
      e | *  * *                       * *  *
        +------------------------------------
        0              x-Achse              1
```


                             --{{1}}--
Das konvertierte Ergebnis ist ein grafisch ansehnliches Diagramm, in dem die
Farben und die Form und Größe der Punkte jeweils durch die ursprünglichen
Buchstaben kodiert sind.


                               {{1}}
                       Der Titel ist optional
    1.9 |    DotS
        |                 ***
      y |               *     *
      - | r r r r r r r*r r r r*r r r r r r r
      A |             *         *
      c |            *           *
      h | B B B B B * B B B B B B * B B B B B
      s |         *                 *
      e | *  * *                       * *  *
        +------------------------------------
        0              x-Achse              1


### ASCII-Art 2

                             --{{0}}--
Wer möchte, der kann auch noch kompliziertere Sachverhalte, wie zum Beispiel
Graphen, UML-Diagramme oder Bilder mithilfe von einfachen Textbausteinen
abbilden.

``` HTML
                           .--->  F
  A       B     C   D     /
  *-------*-----*---*----*----->  E
           \            ^ \
            v          /   '--->  G
             B --> C -'
```

<!-- style="display: block; margin-left: auto; margin-right: auto; max-width: 315px;" -->
`````````````````````````````````````
                           .--->  F
  A       B     C   D     /
  *-------*-----*---*----*----->  E
           \            ^ \
            v          /   '--->  G
             B --> C -'
`````````````````````````````````````

                             --{{1}}--
Und dazu auch alle möglichen Unicode-Zeichen nutzen. Da LiaScript international
ist und somit auch chinesischen, griechische, arabische oder anderweitige
Zeichen und Symbole interpretieren kann.

                               {{1}}
*******************************************************************

```
  ┌─┬┐  ╔═╦╗  ╓─╥╖  ╒═╤╕
  │ ││  ║ ║║  ║ ║║  │ ││
  ├─┼┤  ╠═╬╣  ╟─╫╢  ╞═╪╡
  └─┴┘  ╚═╩╝  ╙─╨╜  ╘═╧╛
  ┌───────────────────┐
  │  ╔═══╗ Some Text  │▒
  │  ╚═╦═╝ in the box │▒
  ╞═╤══╩══╤═══════════╡▒
  │ ├──┬──┤           │▒
  │ └──┴──┘           │▒
  └───────────────────┘▒
   ▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
```

<!-- style="display: block; margin-left: auto; margin-right: auto; max-width: 315px;" -->
````````````````````````````````
  ┌─┬┐  ╔═╦╗  ╓─╥╖  ╒═╤╕
  │ ││  ║ ║║  ║ ║║  │ ││
  ├─┼┤  ╠═╬╣  ╟─╫╢  ╞═╪╡
  └─┴┘  ╚═╩╝  ╙─╨╜  ╘═╧╛
  ┌───────────────────┐
  │  ╔═══╗ Some Text  │▒
  │  ╚═╦═╝ in the box │▒
  ╞═╤══╩══╤═══════════╡▒
  │ ├──┬──┤           │▒
  │ └──┴──┘           │▒
  └───────────────────┘▒
   ▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
````````````````````````````````

*******************************************************************


### Quizze

                             --{{0}}--
Ein Quiz ist eine gute Möglichkeit dem Lernenden kurz nochmal die Möglichkeit zu
geben, sein Wissen bezüglich eines aktuellen Lernmoduls zu überprüfen. LiaScript
unterstützt grundsätzlich 4 Quiz-Typen, die durch zusätzliche Elemente
angepasst werden können:

                              {{0-1}}
1. Texteingaben
2. Single-Choice
3. Multiple-Choice
4. Freie, mithilfe von JavaScript


                             --{{1}}--
Ein Text-Quiz wird durch doppelt eckige Klammern definiert, welches diese
jeweilige Lösung schon enthält. Diese Lösung ist für den Nutzer nicht sichtbar.
Er kann seine Eingaben mehrfach prüfen und gegebenenfalls die Auflösung
anfordern:


                              {{1-2}}
*******************************************************************************

``` markdown Text-Quiz 1
**Welche Markdown-Erweiterung wird hier vorgestellt?**

    [[LiaScript]]
```

---

**Welche Markdown-Erweiterung wird hier vorgestellt?**

    [[LiaScript]]

*******************************************************************************


                             --{{2}}--
Hinweise können in der dargestellten Notation an jeden Quiz-Typ angehängt
werden, beziehungsweise kann auch jedes Quiz mit einer erweiterten Erklärung zur
Auflösung versehen werden. Diese Erklärung muss nur in einen Block
eingeschlossen werden, der durch zwei Reihen "Sternchen" definiert ist. Der
zusätzliche `script`-Block erlaubt es die aktuelle Eingabe gesondert zu
überprüfen und damit auf verschiedene Schreibweisen (Groß/klein) sowie den
Einschub von Leerzeichen zu prüfen.

                              {{2-3}}
*******************************************************************************

``` markdown Text-Quiz 2
**Welche Markdown-Erweiterung wird hier vorgestellt?**

    [[LiaScript]]
    [[?]] Es handelt sich dabei um eine neue Sprache...
    [[?]] Diese wurde speziell zur Erstellung von online-Kursen entwickelt.
    [[?]] Es kommen ein __L__ und ein __S__ darin vor.
    <script>
      let input = "@input".trim().toLowerCase();
      input == "liascript";
    </script>
    **************************

    Diese zusätzliche Auflösung ist optional und kann so __viele__
    Markdown-Blöcke und Absätze, Bilder, Videos oder Formeln
    enthalten, wie sie wünschen ...

    $$
       \sum_{i=1}^\infty\frac{1}{n^2}
            =\frac{\pi^2}{6}
    $$

    **************************
```

---

**Welche Markdown-Erweiterung wird hier vorgestellt?**

    [[LiaScript]]
    [[?]] Es handelt sich dabei um eine neue Sprache...
    [[?]] Diese wurde speziell zur Erstellung von online-Kursen entwickelt.
    [[?]] Es kommen ein __L__ und ein __S__ darin vor.
    <script>
      let input = "@input".trim().toLowerCase();
      input == "liascript";
    </script>
    **************************

    Diese zusätzliche Auflösung ist optional und kann so __viele__
    Markdown-Blöcke und Absätze, Bilder, Videos oder Formeln
    enthalten, wie sie wünschen ...

    $$
       \sum_{i=1}^\infty\frac{1}{n^2}
            =\frac{\pi^2}{6}
    $$

    **************************

*******************************************************************************


                             --{{3}}--
Ein Single-Choice-Quiz wird durch stilisierte Radio-Buttons dargestellt, wobei
das `X` hier die richtige Lösung markiert. Hierbei darf nur eine Zeile ein `X`
enthalten. Erweiterungen um Hinweise und Auflösungen sind genauso wie bei einem
Text-Quiz möglich.


{{3-4}}
*******************************************************************************

``` markdown Single-Choice-Quiz
**Welche Antwort trifft zu?**

    [( )] Antwort 1
    [( )] Antwort 2
    [(X)] Antwort 3
    [( )] Antwort 4
```

---

**Welche Antwort trifft zu?**

    [( )] Antwort 1
    [( )] Antwort 2
    [(X)] Antwort 3
    [( )] Antwort 4

*******************************************************************************


                             --{{4}}--
Mehrere Auswahloptionen und damit Multiple-Choice-Fragen können durch eine
Gruppe sogenannter Check-Buttons angegeben werden. Dabei können auch alle
Elemente durch ein `X` markiert sein oder keines.


                              {{4-5}}
*******************************************************************************

``` markdown Multiple-Choice-Quiz
**Welche Antworten treffen zu?**

    [[ ]] Antwort 1
    [[X]] Antwort 2
    [[ ]] Antwort 3
    [[X]] Antwort 4
    [[ ]] Antwort 5
```

---

**Welche Antworten treffen zu?**

    [[ ]] Antwort 1
    [[X]] Antwort 2
    [[ ]] Antwort 3
    [[X]] Antwort 4
    [[ ]] Antwort 5

*******************************************************************************


                             --{{5}}--
Ein generisches Quiz kann mithilfe der folgenden Notation erstellt werden, wobei
hier das Ergebnis einzig und allein von einer Zufallszahl abhängig ist. Über andere HTML-Elemente können dann auch die Eingaben abgebildet werden.


                               {{5}}
*******************************************************************************

``` markdown Generisches Quiz
[[!]]
<script>
  // you are free to check anything you want
  if(Math.random() < 0.2)
    true;
  else
    false;
</script>
```

    [[!]]
    <script>
      // you are free to check anything you want
      if(Math.random() < 0.2)
        true;
      else
        false;
    </script>

*******************************************************************************


### Ausgaben

                              --{{0}}--
Im folgenden Teil soll die Aufteilung eines Abschnittes in mehrere Fragmente
erläutert werden sowie die Nutzung der Sprachausgabe.

                               {{0-1}}
![boring](https://media.giphy.com/media/HlqvH9JrahLZ6/giphy.gif)<!-- width="100%" -->


                              --{{1}}--
Falls Sie es noch nicht bemerkt haben, oben in der rechten Ecke befindet sich
ein Knopf, der es erlaubt zwischen den unterschiedlichen Darstellungsmodi zu
wechseln. Diese Option kommt frei Haus und sie können selber entscheiden, ob sie
lieber den Erklärtext wie in einer Präsentation hören oder lieber ein Buch
lesen.
                                {{1}}
1. Präsentation (mit Text2Speech)
2. Folien (mit Notizen)
3. Lehrbuch (ohne Fragmente und Sprachausgabe)


#### Fragmente

                              --{{0}}--
Um Fragmente zu definieren, muss nur die jeweilige Fragment-Nummer in doppelt
geschweifte Klammern geschrieben werden und einem Markdown-Block vorangestellt.
Eine zweite Zahl nach einem Minus definiert, bei welchem Punkt das Fragment
wieder ausgeblendet wird. Innerhalb eines Blocks können auch einzelne Elemente
aufgedeckt werden, dazu muss die doppelt geschweifte Klammer nur ausgepackt
werden, wobei die zweiten Klammern das oder die jeweiligen Elemente umschließen,
die ein- beziehungsweise ausgeblendet werden sollen. Verschiedenen Blöcken
können die gleichen Nummern zugewiesen werden oder man umschließt sie mit zwei
Linien aus Sternchen `*`, ebenso wie es auch im Abschnitt [Quizze](#6) mit der
Auflösung gemacht wurde.  

``` markdown
                  {{1}}
Dieser Text erscheint zu{3}{__aller__}erst.


{{2-4}} Dieser Block erscheint als zweites
Fragment und verschwindet bei Punkt 4.

Ich bin immer da ...

{{4}} Ich komme zuletzt.
```

                               {{1}}
Dieser Text erscheint zu{3}{__aller__}erst.


{{2-4}} Dieser Block erscheint als zweites
Fragment und verschwindet bei Punkt 4.

Ich bin immer da ...

{{4}} Ich komme zuletzt.


#### Spracheausgabe

                              --{{0}}--
Die Sprachausgabe erfolgt mithilfe von
[ResponsiveVoice](http://responsivevoice.org). In initialen Kommentar-Tag kann
die Standardstimme definiert werden, diese kann je Abschnitt und Sprachausgabe
auch geändert werden. Die Kommentarfunktion kann als Erweiterung der
Fragment-Notation interpretiert werden und muss in doppelte Minuszeichen
eingefügt werden. Sozusagen, die Erläuterung zu einem bestimmten Unterpunkt.
Innerhalb eines Kommentars kann auch die Sprachausgabe geändert werden. In
Abhängigkeit zum aktuellen Präsentationsmodus wird dieser Text entweder
gesprochen oder im Dokument dargestellt. Des Weiteren lassen sich so auch
Dialoge zwischen unterschiedlichen Personen realisieren.

``` markdown
<!--
..
narrator: Deutsch Female
-->

# Überschrift 1

              --{{1}}--
Dieser Text wird deutsch ausgesprochen.


     --{{2 UK English Male}}--
I should speak with a UK like accent.

     --{{3 Russian Female}}--
Я говорю по-русски с женским голосом.
```

                             --{{1}}--
Dieser Text wird deutsch ausgesprochen.


                      --{{2 UK English Male}}--
I should speak with a UK like accent.

                       --{{3 Russian Female}}--
Я говорю по-русски с женским голосом.



## Eigene Erweiterungen

                             --{{0}}--
Auf den vorhergehenden Seiten wurden sprachliche Erweiterungen von Markdown
vorgestellt. Das Internet bietet aber weitaus mehr Möglichkeiten, die für einen
Kurs von Interesse sein könnten. LiaScript erlaubt es, im Gegensatz zu anderen
Markdown-Interpretern, auch JavaScript, HTML und CSS auf verschiedenste Art und
Weise zu integrieren. Außerdem bietet LiaScript eine eigene Macro-Notation um
wiederkehrende und aufwendige Aufgaben zu automatisieren.

![www](https://media.giphy.com/media/RxR1KghIie2iI/giphy.gif)<!-- width="100%"-->

### JavaScript und HTML

                             --{{0}}--
Wer mit den Möglichkeiten von Markdown nicht zufrieden ist, kann auch HTML,
JavaScript und CSS Elemente direkt einfügen. Um externe Bibliotheken zu laden,
müssen diese im "Haupt"-Kommentar des Dokumentes oder des Abschnittes definiert
sein. Handelt es sich um eine JavaScript-Bibliothek so muss das Schlüsselwort
`script` verwendet werden und bei einem Style-Sheet das Schlüsselwort `link`.

``` html
<!--
script: https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js
        https://cdn.jsdelivr.net/chartist.js/latest/chartist.min.js

link:   https://cdn.jsdelivr.net/chartist.js/latest/chartist.min.css
-->
```

                             --{{1}}--
Danach kann überall im Dokument auf die eingefügte Funktionalität mit
zugegriffen werden. Das nun dargestellte Beispiel nutzt die
JavaScript-Bibliothek [Chartist](https://gionkunz.github.io/chartist-js/), um
einen einfachen Graphen zu plotten.

                               {{1}}
``` html
<div class="ct-chart ct-golden-section" id="chart">
</div>

<script>
  let chart = new Chartist.Line('#chart', {
    labels: [1, 2, 3, 4],
    series: [[100, 120, 180, 200]]
  });
</script>
```

                             --{{2}}--
Der erzeugte Graph sieht dann wie folgt aus ...

                               {{2}}
<div class="ct-chart ct-golden-section" id="chart"></div>
<script>
$.getScript("https://cdn.jsdelivr.net/chartist.js/latest/chartist.min.js", function(){
  let chart = new Chartist.Line('#chart', {
    labels: [1, 2, 3, 4],
    series: [[100, 120, 180, 200]]
  })});
</script>


### Macros


                             --{{0}}--
Im Abschnitt [Quizze](#6) wurde bereits das `@input`-Macro genutzt um die
Stellen zu markieren, die durch die Nutzereingaben ersetzt werden sollen. Ein
Macro beginnt immer mit einem `@`-Symbol und kann im  "Haupt"-Kommentar eines
Dokumentes definiert werden. Macros beschreiben einfache Regeln für die
Textersetzung. Für das einzeilige `@red` Macro gilt, alles was nach dem
Doppelpunkt folgt definiert den Ersetzungstext. Parameterersetzungen werden
jeweils durch ein `@`-Symbol gefolgt mit einer Nummer definiert.


                              {{0-2}}
``` html
<!--
...
@red: <b style="color: red"> @0</b>
...
-->
```

                             --{{1}}--
Diese Erweiterungen können dann beliebig im Dokument eingesetzt werden, wie im
folgenden Beispiel dargestellt.

                              {{1-2}}
*******************************************************************************

``` markdown
> Dies ist ein Blockzitate mit
> einem @red(sehr wichtigen)
> Beispiel...
```

---

> Dies ist ein Blockzitate mit
> einem @red(sehr wichtigen)
> Beispiel...

*******************************************************************************

                             --{{2}}--
Ein Macro kann auch andere Macros aufrufen und komplexere Macros können wie im
Beispiel gezeigt, auch als Block definiert werden, welches aus beliebigen HTML,
Markdown, und JavaScript-Elementen besteht. In diesem Fall sollte die Nutzung
von Chartist vereinfacht werden, indem einmal die ID für das `div`-Element
verändert wird aber auch der zu zeichnende Inhalt als zweiter Parameter
übergeben wird.

                               {{2}}
``` html
<!--
...
@Chartist
<div class="ct-chart ct-golden-section" id="chart@0">
</div>
<script>
  let chart = new Chartist.Line('#chart@0', {@1});
</script>

@end
...
-->
```

                             --{{3}}--
Auch dieses Macro kann über die bekannte "funktionsähnliche" Notation aufgerufen
werden. Da Kommas als Separatoren für die Parameter genutzt werden, müssen hier
sogenannte back-ticks verwendet werden, um den zweiten Parameter als ganzen
String zu übergeben. Zugegebenermaßen kann dies für sehr lange Eingaben auch
sehr schnell unleserlich werden.


                              {{3-4}}
*******************************************************************************

``` markdown
@Chartist(id1,`labels: [1,2,3], series: [[1,3,1]]`)
```

---

@Chartist(id1,`labels: [1,2,3], series: [[1,3,1]]`)

*******************************************************************************

                             --{{4}}--
Aus diesem Grund können Macros auch über einen Code-Block aufgerufen werden,
dazu muss nur im Kopf des Blockes das jeweilige Macro aufgerufen werden. Der
Körper des Blocks wird dann insgesamt als letzter Parameter an die Textersetzung
übergeben. Neben der übersichtlicheren Schreibweise werden von allen gängigen
Markdown-Viewern diese Elemente zumindest als Code-Block mit Syntax-Highlighting
richtig dargestellt und erlaubt zumindest die Interpretation der Parameter.


                               {{4}}
*******************************************************************************

``` markdown
` ` `json @Chartist(id2)
labels: [1, 2, 3, 4, 5, 6, 7],
series: [
  [100, 120, 180, 200, 0, 12, -1],
  [10, 20, 30, 40, 50, 90, -100]]
` ` `
```

---

```json @Chartist(id2)
labels: [1, 2, 3, 4, 5, 6, 7],
series: [
  [100, 120, 180, 200, 0, 12, -1],
  [10, 20, 30, 40, 50, 90, -100]]
```

*******************************************************************************



## Programmierung

                             --{{0}}--
Durch die folgende Syntax können mehrere standard Markdown Code-Blöcke zu einem
Projekt zusammengefasst werden. Die unterschiedlichen Dateien können mit einem
Namen versehen werden und lassen sich auf und zu klappen. Das zusätzliche
`script`-tag am Ende weist diese Blöcke als ausführbaren Code aus und definiert
wie mit den Inhalten der einzelnen Blöcke zu verfahren ist.

                              {{0-1}}
``` markdown
` ` ` js     -EvalScript.js
let who = data.first_name + " " + data.last_name;

if(data.online) {
  who + " is online"; }
else {
  who + " is NOT online"; }
` ` `
` ` ` json    +Data.json
{
  "first_name" :  "Sammy",
  "last_name"  :  "Shark",
  "online"     :  true
}
` ` `
<script>
  // insert the JSON dataset into the local variable data
  let data = @input(1);

  // eval the script that uses this dataset
  eval(`@input(0)`);
</script>
```

                             --{{1}}--
Die LiaScript-Interpretation dieser Blöcke sieht dann wie folgt aus. Alle
Dateien sind editierbar und es wird, um Änderungen zu verfolgen, auch ein
eigenes lineares Versions-Management-System genutzt. Probieren Sie es aus,
verändern Sie den Code und kehren zu früheren Versionen zurück.

                              {{1-2}}
``` js     -EvalScript.js
let who = data.first_name + " " + data.last_name;

if(data.online) {
  who + " is online"; }
else {
  who + " is NOT online"; }
```
``` json    +Data.json
{
  "first_name" :  "Sammy",
  "last_name"  :  "Shark",
  "online"     :  true
}
```
<script>
  // insert the JSON dataset into the local variable data
  let data = @input(1);

  // eval the script that uses this dataset
  eval(`@input(0)`);
</script>


                             --{{2}}--
Da es, wie in Abschnitt [Eigene Erweiterungen](#10) gezeigt, auch möglich ist
verschiedene JavaScript-Funktionalität und Bibliotheken einzubinden, können auch
unterschiedliche Programmiersprachen unterstützt werden. Das Beispiel zeigt ein
einfaches _C_-Programm, dass mithilfe der
[rextester-API](https://rextester.com/main) kompiliert und ausgeführt werden
kann. Die etwas komplexere Definition im benötigten `script`-tag wurde hier
mithilfe des Macros `@Rextester.eval` zur Verfügung gestellt. Auf diese Weise
können beliebige ausführbare Code-Fragmente in einem Dokument definiert werden.


                              {{2-3}}
``` c source_file.c
#include <stdio.h>
int main()
{
  int i; // Löschen Sie das Semikolon und
         // schauen was passiert ...

  for(i=0; i<10; ++i) {
    printf("Hello, World! #% d\n", i);
  }
  return 0;
}
```
@Rextester.eval


                             --{{3}}--
Auch die Kombination mit anderen Sprachen und Visualisierungen ist möglich, wie
hier zum Beispiel mit [_Processing_](https://de.wikipedia.org/wiki/Processing):

                               {{3}}
``` cpp ABSTRACT01js
int num,cnt,px,py,fadeInterval;
Particle[] particles;
boolean initialised=false,doClear=false;
float lastRelease=-1,scMod,fadeAmount;

void setup() {
  size(400,300);
  background(255);
  smooth();
  rectMode(CENTER_DIAMETER);
  ellipseMode(CENTER_DIAMETER);

  cnt=0;
  num=150;
  particles=new Particle[num];
  for(int i=0; i<num; i++) particles[i]=new Particle();

  reinit();
  px=-1;
  py=-1;
}

void draw() {
  if(doClear) {
    background(255);
    doClear=false;
  }

  noStroke();

  if(frameCount%fadeInterval==0) {
    fill(255,255,255, fadeAmount);
    rect(width/2,height/2, width,height);
  }  

  updateAuto();

  for(int i=0; i<num; i++)
    if(particles[i].age>0) particles[i].update();
}

void reinit() {
  doClear=true;
  scMod=random(1,1.4);
  fadeInterval=(int)random(220,300);
  fadeAmount=random(30,60);
//  println("fadeInterval "+fadeInterval+" scMod "+nf(scMod,0,3)+
//    " fadeAmount "+nf(fadeAmount,0,3));
  for(int i=0; i<num; i++) particles[i].age=-1;
  initAuto();
}

void mousePressed() {
  reinit();
}

AutoMover auto[];
int autoN;

void initAuto() {
  autoN=(int)random(3,6);
//  println("initAuto "+autoN);
  auto=new AutoMover[autoN];
  for(int i=0; i<autoN; i++) auto[i]=new AutoMover();

}

void updateAuto() {
  for(int i=0; i<autoN; i++) auto[i].update();
}

class AutoMover {
  Vec2D pos,posOld;
  float dir,dirD,speed,sc,dirCnt;
  int type,age,ageGoal,interval;


  AutoMover() {
    reinit();
  }

  void reinit() {
    ageGoal=(int)random(150,350);
    if(random(100)>95) ageGoal*=1.25;
    age=-(int)random(50,150);    
    pos=new Vec2D(random(width-100)+50,random(height-100)+50);


    dir=(int)random(36)*10;
    type=0;
    if(random(100)>60) type=1;

    interval=(int)random(2,5);    
    if(type==1) {
      interval=1;
      dir=degrees(atan2(-(pos.y-height/2),pos.x-width/2));
    }

    dirD=random(1,2);
    if(random(100)<50) dirD=-dirD;
    speed=random(3,6);

    sc=random(0.5,1);
    if(random(100)>90) sc=random(1.2,1.6);
    dirCnt=random(20,35);


    if(type==0) {
      if(random(100)>95) sc=random(1.5,2.25);
      else sc=random(0.8,1.2);
    }
    sc*=scMod;
    speed*=sc;
  }

  void update() {
    age++;
    if(age<0) return;
    else if(age>ageGoal) reinit();
    else {
      if(type==1) {
        pos.add(
          cos(radians(dir))*speed,sin(radians(dir))*speed);

        dir+=dirD;
        dirD+=random(-0.2,0.2);
        dirCnt--;
        if(dirCnt<0) {
          dirD=random(1,5);
          if(random(100)<50) dirD=-dirD;
          dirCnt=random(20,35);
        }
      }
      if(age%interval==0) newParticle();   
      if(pos.x<-50 || pos.x>width+50 ||
        pos.y<-50 || pos.y>height+50) reinit();
    }
  }

  void newParticle() {
    int partNum,i;

    if(type==0) dir=int(random(36))*10;

    i=0;
    while(i<num) {
      if(particles[i].age<1) {
        float offs=random(30,90);
        if(random(100)>50) offs=-offs;
        particles[i].init(dir+offs,pos.x,pos.y,sc);

        break;
      }
      i++;
    }

    px=mouseX;
    py=mouseY;
  }
}

class Particle {
  Vec2D v,vD;
  float dir,dirMod,speed,sc;
  int col,age,stateCnt;
  int type;

  Particle() {
    v=new Vec2D(0,0);
    vD=new Vec2D(0,0);
    age=0;
  }

  void init(float _dir,float mx,float my,float _sc) {
    dir=_dir;
    sc=_sc;

    float prob=random(100);
    if(prob<80) age=15+int(random(30));
    else if(prob<99) age=45+int(random(50));
    else age=100+int(random(100));

    if(random(100)<80) speed=random(2)+0.5;
    else speed=random(2)+2;

    if(random(100)<80) dirMod=20;
    else dirMod=60;

    v.set(mx,my);
    initMove();
    dir=_dir;
    stateCnt=10;
    if(random(100)>50) col=0;
    else col=1;

    type=(int)random(30000)%2;
  }

  void initMove() {
    if(random(100)>50) dirMod=-dirMod;
    dir+=dirMod;

    vD.set(speed,0);
    vD.rotate(radians(dir+90));

    stateCnt=10+int(random(5));
    if(random(100)>90) stateCnt+=30;
  }

  void update() {
    age--;

    if(age>=30) {
      vD.rotate(radians(1));
      vD.mult(1.01f);
    }

    v.add(vD);
    if(col==0) fill(255-age,0,100,150);
    else fill(100,200-(age/2),255-age,150);

    if(type==1) {
      if(col==0) fill(255-age,100,0,150);
      else fill(255,200-(age/2),0,150);
    }

    pushMatrix();
    scale(sc);
    translate(v.x,v.y);
    rotate(radians(dir));
    rect(0,0,1,16);
    popMatrix();

    if(age==0) {
      if(random(100)>50) fill(200,0,0,200);
      else fill(00,200,255,200);
      float size=2+random(4);
      if(random(100)>95) size+=5;
      size*=sc;
      ellipse(v.x*sc,v.y*sc,size,size);
    }
    if(v.x<0 || v.x>width || v.y<0 || v.y>height) age=0;

    if(age<30) {
      stateCnt--;
      if(stateCnt==0) {
        initMove();
      }
    }
   }

}

// General vector class for 2D vectors
class Vec2D {
  float x,y;

  Vec2D(float _x,float _y) {
    x=_x;
    y=_y;
  }

  Vec2D(Vec2D v) {
    x=v.x;
    y=v.y;
  }

  void set(float _x,float _y) {
    x=_x;
    y=_y;
  }

  void set(Vec2D v) {
    x=v.x;
    y=v.y;
  }

  void add(float _x,float _y) {
    x+=_x;
    y+=_y;
  }

  void add(Vec2D v) {
    x+=v.x;
    y+=v.y;
  }

  void sub(float _x,float _y) {
    x-=_x;
    y-=_y;
  }

  void sub(Vec2D v) {
    x-=v.x;
    y-=v.y;
  }

  void mult(float m) {
    x*=m;
    y*=m;
  }

  void div(float m) {
    x/=m;
    y/=m;
  }

  float length() {
    return sqrt(x*x+y*y);
  }

  float angle() {
    return atan2(y,x);
  }

  void normalise() {
    float l=length();
    if(l!=0) {
      x/=l;
      y/=l;
    }
  }

  Vec2D tangent() {
    return new Vec2D(-y,x);
  }

  void rotate(float val) {
    // Due to float not being precise enough, double is used for the calculations
    double cosval=Math.cos(val);
    double sinval=Math.sin(val);
    double tmpx=x*cosval - y*sinval;
    double tmpy=x*sinval + y*cosval;

    x=(float)tmpx;
    y=(float)tmpy;
  }
}
```
@processing(ABSTRACT01js)


                               --{{4}}--
Für solche JavaScript-Bibliotheken und auch zur Nutzung anderer Funktionalitäten
bieten wir Templates an, die über ein eigenes Macro-System implementiert wurden.
Diese können frei übernommen werden und minimieren auch den Bruch beim lesen des
originalen Markdown-Dokumentes.


## Abschließende Worte

                              --{{0}}--
LiaScript ist eine reine JavaScript-Implementierung, das heißt, der Kurs wird
live im Browser beim Besucher geladen und interpretiert. LiaScript speichert
auch keine Zustände auf der Webseite oder nutzt Cookies.

                              --{{1}}--
Kurse können überall gehostet werden, es wird nur der Verweis auf das jeweilige
Markdown-Dokument benötigt. Hierfür eignet sich GitHub besonders, Kurse können
hier in mehreren Versionen vorliegen und es können auch private Projekte
angelegt werden, die nicht von jedem eingesehen werden können.

                              --{{2}}--
Jeder der einen Kurs erstellt, behält auch all seine Rechte daran und kann seine
Inhalte nach eigenem Belieben und Ermessen verändern und mit anderen Menschen
Teilen.


1. LiaScript ist eine reine clientseitige Implementierung in JavaScript
   ([Elm](https://elm-lang.org))
2. Kurse können überall gehostet werden
   (bevorzugt auf [GitHub](https://github.com))
3. Jeder behält die Rechte an seinem Kurs

## Werkzeuge

                              --{{0}}--
Derzeit existieren zwei Plugins für [Atom](https://atom.io), den freien Editor
von [GitHub](https://github.com). Diese sollen den Einstieg und die Entwicklung
von Kursen in LiaScript vereinfachen.

__[Atom](https://atom.io) Editor-Plugins:__

1. liascript-preview: https://atom.io/packages/liascript-preview
2. liascript-snippets: https://atom.io/packages/liascript-snippets

![Preview](https://raw.githubusercontent.com/andre-dietrich/liascript-preview/master/preview.gif)<!-- width="100%"-->

## Kontakt

| Name           | eMail                                   |
|----------------|-----------------------------------------|
| André Dietrich | andre.dietrich@ovgu.de                  |
| Sebastian Zug  | sebastian.zug@informatik.tu-freiberg.de |
