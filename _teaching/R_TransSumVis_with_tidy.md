*tidy* `R`: Transformation, Zusammenfassen und Visualisieren von Daten mit tidy
================
Philipp Meyer, Institut für Politikwissenschaft
21.04.2021, Grundlagen, Seminar: Quantitative Textanalyse

-   [1. Einleitung](#einleitung)
-   [2. Installieren von tidyverse](#installieren-von-tidyverse)
-   [3. Grundlagen des Tidyverse](#grundlagen-des-tidyverse)
    -   [3.1. Daten lesen: `read_csv`](#daten-lesen-read_csv)
    -   [3.2. Subsetting mit `filter()`](#subsetting-mit-filter)
    -   [3.3. Nebenbei: Hilfe zu einer (tidy) Funktion erhalten](#nebenbei-hilfe-zu-einer-tidy-funktion-erhalten)
    -   [3.4. Auswählen bestimmter Spalten](#auswählen-bestimmter-spalten)
    -   [3.5. Sortieren mit `arrange()`](#sortieren-mit-arrange)
    -   [3.6. Hinzufügen oder Verändern von Variablen mit `mutate()`](#hinzufügen-oder-verändern-von-variablen-mit-mutate)
    -   [3.7. Arbeiten mit Pipes](#arbeiten-mit-pipes)
-   [4. Aggregation von Daten und Datensätzen](#aggregation-von-daten-und-datensätzen)
    -   [4.1. Einfache Aggregation](#einfache-aggregation)
    -   [4.2. Multiple Aggregation/Zusammenfassen](#multiple-aggregationzusammenfassen)
    -   [4.3. Fehlende Werte](#fehlende-werte)
-   [5. Die Grundlagen der Datenvisualisierung](#die-grundlagen-der-datenvisualisierung)
    -   [5.1. `ggplot2`](#ggplot2)
    -   [5.2. Ein paar Hinweise zur `ggplot`-Syntax](#ein-paar-hinweise-zur-ggplot-syntax)
    -   [5.3. Weitere ästhetische Aspekte](#weitere-ästhetische-aspekte)
    -   [5.4. Balkendiagramme](#balkendiagramme)
    -   [5.5. Graphikoptionen auf der Diagrammebene](#graphikoptionen-auf-der-diagrammebene)
    -   [5.5. Linien-/Kurvendiagramme](#linien-kurvendiagramme)
    -   [5.6. Themes](#themes)
    -   [5.7. Karten!](#karten)

# 1. Einleitung

Das Ziel dieses Tutorials ist euch mit dem *Tidyverse* vertraut zu machen. Außerdem soll das Tutorial euch eine Einführung in die Arbeit mit Daten, deren Verarbeitung und Visualisierung geben.

Das *Tidyverse* ist eine Sammlung von Paketen, die auf der Basis von klar definierten Prinzipien entwickelt wurden. *Tidy* propagiert klare Vorstellungen wie Daten aussehen sollten und wie wir mit ihnen arbeiten sollten.

Das *tidyverse* wird in dem kostenlosen (Online-) Buch *R for Data Science* vorgestellt. Dieses Tutorial behandelt vor allem die Inhalte aus den Kapitel 3, 5 und 7:

-   [Kapitel 3](https://r4ds.had.co.nz/data-visualisation.html)
-   [Kapitel 5](https://r4ds.had.co.nz/transform.html)
-   [Kapitel 7](https://r4ds.had.co.nz/exploratory-data-analysis.html)

In diesem Teil des Tutorials konzentrieren wir uns auf die Arbeit mit dem Paket `tidyverse`. Ein wichtiger Teil ist das Paket `dplyr` (data-pliers), das die meisten der Funktionen enthält, die wir im Folgenden verwenden. Weitere Inhalte von `dplyr` sind Funktionen zum Lesen, Analysieren und Visualisieren von Daten, auf die wir am Ende des Tutorial eingehen werden.

# 2. Installieren von tidyverse

Wie zuvor verwenden wir `install.packages()`, um das Paket herunterzuladen und zu installieren. Anschließend nutzen wir `library()`, damit wir die Funktionen aus dem Paket auch nutzen können.

**Tipp**: schreibt die `library()`-Funktion für die Pakete, die ihr in eurem `R`-Skript verwendet, untereinander an den Anfang eures jeweiligen Skripts. Mit mehr Erfahrungen in `R` werdet ihr Anfangen für jede Aufgabe bzw. jeden Analyseschritt für eine Hausarbeit (z.B. deskriptive Statistik, Visualisierung, multivariate Analyse) ein einzelnes Skript zu verwenden. Das hat seinen Grund darin, dass jeder dieser Schritte unterschiedliche Datenformate, Pakete etc. benötigt. Im Gegensatz zu `install.packages()`, das ich pro Computer und pro Paket nur einmal ausführen müsst, ist die `library()`-Funktion bei jedem Start von `R` notwendig.

``` r
install.packages("tidyverse") # nur einmal notwendig. Beachtet: beim Installieren von Paketen müsst ihr Anführungszeichen verwenden. R ist case-sensitive und die Pakete müssen spezifisch angesprochen werden.
```

``` r
library(tidyverse) # bei jedem Start von R notwendig
```

**Hinweis**: Erschrecket nicht, wenn nach dem Aufruf der *tidyverse* oder jeder anderen Paket-Bibliothek eine rote Meldung zu sehen ist. `RStudio` unterscheidet nicht zwischen Meldungen, Warnungen und Fehlern und zeigt daher alle drei in Rot an. Eine Operation ist dann fehlerhaft, wenn in der Meldung auch wirklich das Wort "Fehler" auftaucht. Das passiert zum Beispiel bei einem falsch geschriebenen Paket (`R` ist case-sensitive und nimmt Groß- und Kleinschreibung sehr, wirklich sehr, ernst.)

``` r
library(tidyvers) 
```

`Fehler in library(tidyvers) : es gibt kein Paket namens ‘tidyvers’`

# 3. Grundlagen des Tidyverse

Die Funktionalität von `R`-Paketen definiert sich durch die Funktionen die sie beinhalten. Umgangssprachlich ausgedrück ist eine Funktion ein Befehl an den Computer, eine Handlung durchzuführen und und das Ergebnis dieser Handlung zu berichten/zurückzugeben. Die Funktionalität von `dyplyr` basiert vor allem auf den Umgang mit Datensätzen, wie zum Beispiel das Filtern und Sortieren von Variablen.

Datensätze bestehen aus Reihen (die Fälle bzw. Beobachtungen wie Länder, ProbantInnen, Parteiprogramme, Gerichtsurteile) und Spalten (die Variablen, wie z.B. welcher Senat des Bundesverfassungsgerichts eine Entscheidung zu verantworten hat oder von welcher Partei ein Programm verfasst wurde und wann). In den ersten Tutorials habt ihr `data.frames` als `R`-Objekte für Datensätze kennengelernt. Das *tidyverse* verwendet einen eigenen Datensatztyp den/das(?) `tibble`.

`tibble` sind funktional äquivalent zu `data.frame`, aber deutlich effizienter und einfacher zu verwenden. Im Beispiel erstellen wir beispielsweise einen Datensatz über Verfassungsgerichte (drei Gerichte, deren rechtlichte Tradition und ob sie die Kompetenz zur abstrakten Normenkontrolle haben):

``` r
courts <- tibble(id=c(1,2,3),
                 gericht=c("Bundesverfassungsgericht","Österreichischer Verfassungsgerichtshof","US Supreme Court"), 
                 rechtstradition=c("civil law","civil law","common law"), 
                 abstrakte_NK=c(1, 1, 0)
                 )
courts
```

    ## # A tibble: 3 x 4
    ##      id gericht                                 rechtstradition abstrakte_NK
    ##   <dbl> <chr>                                   <chr>                  <dbl>
    ## 1     1 Bundesverfassungsgericht                civil law                  1
    ## 2     2 Österreichischer Verfassungsgerichtshof civil law                  1
    ## 3     3 US Supreme Court                        common law                 0

Die Struktur und die Erstellung verläuft analog zur Struktur und Erstellun mit der `data.frame`-Funktion. Wir definieren den Namen das Datensatzes ("courts"), verwenden die `tibble`-Funktion und definieren innerhalb dieser Funktion die einzelnen Variablen (id = Identifikationsnummer, gericht = Namen der drei Gerichte, rechtstradition = die Tradition in welcher das jeweilige Rechtssystem steht, abstrakte\_NK=hat das jeweilige Gerichte die Kompetenz für abstrakte Normenkontrolle). Im weiteren Verlauf des Tutorials gehen wir noch etwas mehr auf die Arbeit mit Datensätzen ein.

### 3.1. Daten lesen: `read_csv`

In dem Beispiel oben haben wir einen Datensatz manuell erstellt. In den allermeisten Fällen werdet ihr aber externe Datensätze nutzen, wie zum Beispiel die Datensätze von [V-Dem](https://www.v-dem.net/en/data/data/) oder vom [Comparatives Agendas Project](https://www.comparativeagendas.net). Solche externen Datensätze sind meistens als csv-Datei erhältlich (natürlich finden sich auch Excel-, SPSS-, Stata-, oder auch `R` eigene RDat-Datensätze, aber csv ist zu bevorzugen und wir werden hier auch vor allem mit csv-Dateien arbeiten).

Tidyverse enthält die Funktion `read_csv`, mit der ihr eine csv-Datei direkt als Datensatz einlesen könnt. Dafür gebt ihr den Speicherort der Datei an, also entweder mittels eures lokalen Datenpfads um auf euer Laufwerk zuzugreifen oder aus dem Internet.

Im folgenden Beipsiel laden wir zwei Datensätze herunter. Zum einen meinen eigenen, den ich für meine Dissertation erstellt habe. Er enthält Senatsentscheidungen des Bundesverfassungsgerichts für die Jahre 2015 - 2020. Er beinhaltet vielfältige Meta-Daten wie den Volltext der Entscheidungen, den Verfahrenstyp etc. Zum anderen laden wir einen Datensatz von [fivethirtyeight](https://fivethirtyeight.com/) herunter. Dieser Datensatz enhält Umfragedaten über die private Nutzung von Feuerwaffen in den USA.

Beide Datensätze stammen aus dem Internet. Aber ihr könnt die URL meines Datensatzes nutzen, um diesen direkt herunterzuladen. Falls ihr das macht, dann versucht doch einmal die Daten von eurem lokalen Laufwerk aus einzulesen.

``` r
# Schritt 1: Lest die URL in R ein (die Internetseiten verweisen direkt auf die hochgeladenen csv.Dateien. Das ist nicht überall der Fall, weshalb der hier präsentierte nur ein möglicher Weg von vielen ist)
url_courts = "https://phimeyer.github.io/teaching/FCC_senate_decisions.csv" 
url_weapons = "https://raw.githubusercontent.com/fivethirtyeight/data/master/poll-quiz-guns/guns-polls.csv"

# Schritt 2: Mittels der Funktion read_csv wird nun der im Web gespeicherte csv.Datensatz eingelesen
court <- read_csv(url_courts)
head(court) # head() zeigt die ersten zehn beobachtungen des datensatzes
```

    ## # A tibble: 6 x 34
    ##      X1 doc_id text  docket_nb short_text date   year pr_nr pr_date pr_text
    ##   <dbl> <chr>  <chr> <chr>     <chr>      <chr> <dbl> <chr> <chr>   <chr>  
    ## 1  5745 20150… "\n … 2 BvE 1/… "Unzulaes… 13.0…  2015 <NA>  <NA>     <NA>  
    ## 2  5748 20150… "\n … 1 BvR 93… "Regelung… 14.0…  2015 13/2… 11.03.… "\n   …
    ## 3  5759 20150… "\n … 1 BvR 47… "Ein paus… 27.0…  2015 14/2… 13.03.… "\n   …
    ## 4  5769 20150… "\n … 1 BvR 47… "Auskunft… 24.0…  2015 16/2… 18.03.… "\n   …
    ## 5  5784 20150… "\n … 2 BvB 1/… "Hinweisb… 19.0…  2015 <NA>  <NA>     <NA>  
    ## 6  5788 20150… "\n … 1 BvR 28… "Untersch… 24.0…  2015 26/2… 30.04.… "\n   …
    ## # … with 24 more variables: pr_dummy <dbl>, pr_dec_diff <dbl>,
    ## #   decision_type <dbl>, second_senate_dummy <dbl>, bvr <dbl>, bvl <dbl>,
    ## #   bvq <dbl>, bvc <dbl>, bve <dbl>, bvf <dbl>, bvb <dbl>, bvg <dbl>,
    ## #   bvk <dbl>, bvh <dbl>, bvm <dbl>, bvn <dbl>, pbvu <dbl>, bvp <dbl>,
    ## #   unanimous <dbl>, sep_op <dbl>, oral <dbl>, status_quo <dbl>,
    ## #   lower_co_uncons <dbl>, deadline <dbl>

``` r
weapons <- read_csv(url_weapons)
tail(weapons) # tail() zeigt die letzten zehn beobachtungen des datensatzes
```

    ## # A tibble: 6 x 9
    ##   Question Start End   Pollster Population Support `Republican Sup…
    ##   <chr>    <chr> <chr> <chr>    <chr>        <dbl>            <dbl>
    ## 1 stricte… 2/18… 2/20… YouGov   Registere…      58               36
    ## 2 stricte… 2/16… 2/19… Quinnip… Registere…      66               34
    ## 3 stricte… 2/22… 2/26… Morning… Registere…      68               53
    ## 4 stricte… 3/3/… 3/5/… Quinnip… Registere…      63               39
    ## 5 stricte… 3/4/… 3/6/… YouGov   Registere…      61               42
    ## 6 stricte… 3/1/… 3/5/… Morning… Registere…      69               57
    ## # … with 2 more variables: `Democratic Support` <dbl>, URL <chr>

(ihr könnt die (roten) Meldungen ignorieren, sie sagen nur, wie die einzelnen Spalten *geparst* wurden)

Wenn ihr "manuell" durch eure Datensätze blättern wollen, dann müsst ihr auf den Namen des Datensatzes im oberen rechten Fenster "Umgebung/Environment" klicken oder aber den Befehl `View(court)` als Code schreiben und ausführen.

### 3.2. Subsetting mit `filter()`

Die Filterfunktion `filter()` kann verwendet werden, um Datensätze zu *subsetten*, also um Teilmengen eines Datensatzes zu erstellen. In den Umfragedaten über die Waffennutzung gibt die Spalte "Question" an, welche Frage gestellt wurde. Wir können jetzt nur die Reihen (die einzelnen Fragen der Umfragen) auswählen, die gefragt haben, ob das Mindestkaufalter für Waffen auf 21 Jahre angehoben werden soll. Für den Gerichtsdatensatz können wir zum Beispiel die Spalte "bvf" auswählen und damit die Gerichtsentscheidungen auswählen, die eine Verfassungsbeschwerde zum Streitgegenstand hatten (dummy variable, 1 für ja und 0 für nein):

``` r
age21 <-  filter(weapons, Question == 'age-21') # "Question" ist die Variable und "age-21" die Ausprägung
age21
```

    ## # A tibble: 7 x 9
    ##   Question Start End   Pollster Population Support `Republican Sup…
    ##   <chr>    <chr> <chr> <chr>    <chr>        <dbl>            <dbl>
    ## 1 age-21   2/20… 2/23… CNN/SSRS Registere…      72               61
    ## 2 age-21   2/27… 2/28… NPR/Ips… Adults          82               72
    ## 3 age-21   3/1/… 3/4/… Rasmuss… Adults          67               59
    ## 4 age-21   2/22… 2/26… Harris … Registere…      84               77
    ## 5 age-21   3/3/… 3/5/… Quinnip… Registere…      78               63
    ## 6 age-21   3/4/… 3/6/… YouGov   Registere…      72               65
    ## 7 age-21   3/1/… 3/5/… Morning… Registere…      76               72
    ## # … with 2 more variables: `Democratic Support` <dbl>, URL <chr>

``` r
verfb <-  filter(court, bvf == 1)
verfb
```

    ## # A tibble: 13 x 34
    ##       X1 doc_id text  docket_nb short_text date   year pr_nr pr_date pr_text
    ##    <dbl> <chr>  <chr> <chr>     <chr>      <chr> <dbl> <chr> <chr>   <chr>  
    ##  1  5893 20150… "\n … 1 BvF 2/… "Keine Ge… 21.0…  2015 57/2… 21.07.… "\n   …
    ##  2  5931 20150… "\n … 2 BvF 1/… "Einstwei… 26.0…  2015 63/2… 1.09.2… "\n   …
    ##  3  6086 20160… "\n … 2 BvF 1/… "Einstwei… 15.0…  2016 63/2… 1.09.2… "\n   …
    ##  4  6294 20160… "\n … 2 BvF 1/… "Einstwei… 20.0…  2016 63/2… 1.09.2… "\n   …
    ##  5  6449 20161… "\n … 2 BvF 1/… "Wiederho… 22.1…  2016 <NA>  <NA>     <NA>  
    ##  6  6613 20170… "\n … 2 BvF 1/… "Wiederho… 13.0…  2017 <NA>  <NA>     <NA>  
    ##  7  6781 20171… "BUN… 2 BvF 1/… "Einstell… 21.1…  2017 <NA>  <NA>     <NA>  
    ##  8  6787 20171… "\n … 2 BvF 1/… "Erneute … 1.12…  2017 <NA>  <NA>     <NA>  
    ##  9  6865 20180… "Lei… 1 BvF 1/… "Verpflic… 21.0…  2018 32/2… 4.05.2… "\n   …
    ## 10  6926 20180… "BUN… 2 BvF 1/… "Erneute … 14.0…  2018 <NA>  <NA>     <NA>  
    ## 11  7018 20180… "Lei… 2 BvF 1/… "Vorschri… 19.0…  2018 74/2… 19.09.… "\n   …
    ## 12  7054 20181… "BUN… 2 BvF 1/… "Einstell… 13.1…  2018 <NA>  <NA>     <NA>  
    ## 13  7764 20201… "BUN… 2 BvF 2/… "Erfolglo… 3.11…  2020 100/… 18.11.… "\n   …
    ## # … with 24 more variables: pr_dummy <dbl>, pr_dec_diff <dbl>,
    ## #   decision_type <dbl>, second_senate_dummy <dbl>, bvr <dbl>, bvl <dbl>,
    ## #   bvq <dbl>, bvc <dbl>, bve <dbl>, bvf <dbl>, bvb <dbl>, bvg <dbl>,
    ## #   bvk <dbl>, bvh <dbl>, bvm <dbl>, bvn <dbl>, pbvu <dbl>, bvp <dbl>,
    ## #   unanimous <dbl>, sep_op <dbl>, oral <dbl>, status_quo <dbl>,
    ## #   lower_co_uncons <dbl>, deadline <dbl>

Das erste Argument sind die zu verwendenden Daten (z.B. weapons), und das/die verbleibende(n) Argument(e) definieren was genau mit den Daten geschehen soll.

Beachtet hier bitte die Verwendung von ==. Andere Möglichkeiten wäre hier &gt; (größer als), &lt;= (kleiner als oder gleich) und != (nicht gleich). Ihr könnt auch mehrere Bedingungen mit logischen (booleschen) Operatoren kombinieren: & (und), I (oder), und ! (nicht), und ihr könnt Klammern verwenden, um `R` zu sagen welche Befehle wann ausgeführt werden sollen (also ähnlich der Funktion von Klammern in der Mathematik).

So können wir alle Umfragen finden, bei denen die Unterstützung für die Anhebung des Waffenalters bei mindestens 80 % lag:

``` r
filter(weapons, Question == 'age-21' & Support >= 80)
```

    ## # A tibble: 2 x 9
    ##   Question Start End   Pollster Population Support `Republican Sup…
    ##   <chr>    <chr> <chr> <chr>    <chr>        <dbl>            <dbl>
    ## 1 age-21   2/27… 2/28… NPR/Ips… Adults          82               72
    ## 2 age-21   2/22… 2/26… Harris … Registere…      84               77
    ## # … with 2 more variables: `Democratic Support` <dbl>, URL <chr>

Beachtet bitte, dass wir bei diesem Befehl kein neues Objekt erzeugt haben. Das Ergebnis wird also direkt in der Console angezeigt, aber nicht gespeichert. So könnt ihr eure Daten schnell inspizieren. Wenn ihr aber diese Datenteilmenge weiter analysieren möchtet, dann müsst ihr sie einem Objekt zuweisen.

### 3.3. Nebenbei: Hilfe zu einer (tidy) Funktion erhalten

Wie bereits erklärt, könnt ihr bei Fragen über eine bestimmte Funktion die `?`-Funktion in der Konsole eingeben (z.B. `?filter`) um die Dokumentation der gesuchten Funktion angezeigt zu bekommen.

Wenn ihr euch die Hilfeseite anseht, dann findet ihr, wie immer, zuerst die allgemeine Beschreibung. Danach folgt die Verwendung, die zeigt, wie die Funktion aufgerufen werden soll. Der Rest gibt zusätzliche Informationen darüber, was genau die Funktion macht, die Ausgabe, die sie erzeugt (Value), und Links zu anderen nützlichen Paketen, Funktionen und schließlich eine Reihe von Beispielen.

Auch wenn es anfangs einschüchternd erscheinen mag, ist es wichtig, sich an den Stil der R-Dokumentation zu gewöhnen. Das ist der erste Anlaufpunkt für Fragen über Funktionen und Pakete!

**Aber vergesst nicht**: ihr könnt für jedes Problem in `R` eine Lösung im Internet finden (z.B. indem ihr einfach die Fehlermeldung die `R` ausgibt kopiert und als Suchanfrage eingebt)!

### 3.4. Auswählen bestimmter Spalten

Wo ihr mit `filter()` bestimmte Reihen auswählen könnt, könnt ihr mit `select()` bestimmte Spalten auswählen. Am einfachsten ist es, die Spalten einfach so zu benennen, dass sie in dieser bestimmten Reihenfolge abgerufen werden:

``` r
select(age21, Population, Support, Pollster)
```

    ## # A tibble: 7 x 3
    ##   Population        Support Pollster          
    ##   <chr>               <dbl> <chr>             
    ## 1 Registered Voters      72 CNN/SSRS          
    ## 2 Adults                 82 NPR/Ipsos         
    ## 3 Adults                 67 Rasmussen         
    ## 4 Registered Voters      84 Harris Interactive
    ## 5 Registered Voters      78 Quinnipiac        
    ## 6 Registered Voters      72 YouGov            
    ## 7 Registered Voters      76 Morning Consult

Ihr könnt auch einen Bereich von Spalten angeben, z. B. alle Spalten von Support bis Democratic Support:

``` r
select(age21, Support:`Democratic Support`)
```

    ## # A tibble: 7 x 3
    ##   Support `Republican Support` `Democratic Support`
    ##     <dbl>                <dbl>                <dbl>
    ## 1      72                   61                   86
    ## 2      82                   72                   92
    ## 3      67                   59                   76
    ## 4      84                   77                   92
    ## 5      78                   63                   93
    ## 6      72                   65                   80
    ## 7      76                   72                   86

Beachtet die Verwendung von 'Backticks' (umgekehrten Anführungszeichen) zur Angabe des Spaltennamens, da `R` normalerweise keine Leerzeichen in Namen zulässt.

`select()` kann auch verwendet werden, um Spalten beim Auswählen umzubenennen, z. B. um die Leerzeichen loszuwerden:

``` r
select(age21, Pollster, rep=`Republican Support`, dem=`Democratic Support`)
```

    ## # A tibble: 7 x 3
    ##   Pollster             rep   dem
    ##   <chr>              <dbl> <dbl>
    ## 1 CNN/SSRS              61    86
    ## 2 NPR/Ipsos             72    92
    ## 3 Rasmussen             59    76
    ## 4 Harris Interactive    77    92
    ## 5 Quinnipiac            63    93
    ## 6 YouGov                65    80
    ## 7 Morning Consult       72    86

Wenn ihr nur Spalten umbenennen möchten, könnt ihr die Funktion `rename()` verwenden:

``` r
rename(age21, start_date = Start, end_date = End) # hier nennen wir die Variablen Start und End in start_date und end_date um
```

    ## # A tibble: 7 x 9
    ##   Question start_date end_date Pollster Population Support `Republican Sup…
    ##   <chr>    <chr>      <chr>    <chr>    <chr>        <dbl>            <dbl>
    ## 1 age-21   2/20/18    2/23/18  CNN/SSRS Registere…      72               61
    ## 2 age-21   2/27/18    2/28/18  NPR/Ips… Adults          82               72
    ## 3 age-21   3/1/18     3/4/18   Rasmuss… Adults          67               59
    ## 4 age-21   2/22/18    2/26/18  Harris … Registere…      84               77
    ## 5 age-21   3/3/18     3/5/18   Quinnip… Registere…      78               63
    ## 6 age-21   3/4/18     3/6/18   YouGov   Registere…      72               65
    ## 7 age-21   3/1/18     3/5/18   Morning… Registere…      76               72
    ## # … with 2 more variables: `Democratic Support` <dbl>, URL <chr>

Natürlich könnt ihr Variablen auch löschen. Dazu müsst ihr Minuszeichen vor die Variablennamen setzen:

``` r
select(age21, -Question, -URL)
```

    ## # A tibble: 7 x 7
    ##   Start  End    Pollster   Population  Support `Republican Sup… `Democratic Sup…
    ##   <chr>  <chr>  <chr>      <chr>         <dbl>            <dbl>            <dbl>
    ## 1 2/20/… 2/23/… CNN/SSRS   Registered…      72               61               86
    ## 2 2/27/… 2/28/… NPR/Ipsos  Adults           82               72               92
    ## 3 3/1/18 3/4/18 Rasmussen  Adults           67               59               76
    ## 4 2/22/… 2/26/… Harris In… Registered…      84               77               92
    ## 5 3/3/18 3/5/18 Quinnipiac Registered…      78               63               93
    ## 6 3/4/18 3/6/18 YouGov     Registered…      72               65               80
    ## 7 3/1/18 3/5/18 Morning C… Registered…      76               72               86

### 3.5. Sortieren mit `arrange()`

Ihr könnt einen Datensatz ganz einfach mit der Funktion `arrange()` sortieren. Dazu gebt ihr zuerst den Datensatz an und dann die Spalte(n), nach denen sortiert werden soll. Um absteigend zu sortieren, müsst ihr ein Minus vor eine Variable setzen. Zum Beispiel wird im Folgenden nach Bevölkerung und dann nach Unterstützung für die Anhebung des Waffenalters (absteigend) sortiert:

``` r
age21 <- arrange(age21, Population, -Support)
age21
```

    ## # A tibble: 7 x 9
    ##   Question Start End   Pollster Population Support `Republican Sup…
    ##   <chr>    <chr> <chr> <chr>    <chr>        <dbl>            <dbl>
    ## 1 age-21   2/27… 2/28… NPR/Ips… Adults          82               72
    ## 2 age-21   3/1/… 3/4/… Rasmuss… Adults          67               59
    ## 3 age-21   2/22… 2/26… Harris … Registere…      84               77
    ## 4 age-21   3/3/… 3/5/… Quinnip… Registere…      78               63
    ## 5 age-21   3/1/… 3/5/… Morning… Registere…      76               72
    ## 6 age-21   2/20… 2/23… CNN/SSRS Registere…      72               61
    ## 7 age-21   3/4/… 3/6/… YouGov   Registere…      72               65
    ## # … with 2 more variables: `Democratic Support` <dbl>, URL <chr>

Beachtet bitte, dass ich das Ergebnis der Sortierung wieder dem `age21`-Objekt zugewiesen habe, d.h. ich ersetze das Objekt durch seine sortierte Version. Wenn ich das Ergebnis nicht zuweisen würde, würde es zwar in der Console angezeigt, aber nicht gespeichert werden. Einem Ergebnis den gleichen Namen zuzuweisen bedeutet, dass ich kein neues Objekt anlege, wodurch die Umgebung nicht unübersichtlich wird (und ich mir die Mühe erspare, mir einen weiteren Objektnamen auszudenken).

Im Falle des neu arrangierens von Daten ist das im Allgemeinen in Ordnung, da die sortierten Daten die gleichen Daten wie zuvor enthalten. Im Falle des Subsetting bedeutet das, dass die Daten tatsächlich aus dem Datensatz (im Speicher) gelöscht werden. In diesem Fall müsstet ihr die Daten erneut einlesen (oder mit einem früheren Objekt beginnen), wenn ihr die "alten" Reihen oder Spalten später benötigen solltet (hier zeigen sich die Vorteile eines `R`-Skripts, in dem ihr, im Gegensatz zur Console, einfach zum vorherigen Schritt scrollen und diesen nochmals ausführen könnt). Ihr werdet merken: `R` ist zu großen Teilen **trial and error**!

### 3.6. Hinzufügen oder Verändern von Variablen mit `mutate()`

Die Funktion `mutate()` macht es einfach, neue Variablen zu erstellen oder bestehende Variablen zu verändern.

Wenn ihr euch die Dokumentationsseite anschaut (`?mutate`), dann seht ihr, dass mutate ähnlich wie `filter()` und `select()` funktioniert. Auch hier (wie *fast* überall, auch außerhalb des tidyverse) ist das erste Argument der Datensatz, gefolgt von einer Anzahl von zusätzlichen Argumenten.

Im folgenden werden wir zuerst einige Variablen erstellen und uns dann die Variablen ansehen (mit Hilfe von `select()`, um den Fokus auf die Änderungen zu legen). Konkret werden wir eine Spalte für die Differenz zwischen den Unterstützungswerten für Republikaner und Demokraten erstellen. Damit haben wir ein Maß, welches die Uneinigkeit der Befragten (entlang von Parteilinien) misst.

``` r
age21 <- mutate(age21, party_diff = abs(`Republican Support` - `Democratic Support`))
select(age21, Question, Pollster, party_diff)
```

    ## # A tibble: 7 x 3
    ##   Question Pollster           party_diff
    ##   <chr>    <chr>                   <dbl>
    ## 1 age-21   NPR/Ipsos                  20
    ## 2 age-21   Rasmussen                  17
    ## 3 age-21   Harris Interactive         15
    ## 4 age-21   Quinnipiac                 30
    ## 5 age-21   Morning Consult            14
    ## 6 age-21   CNN/SSRS                   25
    ## 7 age-21   YouGov                     15

``` r
age21 <- arrange(age21, Population, -Support)

age21
```

    ## # A tibble: 7 x 10
    ##   Question Start End   Pollster Population Support `Republican Sup…
    ##   <chr>    <chr> <chr> <chr>    <chr>        <dbl>            <dbl>
    ## 1 age-21   2/27… 2/28… NPR/Ips… Adults          82               72
    ## 2 age-21   3/1/… 3/4/… Rasmuss… Adults          67               59
    ## 3 age-21   2/22… 2/26… Harris … Registere…      84               77
    ## 4 age-21   3/3/… 3/5/… Quinnip… Registere…      78               63
    ## 5 age-21   3/1/… 3/5/… Morning… Registere…      76               72
    ## 6 age-21   2/20… 2/23… CNN/SSRS Registere…      72               61
    ## 7 age-21   3/4/… 3/6/… YouGov   Registere…      72               65
    ## # … with 3 more variables: `Democratic Support` <dbl>, URL <chr>,
    ## #   party_diff <dbl>

Um eine Variable in derselben Spalte zu transformieren (umzukodieren), könnt ihr einfach einen vorhandenen Namen in `mutate()` verwenden, um ihn zu überschreiben.

### 3.7. Arbeiten mit Pipes

Schaut ihr euch den obigen Code an, dann werdet ihr bemerken, dass das Ergebnis jeder Funktion als Objekt gespeichert wird und dass dieses Objekt als erstes Argument für die nächste Funktion verwendet wird. Die temporären Objekte sind meistens nicht von Interesse, da das eigentliche Ziel, wie in diesem Beispiel die Summentabelle, eine Datensatztransformation, eine neue Variable oder ähnliches ist.

Man kann die einzelnen Schritte als eine Pipeline von Funktionen betrachten, bei der die Ausgabe jeder Funktion die Eingabe für die nächste Funktion ist. Tidyverse bietet hier Möglichkeiten den obigen Code zu vereinfachen. Hierfür nutzt ihr den Pipe-Operator `%>%`.

Jedes mal wenn ihr eine Funktion `f(a, x)` schreibt, könnt ihr das durch ein `%>% f(x)` ersetzen. Wenn ihr dann das Ergebnis von `f(a, x)` für eine zweite Funktion verwenden wollt, dann hängt ihr das einfach an eine Pipeline an: `a %>% f(x) %>% f2(y)`. Das ist äquivalent zu `f2(f(a,x), y)` bzw. `b=f(a,x); f2(b, y)`.

Einfach gesagt nehmen Pipes das Ergebnis einer Funktion und verwenden es als Dateneingabe für eine zweite Funktion. Da alle dplyr-Funktionen als erstes Argument ein Tibble voraussetzen und alle Funktionen ein Tibble als Ergebnis zurückgeben, haben wir die Möglichkeit alle Funktionen miteinander zu verknüpfen.

Zur Verdeutlichung schreiben wir alle Code-Elemente dieses Tutorials nochmals:

``` r
weapons <-  read_csv(url_weapons)
age21 <-  filter(weapons, Question == 'age-21')
age21 <-  mutate(age21, party_diff = abs("Republican Support" - "Democratic Support"))
age21 <-  select(age21, Question, Pollster, party_diff)
arrange(age21, -party_diff)  # Hier haben wir insgesamt vier Mal Werte einem Namen zugewiesen (zwei Überschreibungen von age21), außerdem haben wir vor jede Funktion den Datensatznamen geschrieben (z.B. select(age21,....))
```

Um es noch einmal zusammenzufassen: Die csv wird eingelesen, nach bestimmten Fragen gefiltert, die Differenz der Parteianhänger berechnet, andere Variablen entfernt und sortiert. Diesen ganzen Block können wir als eine einzige Pipeline schreiben:

``` r
age21 <- read_csv(url_weapons) %>% filter(Question == 'age-21') %>% 
  mutate(party_diff = abs("Republican Support" - "Democratic Support")) %>%
  select(Question, Pollster, party_diff) %>% 
  arrange(-party_diff)

age21    # Hier haben wir lediglich einmal einen Namen zugewiesen (age21) und den Datensatznamen sonst nicht mehr verwendet (z.B. select(Question,...))
```

Das Elegante an Pipes ist, dass eindeutig zeigen welche Schritte ihr durchführt. Außerdem müssen nicht viele Zwischenobjekte erstellt werden. Wenn es richtig angewendet wird, stellen Pipes schön abgegrenzte Codestücke dar. Damit können dann bestimmte Teile der Analyse von der Roheingabe bis zu den Ergebnissen durchgeführt werden, einschließlich statistischer Modellierung oder Visualisierung.

Natürlich werdet ihr nicht eure ganzes Skript durch eine einzige Pipe ersetzen. Oft ist es auch hilfreich die Zwischenschritte einzeln auszuführen, um die Zwischenwerte speichern zu können. Zum Beispiel könnt ihr einen Datensatz herunterladen, bereinigen und untergliedern, bevor ihr die gewünschten Analysen durchführt. In diesem Fall wollt ihr wahrscheinlich die Ergebnisse des Bereinigens und Subsettings als einzelne Variable speichern und diese dann in euren Analysen (die ihr als Pipes organisieren könnt) verwenden.

# 4. Aggregation von Daten und Datensätzen

Die bisher in diesem Tutorial verwendeten Funktionen für die Datenaufbereitung haben mit einzelnen Reihen gearbeiten. In den meisten Fällen werdet ihr aber eine Vielzahl von Reihen (Beobachtungen, Fälle) untersuchen und nicht nur einzelne Werte. Das wird als Aggregation (Zusammenfassung) bezeichnet. Im tidyverse nutzen wir hierfür die Funktionen `group_by()`, `summarize()` und/oder `mutate()`.

### 4.1. Einfache Aggregation

Zur Wiederholung und Einübung laden und bearbeiten wir unsere Waffenumfragedaten:

``` r
library(tidyverse)
url_weapons <- "https://raw.githubusercontent.com/fivethirtyeight/data/master/poll-quiz-guns/guns-polls.csv"
weapons <-  read_csv(url_weapons) %>% select(-URL) %>% rename(Rep= "Republican Support", Dem= "Democratic Support") # hier haben wir zusätzlich die URL aus dem Datensatz gelöscht und die Parteinamen verkürzt (Rep, Dem) 
weapons
```

    ## # A tibble: 57 x 8
    ##    Question    Start   End     Pollster       Population     Support   Rep   Dem
    ##    <chr>       <chr>   <chr>   <chr>          <chr>            <dbl> <dbl> <dbl>
    ##  1 age-21      2/20/18 2/23/18 CNN/SSRS       Registered Vo…      72    61    86
    ##  2 age-21      2/27/18 2/28/18 NPR/Ipsos      Adults              82    72    92
    ##  3 age-21      3/1/18  3/4/18  Rasmussen      Adults              67    59    76
    ##  4 age-21      2/22/18 2/26/18 Harris Intera… Registered Vo…      84    77    92
    ##  5 age-21      3/3/18  3/5/18  Quinnipiac     Registered Vo…      78    63    93
    ##  6 age-21      3/4/18  3/6/18  YouGov         Registered Vo…      72    65    80
    ##  7 age-21      3/1/18  3/5/18  Morning Consu… Registered Vo…      76    72    86
    ##  8 arm-teache… 2/23/18 2/25/18 YouGov/Huffpo… Registered Vo…      41    69    20
    ##  9 arm-teache… 2/20/18 2/23/18 CBS News       Adults              44    68    20
    ## 10 arm-teache… 2/27/18 2/28/18 Rasmussen      Adults              43    71    24
    ## # … with 47 more rows

#### Datensatzreihen gruppieren

Jetzt können wir die Funktion `group_by()` verwenden, um z. B. die Daten in Bezug auf die verschiedenen Fragetypen (die Variable "Question") zu gruppieren (als kleine Erinnerung: die Bedeutung von Variablen eines Datensatzes findet ihr in dem zum Datensatz gehörigen Codebuch):

``` r
weapons %>% group_by(Question)
```

    ## # A tibble: 57 x 8
    ## # Groups:   Question [8]
    ##    Question    Start   End     Pollster       Population     Support   Rep   Dem
    ##    <chr>       <chr>   <chr>   <chr>          <chr>            <dbl> <dbl> <dbl>
    ##  1 age-21      2/20/18 2/23/18 CNN/SSRS       Registered Vo…      72    61    86
    ##  2 age-21      2/27/18 2/28/18 NPR/Ipsos      Adults              82    72    92
    ##  3 age-21      3/1/18  3/4/18  Rasmussen      Adults              67    59    76
    ##  4 age-21      2/22/18 2/26/18 Harris Intera… Registered Vo…      84    77    92
    ##  5 age-21      3/3/18  3/5/18  Quinnipiac     Registered Vo…      78    63    93
    ##  6 age-21      3/4/18  3/6/18  YouGov         Registered Vo…      72    65    80
    ##  7 age-21      3/1/18  3/5/18  Morning Consu… Registered Vo…      76    72    86
    ##  8 arm-teache… 2/23/18 2/25/18 YouGov/Huffpo… Registered Vo…      41    69    20
    ##  9 arm-teache… 2/20/18 2/23/18 CBS News       Adults              44    68    20
    ## 10 arm-teache… 2/27/18 2/28/18 Rasmussen      Adults              43    71    24
    ## # … with 47 more rows

``` r
weapons
```

    ## # A tibble: 57 x 8
    ##    Question    Start   End     Pollster       Population     Support   Rep   Dem
    ##    <chr>       <chr>   <chr>   <chr>          <chr>            <dbl> <dbl> <dbl>
    ##  1 age-21      2/20/18 2/23/18 CNN/SSRS       Registered Vo…      72    61    86
    ##  2 age-21      2/27/18 2/28/18 NPR/Ipsos      Adults              82    72    92
    ##  3 age-21      3/1/18  3/4/18  Rasmussen      Adults              67    59    76
    ##  4 age-21      2/22/18 2/26/18 Harris Intera… Registered Vo…      84    77    92
    ##  5 age-21      3/3/18  3/5/18  Quinnipiac     Registered Vo…      78    63    93
    ##  6 age-21      3/4/18  3/6/18  YouGov         Registered Vo…      72    65    80
    ##  7 age-21      3/1/18  3/5/18  Morning Consu… Registered Vo…      76    72    86
    ##  8 arm-teache… 2/23/18 2/25/18 YouGov/Huffpo… Registered Vo…      41    69    20
    ##  9 arm-teache… 2/20/18 2/23/18 CBS News       Adults              44    68    20
    ## 10 arm-teache… 2/27/18 2/28/18 Rasmussen      Adults              43    71    24
    ## # … with 47 more rows

Wie ihr sehen könnt, haben sich die Daten nicht geändert. Wir haben lediglich eine Gruppierung (8 Fragen, 8 Gruppen) vorgenommen.

#### Aggregieren/Zusammenfassen

Um Daten zusammenzufassen, nutzen wir die Funktionen `group_by()` und `summarize()`.

Die Syntax von `summarize()` ähnelt sehr stark der von `mutate()` (schaut euch die Dokumentation der Funktion mit `?summarize` an!): `summarize(column=calculation, ...)`. Der zentrale Unterschied ist, dass wir immer eine Funktion zur Berechnung verwenden müssen und erst dann eine Zusammenfassung durchführen könnnen. Gebräuchliche "Zusammenfassungsfunktionen" sind die Berechnung der Summe (`sum()`), des Mittelwerts (`mean()`) oder der Standardabweichung (`sd()`).

Im Folgenden Beispiel wird die durchschnittliche Unterstützung pro unterschiedliche Frage in den Umfragen (Question) berechnet und nach absteigender Unterstützung sortiert:

``` r
weapons_sum <- weapons %>% group_by(Question) %>% summarize(Support=mean(Support)) %>% arrange(-Support) # die aggregierten Daten werden dem Namen weapons_sum zugeordnet und damit wird ein neuer Datensatz erstellt

weapons_sum
```

    ## # A tibble: 8 x 2
    ##   Question                    Support
    ##   <chr>                         <dbl>
    ## 1 background-checks              87.4
    ## 2 mental-health-own-gun          85.8
    ## 3 age-21                         75.9
    ## 4 ban-high-capacity-magazines    67.3
    ## 5 stricter-gun-laws              66.5
    ## 6 ban-assault-weapons            61.8
    ## 7 arm-teachers                   42  
    ## 8 repeal-2nd-amendment           10

Wie ihr sehen können, ändert `summarize()` die Daten. Die Reihen enstprechen jetzt der der Anzahl der unterschiedlichen Fragen in den Umfragen (8), und die einzigen Spalten, die übrig geblieben sind, sind die Gruppierungsvariablen und die zusammengefassten Werte.

Natürlich können wir auch Zusammenfassungen von mehreren Werten berechnen und auch weiterführende Berechnungen durchführen:

``` r
weapons_sum <- weapons %>% group_by(Question) %>% summarize(n=n(), mean=mean(Support), sd=sd(Support))
weapons_sum
```

    ## # A tibble: 8 x 4
    ##   Question                        n  mean    sd
    ##   <chr>                       <int> <dbl> <dbl>
    ## 1 age-21                          7  75.9  6.01
    ## 2 arm-teachers                    6  42    1.55
    ## 3 background-checks               7  87.4  7.32
    ## 4 ban-assault-weapons            12  61.8  6.44
    ## 5 ban-high-capacity-magazines     7  67.3  3.86
    ## 6 mental-health-own-gun           6  85.8  5.46
    ## 7 repeal-2nd-amendment            1  10   NA   
    ## 8 stricter-gun-laws              11  66.5  5.15

Wie ihr jetzt sehen könnt, hat einer der Werte einen fehlenden Wert (NA) für die Standardabweichung. Warum?

#### Verwenden von `mutate()` mit `group_by()`

Die obigen Beispiele reduzieren die Anzahl der Beobachtungen/Fälle auf die Anzahl der von uns definierten Gruppen (also die unterschiedlichen Fragen). Eine weitere Möglichkeit ist die Verwendung von `mutate()`. Damit werden den Reihen die Zusammenfassungswerte hinzugefügt.

Nehmen wir zum Beispiel an, wir möchten sehen, ob eine bestimmte Frage eine andere gesellschaftliche Unterstützung besitzt als der Durchschnitt. Wir können `group_by()` und dann `mutate()` verwenden und berechnen so die durchschnittliche Unterstützung:

``` r
weapons_avgSup <-  weapons %>% group_by(Question) %>% mutate(avg_support=mean(Support), diff=Support - avg_support) # hier erstellen wir die neue Variable "avg_support", also die durchschnittliche Unterstützung
weapons_avgSup
```

    ## # A tibble: 57 x 10
    ## # Groups:   Question [8]
    ##    Question Start End   Pollster Population Support   Rep   Dem avg_support
    ##    <chr>    <chr> <chr> <chr>    <chr>        <dbl> <dbl> <dbl>       <dbl>
    ##  1 age-21   2/20… 2/23… CNN/SSRS Registere…      72    61    86        75.9
    ##  2 age-21   2/27… 2/28… NPR/Ips… Adults          82    72    92        75.9
    ##  3 age-21   3/1/… 3/4/… Rasmuss… Adults          67    59    76        75.9
    ##  4 age-21   2/22… 2/26… Harris … Registere…      84    77    92        75.9
    ##  5 age-21   3/3/… 3/5/… Quinnip… Registere…      78    63    93        75.9
    ##  6 age-21   3/4/… 3/6/… YouGov   Registere…      72    65    80        75.9
    ##  7 age-21   3/1/… 3/5/… Morning… Registere…      76    72    86        75.9
    ##  8 arm-tea… 2/23… 2/25… YouGov/… Registere…      41    69    20        42  
    ##  9 arm-tea… 2/20… 2/23… CBS News Adults          44    68    20        42  
    ## 10 arm-tea… 2/27… 2/28… Rasmuss… Adults          43    71    24        42  
    ## # … with 47 more rows, and 1 more variable: diff <dbl>

Es wir hier also deutlich, dass `summarize()` den Datensatz auf die Gruppen und Zusammenfassungen reduziert, während `mutate()` eine neue Spalte hinzufügt, die für alle Zeilen innerhalb einer Gruppe identisch ist.

#### Gruppierungen aufheben

Schließlich können wir mit `ungroup()` alle Gruppierungen wieder aufheben.

Die im letzten Beispiel erzeugten Daten sind z. B. immer noch nach Fragen (Question) gruppiert. Wenn wir also die Gesamtstandardabweichung der Differenz berechnen möchten, können wir die Gruppierung aufheben und dann zusammenfassen:

``` r
weapons_avgSup %>% ungroup() %>% summarize(diff=sd(diff))
```

    ## # A tibble: 1 x 1
    ##    diff
    ##   <dbl>
    ## 1  5.19

(Natürlich würde die Ausführung von `sd(weapons_avgSup$diff))` das gleiche Ergebnis liefern).

Wie (und warum) würde das Ergebnis aussehen, wenn wir die Gruppierung **nicht** aufheben würden?

### 4.2. Multiple Aggregation/Zusammenfassen

Die obigen Beispiele verwendeten alle eine einzelne Gruppierungsvariable. Natürlich können wir auch nach mehreren Spalten gruppieren. Zum Beispiel könnten wir die durchschnittliche Unterstützung pro Frage und pro Population berechnen:

``` r
weapons %>% group_by(Question, Population) %>% summarize(Support=mean(Support))
```

    ## # A tibble: 15 x 3
    ## # Groups:   Question [8]
    ##    Question                    Population        Support
    ##    <chr>                       <chr>               <dbl>
    ##  1 age-21                      Adults               74.5
    ##  2 age-21                      Registered Voters    76.4
    ##  3 arm-teachers                Adults               42.7
    ##  4 arm-teachers                Registered Voters    41.3
    ##  5 background-checks           Adults               84.5
    ##  6 background-checks           Registered Voters    88.6
    ##  7 ban-assault-weapons         Adults               62.5
    ##  8 ban-assault-weapons         Registered Voters    61.6
    ##  9 ban-high-capacity-magazines Adults               73  
    ## 10 ban-high-capacity-magazines Registered Voters    66.3
    ## 11 mental-health-own-gun       Adults               92  
    ## 12 mental-health-own-gun       Registered Voters    84.6
    ## 13 repeal-2nd-amendment        Registered Voters    10  
    ## 14 stricter-gun-laws           Adults               70  
    ## 15 stricter-gun-laws           Registered Voters    65.7

``` r
weapons
```

    ## # A tibble: 57 x 8
    ##    Question    Start   End     Pollster       Population     Support   Rep   Dem
    ##    <chr>       <chr>   <chr>   <chr>          <chr>            <dbl> <dbl> <dbl>
    ##  1 age-21      2/20/18 2/23/18 CNN/SSRS       Registered Vo…      72    61    86
    ##  2 age-21      2/27/18 2/28/18 NPR/Ipsos      Adults              82    72    92
    ##  3 age-21      3/1/18  3/4/18  Rasmussen      Adults              67    59    76
    ##  4 age-21      2/22/18 2/26/18 Harris Intera… Registered Vo…      84    77    92
    ##  5 age-21      3/3/18  3/5/18  Quinnipiac     Registered Vo…      78    63    93
    ##  6 age-21      3/4/18  3/6/18  YouGov         Registered Vo…      72    65    80
    ##  7 age-21      3/1/18  3/5/18  Morning Consu… Registered Vo…      76    72    86
    ##  8 arm-teache… 2/23/18 2/25/18 YouGov/Huffpo… Registered Vo…      41    69    20
    ##  9 arm-teache… 2/20/18 2/23/18 CBS News       Adults              44    68    20
    ## 10 arm-teache… 2/27/18 2/28/18 Rasmussen      Adults              43    71    24
    ## # … with 47 more rows

Das Ergebnis ist ein Datensatz mit einer Zeile pro Gruppe, d. h. einer Kombination aus `Question` und `Population`, mit separaten Spalten für jede Gruppierung und Aggregation.

Der resultierende Datensatz ist lediglich nach `Question` gruppiert. Die Gruppen nach der Zusammenfassung intakt zu lassen, wäre nicht sinnvoll, da ihr niemals eine Zusammenfassung derselben Gruppen berechnen werdet: Jede der alten Gruppen ist jetzt eine einzelne Reihe.

Während also `mutate()` die Gruppierungsinformationen beibehält, wird bei `summarize()` die eine Gruppierungsspalte, in diesem Fall `Population`, gelöscht.

Dadurch könnt ihr die durchschnittliche Unterstützung pro Frage berechnen: (d. h. der Mittelwert der Zusammenfassungen pro Population):

``` r
weapons %>% group_by(Question, Population) %>% summarize(Support=mean(Support)) %>% mutate(avg_support=mean(Support))
```

    ## # A tibble: 15 x 4
    ## # Groups:   Question [8]
    ##    Question                    Population        Support avg_support
    ##    <chr>                       <chr>               <dbl>       <dbl>
    ##  1 age-21                      Adults               74.5        75.4
    ##  2 age-21                      Registered Voters    76.4        75.4
    ##  3 arm-teachers                Adults               42.7        42  
    ##  4 arm-teachers                Registered Voters    41.3        42  
    ##  5 background-checks           Adults               84.5        86.6
    ##  6 background-checks           Registered Voters    88.6        86.6
    ##  7 ban-assault-weapons         Adults               62.5        62.0
    ##  8 ban-assault-weapons         Registered Voters    61.6        62.0
    ##  9 ban-high-capacity-magazines Adults               73          69.7
    ## 10 ban-high-capacity-magazines Registered Voters    66.3        69.7
    ## 11 mental-health-own-gun       Adults               92          88.3
    ## 12 mental-health-own-gun       Registered Voters    84.6        88.3
    ## 13 repeal-2nd-amendment        Registered Voters    10          10  
    ## 14 stricter-gun-laws           Adults               70          67.8
    ## 15 stricter-gun-laws           Registered Voters    65.7        67.8

``` r
weapons
```

    ## # A tibble: 57 x 8
    ##    Question    Start   End     Pollster       Population     Support   Rep   Dem
    ##    <chr>       <chr>   <chr>   <chr>          <chr>            <dbl> <dbl> <dbl>
    ##  1 age-21      2/20/18 2/23/18 CNN/SSRS       Registered Vo…      72    61    86
    ##  2 age-21      2/27/18 2/28/18 NPR/Ipsos      Adults              82    72    92
    ##  3 age-21      3/1/18  3/4/18  Rasmussen      Adults              67    59    76
    ##  4 age-21      2/22/18 2/26/18 Harris Intera… Registered Vo…      84    77    92
    ##  5 age-21      3/3/18  3/5/18  Quinnipiac     Registered Vo…      78    63    93
    ##  6 age-21      3/4/18  3/6/18  YouGov         Registered Vo…      72    65    80
    ##  7 age-21      3/1/18  3/5/18  Morning Consu… Registered Vo…      76    72    86
    ##  8 arm-teache… 2/23/18 2/25/18 YouGov/Huffpo… Registered Vo…      41    69    20
    ##  9 arm-teache… 2/20/18 2/23/18 CBS News       Adults              44    68    20
    ## 10 arm-teache… 2/27/18 2/28/18 Rasmussen      Adults              43    71    24
    ## # … with 47 more rows

Gibt es eine Möglichkeit, auch den Mittelwert der einzelnen Umfragen zu addieren?

### 4.3. Fehlende Werte

Zusammenfassungsfunktionen in `R` geben standardmäßig NA zurück, wenn einer der zusammenzufassenden Werte fehlt:

``` r
mean(c(3,4,NA,6))
```

    ## [1] NA

Wenn ihr also Daten zusammenfasst, bei denen Reihen fehlendene Werte enthalten, wird der zusammengefasste Wert auf NA gesetzt.

Im folgenden Code erstellen wir mittels `ifelse()` einen NA-Wert für unsere Waffenumfragedaten: Wir setzen den Unterstützungwert für alle CBS News-Umfragen auf NA:

(Hinweis: `ifelse()` nimmt 3 Werte an: `ifelse(test, value-if-true, value-if-false)`, wodurch jede Zeile entsprechend dem Test gesetzt wird. In diesem Fall wird getestet, ob `Pollster` gleich `CBS` ist, und wenn dies der Fall ist, wird `Support` auf `NA` gesetzt, andernfalls wird `support` auf `support` gesetzt (d. h. es bleit unverändert))

``` r
weapons2 <-  weapons %>% mutate(Support=ifelse(Pollster == "CBS News", NA, Support))
weapons2
```

    ## # A tibble: 57 x 8
    ##    Question    Start   End     Pollster       Population     Support   Rep   Dem
    ##    <chr>       <chr>   <chr>   <chr>          <chr>            <dbl> <dbl> <dbl>
    ##  1 age-21      2/20/18 2/23/18 CNN/SSRS       Registered Vo…      72    61    86
    ##  2 age-21      2/27/18 2/28/18 NPR/Ipsos      Adults              82    72    92
    ##  3 age-21      3/1/18  3/4/18  Rasmussen      Adults              67    59    76
    ##  4 age-21      2/22/18 2/26/18 Harris Intera… Registered Vo…      84    77    92
    ##  5 age-21      3/3/18  3/5/18  Quinnipiac     Registered Vo…      78    63    93
    ##  6 age-21      3/4/18  3/6/18  YouGov         Registered Vo…      72    65    80
    ##  7 age-21      3/1/18  3/5/18  Morning Consu… Registered Vo…      76    72    86
    ##  8 arm-teache… 2/23/18 2/25/18 YouGov/Huffpo… Registered Vo…      41    69    20
    ##  9 arm-teache… 2/20/18 2/23/18 CBS News       Adults              NA    68    20
    ## 10 arm-teache… 2/27/18 2/28/18 Rasmussen      Adults              43    71    24
    ## # … with 47 more rows

Wenn wir jetzt den Mittelwert für Support pro Frage nehmen, ergibt das ein `NA` für alle Fragen, bei denen CBS Teil des Sets war:

``` r
weapons2 %>% group_by(Question) %>% summarize(Support=mean(Support))
```

    ## # A tibble: 8 x 2
    ##   Question                    Support
    ##   <chr>                         <dbl>
    ## 1 age-21                         75.9
    ## 2 arm-teachers                   NA  
    ## 3 background-checks              NA  
    ## 4 ban-assault-weapons            NA  
    ## 5 ban-high-capacity-magazines    67.3
    ## 6 mental-health-own-gun          85.8
    ## 7 repeal-2nd-amendment           10  
    ## 8 stricter-gun-laws              NA

``` r
weapons2
```

    ## # A tibble: 57 x 8
    ##    Question    Start   End     Pollster       Population     Support   Rep   Dem
    ##    <chr>       <chr>   <chr>   <chr>          <chr>            <dbl> <dbl> <dbl>
    ##  1 age-21      2/20/18 2/23/18 CNN/SSRS       Registered Vo…      72    61    86
    ##  2 age-21      2/27/18 2/28/18 NPR/Ipsos      Adults              82    72    92
    ##  3 age-21      3/1/18  3/4/18  Rasmussen      Adults              67    59    76
    ##  4 age-21      2/22/18 2/26/18 Harris Intera… Registered Vo…      84    77    92
    ##  5 age-21      3/3/18  3/5/18  Quinnipiac     Registered Vo…      78    63    93
    ##  6 age-21      3/4/18  3/6/18  YouGov         Registered Vo…      72    65    80
    ##  7 age-21      3/1/18  3/5/18  Morning Consu… Registered Vo…      76    72    86
    ##  8 arm-teache… 2/23/18 2/25/18 YouGov/Huffpo… Registered Vo…      41    69    20
    ##  9 arm-teache… 2/20/18 2/23/18 CBS News       Adults              NA    68    20
    ## 10 arm-teache… 2/27/18 2/28/18 Rasmussen      Adults              43    71    24
    ## # … with 47 more rows

Es ist zwar der mathemtische korrekte Weg fehlende Werte zu behandeln, aber meistens wollen wir das einfach ignorieren. Um das zu bewerkstelligen fügen wir `na.rm=T` (T steht für TRUE) zur Mittelwertfunktion hinzu:

``` r
weapons2 %>% group_by(Question) %>% summarize(Support=mean(Support,  na.rm=T))
```

    ## # A tibble: 8 x 2
    ##   Question                    Support
    ##   <chr>                         <dbl>
    ## 1 age-21                         75.9
    ## 2 arm-teachers                   41.6
    ## 3 background-checks              89.5
    ## 4 ban-assault-weapons            62.5
    ## 5 ban-high-capacity-magazines    67.3
    ## 6 mental-health-own-gun          85.8
    ## 7 repeal-2nd-amendment           10  
    ## 8 stricter-gun-laws              66.6

``` r
weapons2
```

    ## # A tibble: 57 x 8
    ##    Question    Start   End     Pollster       Population     Support   Rep   Dem
    ##    <chr>       <chr>   <chr>   <chr>          <chr>            <dbl> <dbl> <dbl>
    ##  1 age-21      2/20/18 2/23/18 CNN/SSRS       Registered Vo…      72    61    86
    ##  2 age-21      2/27/18 2/28/18 NPR/Ipsos      Adults              82    72    92
    ##  3 age-21      3/1/18  3/4/18  Rasmussen      Adults              67    59    76
    ##  4 age-21      2/22/18 2/26/18 Harris Intera… Registered Vo…      84    77    92
    ##  5 age-21      3/3/18  3/5/18  Quinnipiac     Registered Vo…      78    63    93
    ##  6 age-21      3/4/18  3/6/18  YouGov         Registered Vo…      72    65    80
    ##  7 age-21      3/1/18  3/5/18  Morning Consu… Registered Vo…      76    72    86
    ##  8 arm-teache… 2/23/18 2/25/18 YouGov/Huffpo… Registered Vo…      41    69    20
    ##  9 arm-teache… 2/20/18 2/23/18 CBS News       Adults              NA    68    20
    ## 10 arm-teache… 2/27/18 2/28/18 Rasmussen      Adults              43    71    24
    ## # … with 47 more rows

# 5. Die Grundlagen der Datenvisualisierung

In diesem fünften Teil dieses Tutorials arbeiten wir mit dem Paket `ggplot2`. Hier noch ein paar generelle Hinweise zu diesem Paket:

-   Ihr findet ein paar sehr schöne Visualisierungsbeispiele mit `ggplot2` in der [R Graph Gallery](https://www.r-graph-gallery.com/ggplot2-package.html). Dort findet ihr auch den zugehörigen Code. Damit könnt ihr dann die Beispiele mit euren eigenen Daten nachbauen. Keinen Code, aber etwas Inspiration für eure eigenen Visualisierungen findet ihr [hier](https://fivethirtyeight.com/features/the-52-best-and-weirdest-charts-we-made-in-2016/). Abschließen noch der Hinweis auf einen interessanten Artikel der `ggplot2` Entwickler über die ["Grammatik von Graphiken"](http://vita.had.co.nz/papers/layered-grammar.html).

### 5.1. `ggplot2`

Nehmen wir an, dass wir die Beziehung zwischen Hochschulbildung und Haushaltseinkommen für die US Staaten sehen wollen. Die dafür notwendigen Daten beziehen wir von der Github-Seite des ["Houston Data Visualization Meetup"](https://github.com/houstondatavis/data-jam-august-2016), die einen Datensatz namens "**country\_facts**" veröffentlicht haben.

Der Datensatz ist sehr groß, weshalb wir für unser Beispiel mit einem subset arbeiten werden (ich würde mich freuen, wenn ihr privat mit dem gesamten Datensatz experimentieren würdet):

``` r
install.packages("ggplot2") # beim ersten mal nicht vergessen!
```

``` r
csv_folder_url <- "https://raw.githubusercontent.com/houstondatavis/data-jam-august-2016/master/csv" # Im Github-Unterordner "csv" finden sich die verschiedenen Datensätze

facts <-  read_csv(paste(csv_folder_url, "county_facts.csv", sep = "/"))  # read_csv ist eine Funktion im tidyverse-Paket. Das Argument sep = "/" gibt R den Befehl einen Backslah zwischen der in  "csv_folder_url" gespeicherten URL und dem gesuchten Datensatz mit dem Namen "county_facts.csv" zu setzen. 

facts_subset <- facts %>% select(fips, area_name, state_abbreviation, population=Pop_2014_count, pop_change=Pop_change_pct, 
                                over65=Age_over_65_pct, female=Sex_female_pct, anglo_americans=Race_white_pct, 
                                college=Pop_college_grad_pct, income=Income_per_capita) # hier nehmen wir den Datensatz und wählen nur ein paar Variablen aus und benennen einige Variablen um (z.B. die Variable "Pop_college_grad_pct" heißt in unserem facts_subset-Datensatz jetzt "college")

facts_state <- facts_subset %>% filter(is.na(state_abbreviation) & fips != 0) %>% select(-state_abbreviation)
facts_state
```

    ## # A tibble: 51 x 9
    ##     fips area_name population pop_change over65 female anglo_americans college
    ##    <dbl> <chr>          <dbl>      <dbl>  <dbl>  <dbl>           <dbl>   <dbl>
    ##  1  1000 Alabama      4849377        1.4   15.3   51.5            69.7    22.6
    ##  2  2000 Alaska        736732        3.7    9.4   47.4            66.9    27.5
    ##  3  4000 Arizona      6731484        5.3   15.9   50.3            83.7    26.9
    ##  4  5000 Arkansas     2966369        1.7   15.7   50.9            79.7    20.1
    ##  5  6000 Californ…   38802500        4.2   12.9   50.3            73.2    30.7
    ##  6  8000 Colorado     5355866        6.5   12.7   49.8            87.7    37  
    ##  7  9000 Connecti…    3596677        0.6   15.5   51.2            81.2    36.5
    ##  8 10000 Delaware      935614        4.2   16.4   51.6            70.8    28.9
    ##  9 11000 District…     658893        9.5   11.3   52.6            43.6    52.4
    ## 10 12000 Florida     19893297        5.8   19.1   51.1            77.8    26.4
    ## # … with 41 more rows, and 1 more variable: income <dbl>

Gut! Jetzt haben wir einen Datensatz. Machen wir uns jetzt daran eine der am meisten genutzten Visualisierungen überhaupt zu gestalten: ein Steudiagramm! Für selbiges soll der Prozentsatz der HochschulabsolventInnen auf der x-Achse und das Medianeinkommen auf der y-Achse dargestellt werden.

``` r
library(ggplot2) # bei jedem Durchlauf eures Skripts (im Gegensatz zu install.packages())
ggplot(data=facts_state) + geom_point(mapping=aes(x=college, y=income))
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-31-1.png)

Wie ihr seht besteht der Befehl aus zwei Teilen. Im ersten Schritt erstellt `ggplot` eine *leere Leinwand*, die mit dem Datensatz `fact_state` verknüpft ist. Im zweiten Schritt nutzen wir `geom_point` um die uns interessierenden Informationen (Datensatzvariablen) auf diese leere Leinwand zu projezieren. Ihr könnt euch die `ggplot`-Befehle als Schichten vorstellen: 1) die "Leinwand", verknüpft mit dem Datensatz; 2) die geometische Form, hier Punkte (`geom_**point**`); 3) Aspekte um die Datenpunkte abzubilden (hier x=college und y=income).

Unser Beispiel ist die einfachste Form einer Visualisierung. Das `gglot`-Paket hat unendlich viele Möglichkeiten, die ihr mit der Zeit und mit euren eigenen Daten und Ideen selber herausfinden werdet.

Unsere Beispiel-Graphik ist ein Streudiagramm, in dem jeder Punkt einen US Staat repräsentiert. Zusätzlich sehen wir eine klare Korrelation zwischen Bildungsniveau und Einkommen. Es gibt aber auch einen klaren Ausreißer oben rechts. Welcher Staat ist das?

Bevor ihr weiterlest, versucht euch doch mal darin, die Datenpunkte mit Lables zu versehen, um diese Frage zu beantworten.

### 5.2. Ein paar Hinweise zur `ggplot`-Syntax

Damit der Plot funktioniert, muss `R` den gesamten `ggplot`-Aufruf und alle Ebenen als eine einzige Anweisung ausführen. Praktisch bedeutet das, dass, wenn ihr einen Plot über mehrere Zeilen kombiniert, das Pluszeichen am Ende der Zeile stehen **muss**. Erst dann weiß `R`, dass noch mehr kommt. Aber die Positionierung des + ist ebenso wichtig:

**Richtig**:

``` r
ggplot(data=facts_state) + 
  geom_point(mapping=aes(x=college, y=income))
```

**Falsch**:

``` r
ggplot(data=facts_state) 
  + geom_point(mapping=aes(x=college, y=income))
```

Da die Argumente `data` und `mapping` die ersten Argumente sind, die die `ggplot`-Funktionen erwarten (siehe `?gglot` und `?geom_point`), müsst ihr diese nicht extra schreiben:

``` r
ggplot(facts_state) + 
  geom_point(aes(x=college, y=income))
```

### 5.3. Weitere ästhetische Aspekte

Um herauszufinden, welche visuellen Elemente verwendet werden können, lest euch die Dokumentation der Funktionen durch (z. B. `?geom_point`). Hier seht ihr, dass wir unter anderem die Farbe, die Transparenz (alpha) und die Größe der Datenpunkte nach unseren Wünschen verändern können. Stellen wir zunächst die Größe der Punkte auf die Bevölkerung jedes Staates ein:

``` r
ggplot(data=facts_state) + geom_point(mapping=aes(x=college, y=income, size=population))
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-35-1.png)

Da es schwierig ist, überlappende Punkte zu sehen, wollen wir alle Punkte etwas transparenter machen. Hinweis: Da wir das Alpha aller Punkte auf einen einzigen Wert setzen wollen, ist dies kein Mapping (da es nicht auf eine Spalte aus dem Datenrahmen abgebildet wird), sondern eine Konstante. Diese wird deshlab außerhalb des Mapping-Arguments gesetzt:

``` r
ggplot(data=facts_state) + geom_point(mapping=aes(x=college, y=income, size=population), alpha=.5, colour="red")
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-36-1.png)

Anstatt die Farbe auf einen konstanten Wert zu setzen, können wir sie auch mit den Daten variieren lassen. Zum Beispiel können wir die Staaten nach dem Prozentsatz der Bevölkerung über 65 einfärben:

``` r
ggplot(data=facts_state) + geom_point(mapping=aes(x=college, y=income, size=population, colour=over65), alpha=.9)
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-37-1.png)

Schließlich können wir auch eine kategoriale Variable abbilden. Hierfür müssen wir zuerst die Kategorien bilden, z.B. ob die Bevölkerungen in einem Staat wächst (mindestens um 1%) oder stabil bleibt. Um diese Kategorie zu erstellen, benutzen wir die Funktion `if_else()`, die den Wert "if true" zuweist, wenn die definierte Bedingung wahr ist (anderenfalls ist es falsch).

``` r
facts_state <- facts_state %>% mutate(growth=ifelse(pop_change > 1, "Growing", "Stable")) # wenn der wert "pop_change"" über 1 weis der neuen Variable "growth" den Wert "Growing" zu, wenn nicht dann "Stable".
```

Jetzt können wir eine "Kategorien-Farbe" zu der Graphik hinzufügen:

``` r
ggplot(data=facts_state) + geom_point(mapping=aes(x=college, y=income, size=population, colour=growth), alpha=.9) 
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-39-1.png)

Wie ihr in diesen Beispielen sehen können, versucht `ggplot`, die von euch verlangte Zuordnung intelligent zu gestalten. Es setzt die x- und y-Bereiche automatisch auf die Werte der verwendeten Daten. Für die Farbe z.B. erstellt `ggplot` bei Intervallvariablen eine Farbskala, während es bei einer kategorialen Variable jeder Gruppe automatisch eine Farbe zuordnete.

Natürlich kann jede dieser Möglichkeiten angepasst werden. Das ist auch sinnvoll. Zum Beispiel könnte man Rot für Republikaner und Blau für Demokraten verwenden. Andererseits verlangen Publikationen in Fachzeitschriften meistens schlichte Grafiken in schwarz-weiß bzw. mit verschiedenen Grau- oder Schwarztönen. Am besten ihr sucht im Internet mal eigenständig nach Beispielen für andere Farbkonfigurationen für `ggplot`-Graphen und experimentiert etwas rum.

### 5.4. Balkendiagramme

Eine weitere Standardvisualisierung ist das Balkendiagramm. `R` geht bei Balkendiagrammen generell davon aus, dass ein Histogramm dargestellt werden soll. Zum Beispiel können wir darstellen ob die Bevölkerung in den US Staaten wächst oder stabil bleibt:

``` r
ggplot(data=facts_state) + geom_bar(mapping=aes(x=growth)) # wir nehmen die von uns oben erstelle kategoriale variable "growth"
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-40-1.png)

Zugegeben, diese Darstellung ist leidlich interessant. Um das zu ändern laden wir jetzt einen weiteren Datensatz von der Github-Seite des ["Houston Data Visualization Meetup"](https://github.com/houstondatavis/data-jam-august-2016) und visualisieren die Stimmen pro republikanischem Kandidaten in der Vorwahl in New Hampshire. Zuerst laden wir die Daten pro Bezirk herunter, fassen diese auf Ebene der Staaten zusammen und filtern die Ergebnisse für die repubikanische Partei in New Hampshire heraus.

``` r
results <-  read_csv(paste(csv_folder_url, "primary_results.csv", sep = "/")) # der Datensatz auf der Github-Seite hat den Namen "primary_results"
results_state <-  results %>% group_by(state, party, candidate) %>% summarize(votes=sum(votes))
nh_gop <-  results_state %>% filter(state == "New Hampshire" & party == "Republican")
nh_gop
```

    ## # A tibble: 8 x 4
    ## # Groups:   state, party [1]
    ##   state         party      candidate       votes
    ##   <chr>         <chr>      <chr>           <dbl>
    ## 1 New Hampshire Republican Ben Carson       6509
    ## 2 New Hampshire Republican Carly Fiorina   11706
    ## 3 New Hampshire Republican Chris Christie  21069
    ## 4 New Hampshire Republican Donald Trump   100406
    ## 5 New Hampshire Republican Jeb Bush        31310
    ## 6 New Hampshire Republican John Kasich     44909
    ## 7 New Hampshire Republican Marco Rubio     30032
    ## 8 New Hampshire Republican Ted Cruz        33189

Jetzt erstellen wir ein Balkendiagramm mit Stimmen (y) pro Kandidat (x). Da wir nicht wollen, dass `ggplot` die Daten für uns zusammenfasst (das haben wir bereits selbst getan), definieren wir `stat="identity"`. So setzen wir die Gruppierungsstatistik auf die "Identitätsfunktion"", d.h. `ggplot` soll jeden Datenpunkt so verwenden, wie es ihn vorfindet, also keine Aggregation/Transformation vornehmen.

``` r
ggplot(data=nh_gop) + geom_bar(mapping=aes(x=candidate, y=votes), stat='identity')
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-42-1.png)

### 5.5. Graphikoptionen auf der Diagrammebene

Einige Optionen, wie Beschriftungen, Legenden und das Koordinatensystem, gelten für das gesamte Diagramm und nicht pro ästhetischer Ebene (siehe die Beschreibungen in Punkt 5.1.). Diese Optionen werden dem Diagramm direkt hinzugefügt, indem dem zusätzliche Funktionen hinzugefügt werden. Zum Beispiel können wir `coord_flip()` verwenden, um die x- und y-Achse zu vertauschen:

``` r
ggplot(data=nh_gop) + 
  geom_bar(mapping=aes(x=candidate, y=votes), stat='identity') + 
  coord_flip()
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-43-1.png)

Die `reorder()`-Funktion kann vorhandene Kategorien umsortieren, zum Beispiel nach der Anzahl der erhaltenen Stimmen. Natürlich können wir dann auch etwas Farbe hinzufügen:

``` r
ggplot(data=nh_gop) + 
  geom_bar(mapping=aes(x=reorder(candidate, votes), y=votes, fill=candidate), stat='identity') + 
  coord_flip()
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-44-1.png)

Jetzt sollten wir noch die Beschriftung der Achses verändern und die in diesem Fall unnötige Legende entfernen. Außerdem sollte eine Graphik immer eine Überschrift haben! Zu guter letzt sind WissenschaftlerInnen meistens Freunde von minimalistischen Darstellungen. Das lässt sich mit Grautönen und der Änderung des "Themes" bewerkstelligen:

``` r
p <- ggplot(data=nh_gop) + 
  geom_bar(mapping=aes(x=reorder(candidate, votes), y=votes, fill=candidate), stat='identity') +  
  coord_flip() + 
  xlab("Candidate") + 
  xlab("Votes") + 
  ggtitle("New Hampshire: Votes per Candidate in the primaries") +
  guides(fill=F) +  
  scale_fill_grey() + 
  theme_minimal() # hier wählen wir das gewünschte minimalistische theme aus
p
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-45-1.png)

(Beachtet: Wir geben einer Graphik zum ersten Mal einen Namen (p)! Damit lassen sich die Graphiken nicht nur im Environment (oben links) speichern, sondern der Code lässt sich auch sehr gut und übersichtlich darstellen.)

Wir können auch Gruppen zu Balkendiagrammen hinzufügen. Zum Beispiel können wir uns die Staaten anschauen (der Lesbarkeit wegen begrenzt auf New Hampshire und Iowa) und dann nach Kandidaten gruppieren:

``` r
gop2 <-  results_state %>% filter(party == "Republican" & (state == "New Hampshire" | state == "Iowa")) # Neuer Datensatz mit Daten nur für Iowa und New Hampshire

ggplot(data=gop2) + geom_bar(mapping=aes(x=state, y=votes, fill=candidate), stat='identity')
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-46-1.png)

Standardmäßig sind solche Graphiken gestapelt. Dies kann mit dem Parameter `position` gesteuert werden, der auf `dodge` (für gruppierte Balken) oder `fill` (Stapeln auf 100 %) eingestellt werden kann:

``` r
ggplot(data=gop2) + geom_bar(mapping=aes(x=state, y=votes, fill=candidate), stat='identity', position='dodge')
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-47-1.png)

``` r
ggplot(data=gop2) + geom_bar(mapping=aes(x=state, y=votes, fill=candidate), stat='identity', position='fill')
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-47-2.png)

Wir können auch dafür sorgen, dass sich die gruppierten Balken zu 100 % addieren. Hierfür müssen wir das Verhältnis manuell berechnen.

``` r
gop2 <-  gop2 %>% group_by(state) %>% mutate(vote_prop=votes/sum(votes)) # Neuer Datensatz (mit dem "alten" Namen) mit der manuellen Berechnung

ggplot(data=gop2) + geom_bar(mapping=aes(x=state, y=vote_prop, fill=candidate), stat='identity', position='dodge') + ylab("Votes (%)")
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-48-1.png)

Beachtet hier bitte, dass `group_by %>% summarize` den Datensatz im Ganzen verändert und `group_by %>% mutate` dem bestehenden Datensatz lediglich eine weitere Spalte/Variable hinzufügt (also in diesem Fall das Eregbnis der manuellen Berechnung `vote_prop=votes/sum(votes)`).

### 5.5. Linien-/Kurvendiagramme

Zu guter Letzt ein weiterer Evergreen der Datenvisualisierung: das Liniendiagramm.

Wir können zum Beispiel den Erfolg von Donald Trump darstellen, indem wir seinen Stimmenanteil über Zeit betrachten. Zuerst kombinieren wir die Ergebnisse pro Bundesstaat mit den Terminen der Primaries (hierfür verwende ich ein paar euch bisher unbekannte, da noch nicht besprochene, Funktionen zum Zusammemnführen von Daten):

``` r
schedule  <-  read_csv(paste(csv_folder_url, "primary_schedule.csv", sep="/")) # Der Datensatz "primary_schedule" stammt ebenfalls aus der Github-Seite des Houston meetups

schedule <-  schedule %>% mutate(date = as.Date(date, format="%m/%d/%y")) # hier transformieren wir die Daten der Vorwahlen in Monat-Tag-Jahr

trump <-  results_state %>% group_by(state, party) %>% mutate(vote_prop=votes/sum(votes)) %>% filter(candidate=="Donald Trump") # die Daten für die US Staaten werden gesubsettet um die Ergebnisse für Donald Trump zu isolieren, außerdem wird eine neue Variable ("vote_prop") mittels manueller Berechnung (votes/sum(votes)) erstellt.

trump <-  left_join(trump, schedule) # jetzt führen wir die Daten der Vorwahlen mit dem Trump-spezifischen Datensatz zusammen

trump <-  trump %>% group_by(date) %>% summarize(vote_prop=mean(vote_prop)) # gruppieren und zusammenfassen
trump
```

    ## # A tibble: 17 x 2
    ##    date       vote_prop
    ##    <date>         <dbl>
    ##  1 2016-02-01     0.243
    ##  2 2016-02-09     0.360
    ##  3 2016-02-20     0.325
    ##  4 2016-02-23     0.461
    ##  5 2016-03-01     0.341
    ##  6 2016-03-05     0.343
    ##  7 2016-03-08     0.397
    ##  8 2016-03-15     0.413
    ##  9 2016-03-22     0.357
    ## 10 2016-04-05     0.360
    ## 11 2016-04-19     0.604
    ## 12 2016-04-26     0.602
    ## 13 2016-05-03     0.546
    ## 14 2016-05-10     0.752
    ## 15 2016-05-17     0.666
    ## 16 2016-05-24     0.789
    ## 17 2016-06-07     0.770

Nehmt euch Zeit, den obigen Code zu verstehen. Zeile für Zeile! Das geht am besten, wenn ihr in einem eigenen `R`-Skript die Ausgabe jeder Zeile untersucht und die Ergebnisse und Auswirkungen jedes Befehls zurückverfolgt.

``` r
# plotting 

ggplot(trump) + geom_line(aes(x=date, y=vote_prop))
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-50-1.png)

Machen wir jetzt das selbe für mehr Kandidaten. Diesmal wählen wir die Demokratischen-Kandidaten aus:

``` r
dems <-  results_state %>% filter(party=="Democrat") %>% left_join(schedule)
dems <-  dems %>% group_by(date, candidate) %>% summarize(votes=sum(votes)) %>% mutate(vote_prop=votes / sum(votes))
ggplot(dems) + geom_line(aes(x=date, y=vote_prop, colour=candidate))
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-51-1.png)

Bonusfrage: Im Code für den Trump-Plot wurde der Anteil in zwei Anweisungen berechnet (zuerst pro Staat, dann pro Datum). In dem Demokraten-Code wurde er nur pro Datum berechnet. Inwiefern spielt das eine Rolle? Ist eine der beiden Berechnungen korrekter als die andere?

Nur um euch einige der weiteren Möglichkeiten von `ggplot` zu zeigen, erstellen wir jetzt ein Diagramm aller republikanischen Vorwahlergebnisse am Super Tuesday (1. März):

``` r
super <-  results_state %>% left_join(schedule) %>% 
  filter(party=="Republican" & date=="2016-03-01") %>% 
  group_by(state) %>% mutate(vote_prop=votes/sum(votes))

ggplot(super) + geom_bar(aes(x=candidate, y=vote_prop), stat='identity')  + facet_wrap(~ state, nrow = 3) + coord_flip()
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-52-1.png)

Ihr müsst nicht direkt alles in den präsentierten Code-Zeilen verstehen. Das wird alles mit Zeit und Übung kommen. Wichtig ist hier nur eines: ihr müsst den Code sehen und zumindest die Logik dahinter verstehen.

### 5.6. Themes

Die Anpassung von Dingen wie Hintergrundfarbe, Gitterfarbe usw. wird durch sogenannte Themes gehandhabt. `ggplot` hat zwei standardmäßige Themes: theme\_grey (Standard) und theme\_bw (für ein minimalistischeres Theme mit weißem Hintergrund).

Das Paket `ggthemes` (wie immer: einmalig `install.packages("ggthemes")` und dann vor jeder Verwendung `library(ggthemes)`!) hat einige weitere Themes, darunter ein `economist`-Theme (basierend auf der Zeitung). Um ein Thema zu verwenden, müsst ihr das einfach nur als Codezeile definieren:

``` r
library(ggthemes)
ggplot(trump) + geom_line(aes(x=date, y=vote_prop)) + theme_economist()
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-53-1.png)

Einige Links, um mehr über Themes zu erfahren:

<https://ggplot2.tidyverse.org/reference/theme.html> <https://www.datanovia.com/en/blog/ggplot-themes-gallery> <http://rstudio-pubs-static.s3.amazonaws.com/284329_c7e660636fec4a42a09eed968dc47f32.html> (hier werden auch nette und für euch verfügbare Daten verwendet. das reinschauen lohnt sich!)

### 5.7. Karten!

Geografische Informationen können in ggplot ähnlich wie Streudiagramme geplottet werden. Dafür werden einfach einfach Längen- und Breitengrad als x und y verwendet. Oftmals möchten wir Daten auf einer bestimmten Karte (eines Teils) der Welt plotten, z. B. um Standorte von Tweets zu plotten oder eine Karte mit Informationen pro Land oder Staat einzufärben.

In ggplot wird das realisiert, indem die Umrisse der Länder geplottet werden. Das Paket enthält standardmäßig (sie werden bei der Paket-Installation mitgeliefert) Daten für die USA, die Welt und einige Länder wie Frankreich (nicht die EU oder Deutschland). Die Karten stammen original aus dem Paket `maps`. Das heißt ihr könnt in der Dokumentation von `maps` (wie immer: `?maps`) die Liste der Länder nachschauen.

``` r
states <-   map_data("state") # die Namen der beim Paket (hier ggplot2) mitglieferten Daten (hier "state") findet ihr in der Dokumentation des jeweiligen Pakets und natürlich auf CRAN!
head(states)
```

    ##        long      lat group order  region subregion
    ## 1 -87.46201 30.38968     1     1 alabama      <NA>
    ## 2 -87.48493 30.37249     1     2 alabama      <NA>
    ## 3 -87.52503 30.37249     1     3 alabama      <NA>
    ## 4 -87.53076 30.33239     1     4 alabama      <NA>
    ## 5 -87.57087 30.32665     1     5 alabama      <NA>
    ## 6 -87.58806 30.32665     1     6 alabama      <NA>

Wir können diese Daten sofort plotten, indem wir die Funktion `geom_polygon` verwenden. Wir geben x und y als Längen- und Breitengrad an, füllen nach Staaten und machen die Grenzen weiß.

``` r
ggplot(data = states) + 
  geom_polygon(aes(x = long, y = lat, fill = region, group = group), color = "white") + 
  coord_fixed(1.3) + guides(fill=FALSE)  
```

![](R_TransSumVis_with_tidy_files/figure-markdown_github/unnamed-chunk-55-1.png)

Hinweis: Der Befehl `coord_fixed()` fixiert das Seitenverhältnis auf 1,3 und `guides(fill=FALSE)` verhindert dass eine Legende gezeichnet wird (in diesem Fall würde die Legende für jeden US Staat die zugeordnete Farbe auflisten).