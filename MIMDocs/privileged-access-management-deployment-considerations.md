---
Titel: Privileged Access Management Aspekte der Bereitstellung
MS.Custom: Na
MS.Reviewer: Na
MS.Suite: Na
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId: dd883415-d39f-4903-8b01-1c8b52e8d447
---
# Überlegungen zur Bereitstellung für Privileged Access Management
Willkommen zu CAPS Abzug Editor!
====

CAPS ist die Verwendung von GitHub Flavored Markdown(GFM) die eines der beliebtesten Markdown-Typ ist. Können erfahren Sie, wie Sie das benutzerdefinierte Dokument mit folgenden Regeln.  

*Anzeigen der [Markdown-Grundlagen](https://help.github.com/articles/markdown-basics/)*<br/>
*Anzeigen der [Github Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/)*<br/>
*Anzeigen der [onlinebeispiels](http://github.github.com/github-flavored-markdown/sample_content.html).*<br/>
  
<br/>
Kurze Anleitung 
---
Wenn Sie Ihre Inhalte Abzug in Großbuchstaben erstellen, konnte Sie Unterstützung von verschiedene Arten erhalten:
- Verwendung `Alt + F1` in Internet Explorer oder `F1` in Chrome zu öffnen `Command Palette`
- Verwendung `Ctrl + space` Fügen Sie an einer beliebigen Stelle zu öffnen.
- Verwenden Sie authoring Tool-Leiste oben auf der-Editor, um Ihre Inhalte zu erstellen.

<br/>
Formatieren des Inhalts
---
- **Absatz**
  Absätze in Abzug sind nur eine oder mehrere Textzeilen aufeinander folgende gefolgt von einer oder mehrere leere Zeilen.
  
  ```
  On July 2, an alien ship entered Earth's orbit and deployed several dozen saucer-shaped "destroyer" spacecraft, each 15 miles (24 km) wide.
  
  On July 3, the Black Knights, a squadron of Marine Corps F/A-18 Hornets, participated 
  ```
  
- **Überschriften**  
  Können Sie entweder Sie Header Dropdown-Liste bereitgestellt, in der authoring-Symbolleiste oder manuell hinzufügen '---' oder ' ===' in der nächsten Zeile unter Ihrem Abschnittstitel. Sie können auch eine Überschrift durch Hinzufügen von erstellen, eine oder mehrere `#` Symbole vor der Überschrift. Die Anzahl der `#` wird Verwendung bestimmen die Größe der Überschrift.

  ```
  # The largest heading (an <h1> tag)
  ## The second largest heading (an <h2> tag)
  …
  ###### The 6th largest heading (an <h6> tag)
  ```

- **Block-Angebote**
  Sie können Block Anführungszeichen durch Angeben einer `>`.
  ```
  In the words of Abraham Lincoln:
  
  > Pardon my French
  > Second line of the quote
  ```

- **Formatieren von text**
  Sie können den Text machen **Fett** oder *Kursiv*
  ```
  **This text will be bold**
  *This text will be italic*  
  ```
  Sie können auch authoring Symbolleiste verwenden, um dies zu erreichen.

- **Listen**
  Sie können eine ungeordnete Liste hinzufügen, indem Sie vor der Listenelemente mit `*` oder `-`
  ```
  * Item
  * Item
  * Item
  
  - Item
  - Item
  - Item  
  ```
  Sie können geordnete Liste hinzufügen, indem Sie Elemente mit einer Reihe vor
  ```
  1. Item 1
  2. Item 2
  3. Item 3  
  ```
  Sie können geschachtelte Listen erstellen, indem Sie Einzüge Listenelemente durch zwei Leerzeichen.
  ```
  1. Item 1
    1. A corollary to the above item.
    2. Yet another point to consider.
  2. Item 2
    * A corollary that does not need to be ordered.
      * This is indented four spaces, because it's two spaces further than the item above.
      * You might want to consider making a new list.
  3. Item 3 
  ```
  Sie können auch authoring Symbolleiste verwenden, um dies zu erreichen.
  
- **Formatieren von Code**
  Dreifache Ticks können ("") zum Formatieren von Text als eigene distinct-Block.
  Sehen Sie sich dieses praktische Anwendung, den ich geschrieben habe:  
  ~~~~
  ```
  x = 0
  x = 2 + 2
  what is x
  ```
  ~~~~

<br/>
Verweis auf Ihre Inhalte hinzufügen
---
- **Einfügen von Links**
  Sie können eine Inline-Verknüpfung erstellen, indem Sie Text des Links in Klammern einschließen `[Link Label]`, und klicken Sie dann den Link in Klammern einschließen `(http://URL)`. 
  [Bing.com](http://www.bing.com)
  
  Sie können auch eine Verknüpfung zu einem vorhandenen Thema in Großbuchstaben mit der authoring Symbolleiste erstellen
  
- **Bild einfügen**
  Sie können ein online-Image aus externen Ressource verweisen, indem `![Image Label]`, und klicken Sie dann die Bild-Url-Ressource in Klammern einschließen `(http://ImageURL)`
  
  Sie können auch ein Bild von CAPS mit der authoring Symbolleiste einfügen


<br/>
Verwenden von Tabellen in Ihrer Inhalte
---
Sie können Tabellen erstellen, durch Zusammenstellen einer Liste von Wörtern und teilen sie mit einem Bindestrich - (für die erste Zeile), und trennen Sie dann jede Spalte mit einer Pipe |:

Erste Kopfzeile  | Zweite Header
------------- | -------------
Inhalt der Zelle  | Inhalt der Zelle
Inhalt der Zelle  | Inhalt der Zelle

Sie können auch Inline Abzug Formatsyntax wie z. B. Links, fett, kursiv, oder durchgestrichen:

| Name | Beschreibung          |
| ------------- | ----------- |
| Hilfe      | ~~Anzeigen der~~ Hilfefenster.|
| Schließen     | _Schließt_ ein Fenster     |

Stehen Ihnen weitere Formatierungsoptionen Steuerelement dazu Doppelpunkte: in der Kopfzeile können Text linksbündig, rechtsbündig oder zentriert ausgerichtet werden:

| Linksbündig  | Zentriert ausgerichtet  | Rechtsbündig |
| :------------ |:---------------:| -----:|
| ist SP 3      | scheuen text | $1600 |
| ist SP 2      | Zentriert        |   $12 |
| Zebra Streifen | praktisch sind        |    $1 |

Ein Doppelpunkt auf die **am weitesten links** Seite gibt eine links ausgerichtete Spalte, einem Doppelpunkt auf die **am weitesten rechts befindlichen** Seite gibt eine rechts ausgerichtete Spalte, einem Doppelpunkt auf **beide** Seiten gibt eine Spalte zentriert ausgerichtet.


<br/>
HTML
---
Sie können eine Teilmenge von HTML in Ihrer Inhalte verwenden. 
Eine vollständige Liste unserer unterstützten Tags und Attribute finden Sie [hier](https://github.com/github/markup/tree/master#html-sanitization)


  
<!--HONumber=Mar16_HO1-->
