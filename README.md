# LaborprotokolleMT1-Vorlage
LaTeX-Vorlage für die Laborprotokolle aus dem ersten und zweiten Semester des Studiengangs Medientechnik an der HAW Hamburg.

Diese Repository ist eine GitHub-Vorlage, die die grundlegende Struktur und Formatierung bereits enthält. Für einige Befehle und Formatierungsmöglichkeiten sind an entsprechender Stelle Beispiele gegeben. Besondere Befehle werden ebenfalls in dieser ReadMe erläutert.

## Benutzung der Vorlage
Die Vorlage kann in der GitHub WebApp zum Erstellen einer neuen Repository verwendet werden. Diese enthält dann bereits die nötigen Dateien und kann nachfolgend modifiziert werden.

Getestet wurde die Vorlage mit MacTex / TexLive. **(Muss vorab installiert werden!!!)**

Informationen zum Klonen einer Vorlage können hier gefunden werden:
https://docs.github.com/de/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template

## Befehle und Beispiele

### Eigene Befehle
Zur Vereinfachung des Arbeitsprozesses wurden einige eigene Befehle definiert. Einige sind für die Erstellung von Laborprotokollen jedoch nicht relevant und werden deshalb nicht erwähnt. Ein Blick in die Datei `Befehle.tex` lohnt sich dennoch trotzdem.

#### Referenzen
Abbildungen und Tabellen werden bei Verwendung der korrekten Einfügemethoden immer automatisch mit dem korrekten Label versehen. Alle funktionieren nach dem gleichen Muster: `\Befehl{Caption}{Label}{Datei}`.

Es gibt die folgenden Befehle zum Einfügen und Referenzieren.
````LaTeX
\tabelle{Messdaten für Aufgabe 1}{A1_Messdaten}{src/Aufgabe1/Data/Messdata.csv}
\reftable{A1_Messdaten}
\reftablefull{A1_Messdaten}

\graph{Graph für Aufgabe 1}{A1_Graph}{src/Aufgabe1/Data/Messdata.csv}
\mygraphref{A1_Graph}

\circuit{Messschaltung für Aufgabe 1}{A1_Circuit}{src/Aufgabe1/Circuit/Circuit.tex}
\mycircuitref{A1_Graph}

% Allgemein:
\myfigureref{}{}{}
\myfigurereffull{}{}{} % Enthält auch Seitenzahl
````

#### ToDo Notes

Für eine bessere Zusammenarbeit können ToDo Notizen verwendet werden. Es gibt verschiedene Varianten mit verschiedenen Farben und Einbindungen.

````LaTeX
\ausdruck{}
\ergaenzungen{}
\hinweis{}
\mytodo{}
````

Für die Fragen im Rahmen der Auswertung ist der Befehl `\Frage{}` vorgesehen.

#### Mathe und Formeln

Um sich Schreibarbeit zu ersparen empfiehlt sich immer die Verwendung von `eqnarray`. Dort können mittels Linebreak auch mehrere Gleichungen eingefügt werden und mit dem Kaufmännischem "Und" um das Glecihheitszeichen herum auch alle aneinander ausgerichtet werden. `&=&`

````LaTeX
\begin{eqnarray}
    P_V &=& U_V \cdot I_V \\
    P_I &=& I_V \cdot (U_{leer} - U_V) \\
    \eta &=& \frac{P_V}{P_V + P_I}
\end{eqnarray}
````

Sofern mit Zahlen gerechnet wird, soll sich zwischen Zahl und Einheit ein Abstand befinden. Laut SI-Konsortium soll es sich dabei jedoch nicht um ein vollständiges Leerzeichen handeln. Es ist ein "Non-Breaking-Space" vorgesehen. Dafür sollte der Befehl `\SI{}{}` verwendet werden. Im untenstehendem Beispiel in der Ausführung als In-Line Mathe.

````LaTeX
$R_V \approx \SI{220}{\Omega}$
````

### Erstellung von Schaltkreisen in LaTeX

Für die Erstellung von Schaltbildern wird das Paket `circuitiks` verwendet. Es ist bereits in der Vorlage enthalten. Es folgt ein Beispiel zur Verwendung des Codes.

````LaTeX
\begin{circuitikz}[european]\draw
    (0,0) to[battery1] (0,6) -- (1,6)
    (1,6) to[voltmeter = $U_q$, *-*] (1,0) -- (0,0)
    (1,6) to[ammeter = I] (3,6)
    (3,6) to[R=$R_1$, *-*] (3,3)
    (3,6) -- (5,6) to[voltmeter = $U_1$, -*] (5,3) -- (3,3)
    (3,3) to[R=$R_2$, *-*] (3,0) -- (1,0)
    (3,3) -- (5,3) to[voltmeter = $U_2$] (5,0) -- (3,0)

;\end{circuitikz}
````

Wichtig ist hier das Attribut `[european]`. Ohne dieses werden die Widerstände nicht nach europäischem Standard dargestellt. Aus Gründen der Einfachheit empfiehlt sich insbesondere bei großen Schaltbildern das Aufzeichnen in einem Koordinatensystem.

Eine grundlegende Anleitung zu diesem Paket gibt es hier:
https://www.overleaf.com/learn/latex/LaTeX_Graphics_using_TikZ%3A_A_Tutorial_for_Beginners_(Part_4)—Circuit_Diagrams_Using_Circuitikz

Die Dokumentation zu diesem Paket kann hier gefunden werden:
https://texdoc.org/serve/circuitikzmanual.pdf/0


### Diagramme mit LaTeX zeichnen

Für die Erstellung der Diagramme wurde das Paket `pgfplots` der Vorlage hinzugefügt. Es folgt ein Beispiel zur Verwendung des Codes.

````LaTeX
\begin{tikzpicture}
    \begin{axis}[
        title={$U_v=f(Iv)$},
        xlabel={$I_v$ / $mA$},
        ylabel={$U_v$ / $V$ },
        xmin=0, xmax=70,
        ymin=0, ymax=16,
        xtick={0,10,20,30,40,50,60,70},
        ytick={0,2,4,6,8,10,12,14,16},
        legend pos=north west,
        ymajorgrids=true,
        grid style=dashed,
    ]
    
    \addplot table[
        color=blue,
        mark=circle, 
        col sep=semicolon,
        x index=0,
        y index=2
        ]{src/Aufgabe4/Data/Data4_b.csv};

    \legend{Alpha = 0.3, Alpha = 0.6}
        
    \end{axis}
\end{tikzpicture}
````

Ein Graph wird mit dem Befehl `\addplot` hinzugefügt. Eine Abbildung kann meherere Graphen enthalten. Mit dem Attribut `[smooth]` direkt hinter `\addplot` kann der Graph parabelartig gezeichnet werden. 
Hinter `\addplot` muss dann der Typ der Datenquelle folgen. Die zwei wichtigsten Datenquellen sind `coordinates` und `table`. Während bei `coordinates` die Punkte direkt dahinter eingegeben werden, wird bei `table` der Dateipfad zu einer `.dat` oder `.csv`-Datei angegeben. Mit anderen Datentypen sind auch Mathematische Funktionen möglich.

Empfohlen wird das Verwenden einer `csv`-Datei. Da diese dann später auch für das Anlegen der Tabellen verwerwendet werden kann und somit die Daten nur einmal übertragen werden müssen. Die `csv` sollte jedoch das Semicolon als Trennzeichen verwenden, da das Komma im europäischen Raum bereits anderweitig bei Zahlen Verwendung findet. Ein Komma als Dezimaltrennzeichen kann von `pgfplots` **nicht** gelesen werden und muss daher vorab durch Punkte ersetzt werden. Diese können dann auch als Dezimaltrennzeichen interpretiert werden. Sofern die gleichen Daten dann aber auch in einer Tabelle abgedruckt sind, sind die Punkte dort ebenfalls als Dezimaltrennzeichen zu interpretieren. Da das nicht dem Standard im europäischem Raum entspricht, sollte darauf möglicherweise einmal mittels einer Fußnote hingewiesen werden.
Der Befehl für das Plotten eines Graphen aus einer `csv`-Datei ist der folgende:

````LaTeX
\addplot table [col sep = comma]{src/Aufgabe1/Data/Graph1.csv};
````

Ebenfalls ist es möglich und empfehlenswert die Achseneinteilung automatisch generieren zu lassen. Dazu müssen einfach nur die Attribute für die Maximalwerte und die Schritte entfernt werden.
Die Reihenfolge der Inhalte der Legende muss der Reihenfolge entsprechen, in der die Graphen eingefügt werden. Es darf nur einen Befehl für die Legende pro Diagramm geben, andernfalls wird sie überschrieben.

Weiterführende Informationen (zu weiteren Diagramtypen) können unter dem folgenden Link gefunden werden.
https://www.overleaf.com/learn/latex/Pgfplots_package

Für kompliziertere Anwendungen kann die offiziellen Dokumentation helfen:
https://ftp.gwdg.de/pub/ctan/graphics/pgf/contrib/pgfplots/doc/pgfplots.pdf

### Anlegen von Tabellen

Wie weiter oben erwähnt können auch die Tabellen direkt aus einer `csv`-Datei generiert werden. Hier ein Beispiel dafür:

````LaTeX
\begin{tabular}{|c|c|c|c|}
    \hline
    \bfseries Variante & \bfseries $I_g$ / A & \bfseries $U_1$ / V & \bfseries $U_2$ / V
    \csvreader[head to column names,separator=semicolon]{src/Aufgabe1/Data/Data1_gemessen.csv}{}
    {\\\hline\textbf{\csvcoli}\ & \csvcolii & \csvcoliii & \csvcoliv}\\
    \hline
\end{tabular}
````