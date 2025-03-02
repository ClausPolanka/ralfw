ANFORDERUNGSANALYSE FÜR ENTWICKLER, TEIL 2

Den Stier bei den Hörnern packen

Wie Sie die Komplexität beim Schreiben von Produktionscode mit einem iterativ-inkrementellen testgestützten Vorgehen in den Griff bekommen.

Was ist der Zweck der Anforderungsanalyse? Die Entwicklung von Verständnis. Nur mit Verständnis können Sie Code schreiben, der die Anforderungen erfüllt. Anforderungen sind erfüllt, wenn der Code das gewünschte Verhalten zeigt, also bei gegebenem Zustand auf Input mit erwartetem Output inklusive einer gegebenenfalls erwarteten Zustandsänderung reagiert. Woher wissen Sie aber, dass Sie Anforderungen verstehen? Indem Sie sie dem Product Owner (PO) oder Kunden „spiegeln“, ihm also erzählen, was Sie meinen, verstanden zu haben? Nein. Das kann nur eine erste Näherung sein, der Versuch einer Optimierung.

Wie stellen Sie fest, ob der Code tut, was er tun soll? Das können Sie nicht dem Kunden überlassen. Denn bis der Code beim Kunden ist und dieser Gelegenheit hat, sich damit auseinanderzusetzen, vergeht viel Zeit, in der es ungewiss ist, ob Ihr Aufwand erfolgreich oder vergeblich war. Solche Ungewissheit ist kein schöner emotionaler Zustand; vor allem aber führen längere Wartezeiten dazu, dass Sie beginnen, weitere Anforderungen umzusetzen. Wartezeiten führen zu einer Erhöhung von Work-in-Process (WIP), und das führt über kurz oder lang zu Unzuverlässigkeit. Die Literatur zu diesem Zusammenhang ist reichlich; Ihre persönliche Erfahrung sollte das ebenfalls bestätigen.

Was folgt daraus?

Softwareentwicklung ist notwendig immer iterativ. Ein Gefühl von Verständnis der Anforderungen führt nicht geradlinig zu befriedigendem Code. Dafür sind realistische Anforderungen zu komplex. Vielmehr ist jede Codelieferung nur eine Hypothese, die es zu überprüfen gilt. Wird sie falsifizert, dann muss in einer nächsten Iteration die Abweichung vom Soll verringert werden.

Um mit Iterationen möglichst schnell aus der Unsicherheit zu kommen, sollten Anforderungen in überschaubare Abschnitte zerlegt werden, deren iterative Umsetzung zügig verlaufen kann, am besten innerhalb von Stunden oder maximal wenigen Tagen. Mit jedem Anforderungsabschnitt wächst der Anteil der umgesetzten Anforderung, das heißt, die Softwareentwicklung sollte inkrementell vorgehen – wobei jedes Inkrement idealerweise für den Kunden einen kleinen Wertzuwachs der Software darstellt.

Um die Wartezeit bis zu einem Urteil über die Hypothese, dass eine Codeversion nun endlich tatsächlich die Anforderungen eines Abschnitts befriedigt, so kurz wie möglich zu halten, sollten Überprüfungen den Kunden nur selten involvieren. Ultimativ ist er natürlich die beurteilende Instanz, doch solide vorläufige Urteile lassen sich auch innerhalb des Softwareentwicklungsteams fällen. Am besten geschieht das durch automatisierte Tests. Sie sind jederzeit reproduzierbar, nachvollziehbar, nicht subjektiv, schnell, analysierbar und dokumentierend. Die Mühe, die zu ihrer Entwicklung aufgewendet werden muss, zahlt sich schnell aus. Nur automatisierte Tests skalieren mit dem zunehmenden Aufwand, den die Vermeidung von Regressionen erfordert (Stabilitätstests).

So weit die Rekapitulation des iterativ-inkrementellen testgestützten Vorgehens der Agilität. Es bleibt allerdings noch­eine Frage offen: Wenn Tests nicht manuell über die Benutzerschnittstelle durchgeführt werden, wo setzen denn dann automatisierte Tests im Code an? Automatisierte Tests setzen an Funktionen im Code an. Bild 1 zeigt die Notwendigkeit dafür: Ist das Ergebnis, das der Sparrechner ausgibt, korrekt? Ein Vergleich mit einem Online-Sparrechner (Bild 2) lässt Zweifel aufkommen. Aber wie soll ein automatisierter Test des Typescript-Codes punktgenau an die Domänenlogik angelegt werden?

Der Code für die Benutzerschnittstelle davor und danach macht das schwer. Wenn jedoch Benutzerschnittstelle und Domäne getrennt werden, ist eine punktgenaue „Testsonde“ möglich (Bild 3). Damit lässt sich die Reife des Domänencodes („Ist der Code schon korrekt?“) und die Stabilität des Domänencodes („Ist der Code noch korrekt?“) einfach und zügig jederzeit von jedermann überprüfen. Eine Anforderungsanalyse, die auf solche Testbarkeit abzielt, die also als Ziel hat, automatisiert testbare Funktionen zu finden, nenne ich Slicing. Jedes Slice ist ein Ab-Schnitt der Gesamtanforderungen, ein Inkrement. Speziell an diesem Inkrement ist, dass es ganz konkret drei Dinge benennt:

Welche Funktion wird konkret getriggert als Wurzel für die Herstellung des gewünschten Verhaltens? (Es können auch mehrere Funktionen sein.) In Bild 3 ist es zum Beispiel Endbetrag_berechnen.

Welche Signatur hat diese Funktion, und mit welchem Zustand arbeitet sie? Die benötigten Datenstrukturen sind also genau zu klären. Im Beispiel sind das das Input-Tupel (sparbetrag:number, zinssatz:number, jahre:number) und der Output-Typ number; Zustand ist nicht im Spiel.

Welche Testfälle sollen erfüllt werden, um zu belegen, dass das Verständnis der Anforderung korrekt umgesetzt wurde? Im Beispiel ist es das Input-Output-Paar ((100, 6, 12), 20901,86). (Der erwartete Wert stammt aus Bild 2. Die Annahme ist, dass der Online-Sparrechner korrekt funktioniert.)

Slicing ist damit konkreter als die übliche agile Anforderungsanalyse. Über User Stories oder Use Cases oder andere Anforderungsformen soll gerne in jeder Weise ausführlich gesprochen werden. Slicing steht dem nicht im Wege. Als Entwickler sollten Sie jedoch nicht zufrieden sein, solange sie nicht aus der Analyse mit konkreten Funktionssignaturen und Testfällen herausgehen. 

Im ersten Teil der Artikelserie [1] habe ich deshalb geschrieben: „Setzen Sie [dem User Story Template] ein eigenes Template für sich als Entwickler gegenüber, mit dem Sie die Qualität von Anforderungen aus Ihrer Perspektive beurteilen: ‚A function with signature <signature> to satisfy the test <test case> by implementing <causal chain>.‘“ Das meine ich wirklich genau so. Meine Begründung ist meine Erfahrung, die mich gelehrt hat, dass weniger Präzision und weniger Klarheit schnell zu Rückfragen führen. Und Rückfragen bedeuten Wartezeiten. Wartezeiten kosten aber nicht nur Geld, sondern verleiten auch zu einer lokalen Optimierung durch Beginn anderer Aufgaben. Das führt zur Vergrößerung von WIP mit allen negativen Konsequenzen. Ich weiß, ich wiederhole mich, doch dieser Zusammenhang ist so wichtig, dass ich das für nötig halte.

Ganz davon zu schweigen, dass ein PO oder Kunde durch Rückfragen unterbrochen wird und sich schnell genervt fühlt. Das vergrößert die Spannungen im Team. Mit weniger als solcher Präzision mit dem Entwurf und der Codierung zu beginnen ist eine Abkürzung, die leicht in eine Sackgasse führt. Es ist eine vorzeitige Optimierung, um sich nicht länger mit dem PO oder Kunden in Diskussionen verhaken zu müssen. Außerdem macht alles nach der Anforderungsanalyse mehr Spaß; dafür sind Sie Entwicklerin beziehungsweise Entwickler geworden, oder? Doch es hilft nichts: Weniger Präzision rächt sich alsbald. Dann ist der Codierspaß auch vorbei. 

Also: Anforderungsanalyse für Softwareentwickler ist nicht nur inkrementell, sondern produziert so feine Schnitte, dass jeder durch mindestens eine testbare Funktion inklu­sive Testfällen spezifiziert ist. Das ist für mich der formale, sehr leicht zu erkennende Endpunkt einer Analyse. Dann können Sie aufstehen und den PO anderen Dingen überlassen. Sie sind bereit, den Code hinter den „Triggerfunktionen“ der Slices zu entwerfen und zu codieren. Dass Sie bei der Anforderungsanalyse mit Slicing Funktionen, Signaturen, Testfälle im Hinterkopf haben, muss den PO übrigens nicht verwirren. Sie müssen ihm diese Details nicht aufdecken. Sie können trotzdem mit ihren Fragen die Analyse so leiten, dass für Sie am Ende diese Präzision herauskommt.

Vom Wert der Prototypen für die Anforderungsanalyse

Doch was, wenn solche Präzision sich bei allem Bemühen nicht einstellen will? Das ist misslich, aber kein Beinbruch. Mangelnde Präzision bei der Anforderungsanalyse bedeutet nicht, dass Sie nicht weitermachen können. Nur ist es ein anderer Modus, in dem Sie fortfahren:

Wenn Sie Anforderungen präzise „geslicet“ haben, geht es weiter mit Entwurf und Codierung von Produktionscode.

Wenn Sie Anforderungen nicht so präzise schneiden konnten, geht es weiter mit Prototypen. Prototypen – in Code oder anderer Art – sind Analysehilfsmittel. Mit ihnen unterstützen Sie den PO und sich selbst dabei, die Anforderungen besser zu verstehen. Anforderungen wollen auch entwickelt werden. Das Entwicklungsziel sind präzise Slices. Prototypen helfen Ihnen dabei, indem sie bisheriges Verständnis greifbarer machen. Der PO oder Sie machen Bekanntschaft mit neuen Sichtweisen, spüren Kon­traste zum bisherigen Verständnis und können erste Hypothesen darüber, was eigentlich die wahren Anforderungen sind, leichtgewichtig überprüfen. Prototypen sind getrennt von Produktionscode. Mit Prototypen wird Produktionscode „nicht verwirbelt“ oder zumindest nicht verändert.

Iterative Entwicklung gibt es für Code. Dort ist ihr Ziel, für gegebene Anforderungen korrekten, befriedigenden Code herzustellen. Iterative Entwicklung gibt es aber auch für Anforderungen. Anforderungen selbst sind nicht korrekt, vollständig, verständlich, nur weil man bei einem Sprint-Planning „mal darüber redet“. Verständnis kann man sogar für inkorrekte oder irrelevante Anforderungen entwickeln. Sie müssen also verstehen, dass auch Anforderungen durch einen Schärfungsprozess, durch Feedbackschleifen gehen müssen – vor und unabhängig von Code. Korrekte, vollständige, relevante Anforderungen zu finden, indem Iterationsschleifen inklusive Veränderung von Produktionscode durchlaufen werden, ist jedoch sehr teuer, langwierig und belastet die Struktur des Produktionscodes. Deshalb sollten Sie sensibel dafür sein, wann Anforderungen noch nicht wirklich „gut“ sind. Solange das der Fall ist, sollten Sie ablehnen, für sie Produktionscode anzufassen. Iterationen zu ihrer Verbesserung sind vor beziehungsweise unabhängig von Produktionscode zu durchlaufen. Genau dafür sind Prototypen da. 

Aber wie nun Anforderungen als Entwickler angehen? Welchem Weg folgen, welche Methode anwenden, um das Ziel „A function with signature <signature> to satisfy the test <test case> by implementing <causal chain>“ zu erreichen?

Slicing: Der Weg zur Funktion

Ich schlage mit Slicing eine schrittweise Verfeinerung von außen nach innen, von grob zu fein vor. Damit können Sie den PO oder sogar Kunden an die Hand nehmen. Sie müssen ihnen nicht jedes technische Detail dabei verraten, auf das Sie aus sind. Sie können sie jedoch mit Grafiken unterstützend begleiten und sie so visuell durch den „Verfeinerungstrichter“ führen.

Die Hierarchieebenen sind (Bild 4):

System: Das gesamte Softwaresystem in seiner Umwelt im Überblick.

Context: Das Softwaresystem, zerlegt in große thematische Bereiche.

App: Ein Context, zerlegt in unterschiedliche, eigenständige Zugänge für Anwenderrollen.

Worker: Eine App, zerlegt in grobe selbstständige Einheiten mit eigenen Schnittstellen für Mensch oder Software.

Perspective: Ein Worker, betrachtet durch unterschiedliche Benutzerbrillen.

Dialog: Eine Perspective, zerlegt in thematisch fokussierte, deutlich unterscheidbare „Fenster“ mit ihren Übergängen beziehungsweise Verbindungen.

Interaction: Die in einem Dialog triggerbaren Funktionen der Software mit ihren Ausgangspunkten und möglicherweise verschiedenen Endpunkten.

Entry Point: Die „Triggerfunktion“ hinter einer Interaktion mit konkreten Beschreibung der Nachrichten an/von eine(r) „Triggerfunktion“ (Request, Response) und des für sie relevanten Zustands beziehungsweise ihrer Seiteneffekte.

CQS Klassifizierung eines Entry Points gemäß Command- Query-Separation-Prinzip beziehungsweise Aufspaltung der „Triggerfunktion“ in mehrere Funktionen, die CQS- konform sind.

Feature: Die Anforderungen an eine „Triggerfunktion“ in feinere Inkremente zerlegen, ausgehend von ihren Nachrichten und ihrem Zustand.

Der Weg ist mit diesen Ebenen vorgezeichnet: Die Analyse beginnt beim System und endet beim Feature. Doch die Häufigkeit, mit der einzelne Ebenen betrachtet werden, ist sehr unterschiedlich. Um das ganze System oder um Contexts geht es selten, sie sind vor allem initial das Thema. Um Interaktionen hingegen geht es ständig. Nicht jedes Softwaresystem muss auch in gleicher Weise ausführlich auf jeder Ebene betrachtet werden. Je kleiner das System ist, desto einfacher fallen Contexts, Apps, Workers, Perspectives aus; womöglich gibt es auf jeder Ebene nur eine Instanz, über die sogar nicht einmal näher gesprochen werden muss. 

Immer relevant sind allerdings das System als Big Picture und die Ebenen ab Dialog nach unten. Kein Softwaresystem ist zu klein, dass hier nicht näher hingeschaut werden könnte und sollte. Die Hierarchie soll Ihnen helfen, immer und immer wieder Schnitte (Bild 5) von oben nach unten, von außen nach innen durchzuführen, um mit jedem Slice nur ein Feature eines Entry Points umsetzen zu müssen. Wenn es fein geschnitten ist, kann das zügig geschehen. Für jedes Feature, für jeden Entry Point, für jede Interaktion ist dabei klar, was der größere Zusammenhang ist. Alle Feature-Slices sind eingebettet in ein Ganzes in unterschiedlicher Granularität.

Die Analyse von oben nach unten verläuft iterativ. Die Struktur jeder Ebene und ihre Slices ergeben sich nicht einfach so, sondern wollen erarbeitet sein. Auf höheren Ebenen kann sich auch durch Erkenntnisse auf tieferen Ebenen etwas verändern; sogar die Implementation eines Features kann sich nach oben auswirken. Genauso ist die Umsetzung eines Features selbst iterativ, nicht geradlinig (Bild 6). Die Ebenen der Hierarchie und das Vorgehen können POs und Kunden verständlich gemacht werden. Auch das Ziel einer glasklaren Vorstellung von der Anforderung, die sich möglichst leicht ohne Rückfragen umsetzen lässt, werden PO und Kunden teilen. Für Sie als Entwickler ist jedoch darüber hinaus interessant, dass jede Ebene eine technische Entsprechung hat. Bild 7 macht dafür einen Vorschlag.

Im Moment mag das für Sie etwas überwältigend erscheinen. Was sollen all die Ebenen? Muss das sein? Zum Glück ist die Antwort darauf: Nein, das muss nicht sein. Genauer: Das muss nicht alles immer sein. Die Hierarchieebenen stellen ein Angebot dar. Wenn Sie sich bei der Anforderungsanalyse unsicher fühlen, können Sie daran Halt finden. Sie können sich fragen: „Wo bin ich?“, „Wohin kann ich als Nächstes gehen?“ oder „Habe ich auch alles bedacht?“ Wenn Sie sich anschicken, einem SaaS-Platzhirsch wie MailChimp seine Position streitig zu machen, tun Sie sicher gut daran, sich sorgfältig von oben nach unten durch alle Ebenen „zu graben“. 

Für den obigen Sparrechner jedoch sind zum Beispiel nur System, Dialog, Interaktion und Entry Point relevant. Alles andere liegt auf der Hand. Wer sich auf Slicing einlässt, wird ein Gefühl dafür bekommen, wann auf welcher Hierarchieebene eine weitere Anforderungsanalyse beginnen sollte. Am Anfang beginnt sie stets beim System. Doch wenn Sie sich von dort schon einmal bis zu Entry Points heruntergearbeitet haben, können Sie beim nächsten Mal vielleicht bei schon gefundenen Interaktionen weitermachen oder in einen anderen Dialog einsteigen. 

Die Hierarchie ist kein Korsett. Sie bietet vor allem Überblick darüber, dass Software beziehungsweise Anforderungen überhaupt auf unterschiedlichen Abstraktionsebenen verstanden werden können, und macht einen konkreten Vorschlag für solche Ebenen aus der Erfahrung heraus. Wenn Sie meinen, es gebe noch andere Ebenen, wenn Sie meinen, das gehe für Sie (meistens) zu weit oder nicht weit genug … dann vertrauen Sie sich und verändern Sie die Hierarchie. Für mich gibt es dabei nur zwei Fixpunkte (Bild 8): Anforderungsanalyse beginnt immer beim gesamten System, in dem einige Aspekte als architektonische Grundpfeiler identifiziert werden sollten. Anforderungsanalyse läuft immer auf einzelne Funktionen mit Signatur und Zustandsinformation hinaus, um Testbarkeit zu garantieren.Passen Sie gern alles andere an, aber behalten Sie diese beiden Ebenen stets im Blick. Beide sollten in jeder Anforderungsanalyse mit PO oder Kunden ausgefeilt werden. 

Um das für Sie greifbar zu machen, steige ich oben in der Hierarchie mit näheren Erläuterungen und Beispielen ein.

Das System

Aus ganz hoher Flughöhe betrachtet ist jedes Softwaresystem zunächst „ein Ding“, eine Einheit ohne Struktur. Sie werden im Grunde beauftragt, eine Black Box zu bauen, die in einer Umwelt einen bestimmten Dienst verrichtet (Bild 9). Gewöhnlich sind die Anforderungen so umfangreich, dass Sie nicht einfach den Code hinschreiben können, der sie erfüllt, also die Logik. Nur Logik erzeugt ja Verhalten. Auch deshalb müssen Sie die Black Box strukturieren. Das ist die Aufgabe des Entwurfs. Die Mittel zur Strukturierung sind Module, zum Beispiel Klassen. Die Frage, die sich im Entwurf stellt, ist: „Welche Module sollte es denn geben?“ Sie ist für nichttriviale Anforderungen schwer zu beantworten, und die Antwort ändert sich auch über die Zeit. Deshalb sollten Sie jede Hilfe willkommen heißen, die Sie bekommen können. Slicing bietet dabei Unterstützung. Schon während der Anforderungsanalyse erhalten Sie Hinweise auf Module, die es mindestens geben sollte. Weitere werden Sie während eines nachgelagerten expliziten Entwurfs finden (müssen). Die „Basismodule“ für ein Softwaresystem ergeben sich aus der Kommunikation des Softwaresystems mit seiner Umwelt. Die Erfahrung hat gezeigt, dass es sich lohnt, dafür spezifische Module aufzusetzen. Kommunikation findet an der Grenze des Softwaresystems statt: Es hat einen Innenraum, in dem es das gewünschte Verhalten mit Code herstellt. Und es existiert in einer Umwelt, der es dient beziehungsweise aus der es Dienste benötigt (Bild 10).

Jedes Softwaresystem dient Benutzern (User); das können Menschen oder andere Softwaresysteme sein.

Viele Softwaresysteme nutzen Ressourcen (Resource); das können Hardwareressourcen (zum Beispiel eine Festplatte oder ein Timer) sein oder andere Softwaresysteme (etwa eine Datenbank oder SAP).

Das Softwaresystem, das Sie entwickeln sollen, ist abhängig von Ressourcen, und seine Benutzer sind abhängig von ihm. Für diese stellt es selbst eine Ressource dar. Beachten Sie die Beziehungslinien in den Grafiken ab Bild 9: Sie enden nicht mit einer Pfeilspitze, wie sonst üblich, sondern in einem Punkt. Er drückt die Abhängigkeitsbeziehung aus. Dadurch wird sie klar unterschieden von Datenflüssen, in denen keine Abhängigkeiten bestehen. Natürlich sind die Zahlen der Benutzer und Ressourcen beliebig. Allerdings gibt es immer mindestens einen Benutzer. Ressourcen sind optional. Benutzer und Ressourcen können auch beliebig um das Softwaresystem herum angeordnet werden, selbst wenn sich Benutzer auf der linken Seite und Ressourcen auf der rechten eingebürgert haben und irgendwie angesichts der Abhängigkeitsverhältnisse intuitiv sind, die auf diese Weise von links nach rechts weisen. Bei den Benutzern geht es nicht um konkrete – Peter, Paul, Maria –, sondern um Kategorien, zum Beispiel Sachbearbeiter, Spieler, Kunde. Benutzer haben eine Rolle gegenüber dem System. Die Kommunikation mit Benutzern und Ressourcen ist im Grunde immer bidirektional: Requests fließen in die eine Richtung zum Dienstleister hin, Responses fließen vom Dienstleister zurück (Bild 11).

Wie hilft diese Sicht bei der Strukturierung? Die Erfahrung zeigt, dass der Code, der für die Kommunikation nötig ist, immer getrennt werden sollte von anderem Code. Ja: immer, stets, auf jeden Fall. Die Gründe dafür sind vielfältig: Testbarkeit, Flexibilität, Verständlichkeit. Vor allem sollte das dafür nötige API (oder Framework) vom restlichen System isoliert werden; kontaminieren Sie nicht weite Teile Ihrer Codebasis mit solchen Details. Damit gibt es nun etwas zu sehen in der Black Box beziehungsweise im Black Circle (Bild 12):

Die Kommunikation zwischen Benutzern und Softwaresystem kapseln Module, die im Slicing Portal heißen.

Die Kommunikation zwischen Softwaresystem und Ressourcen kapseln Module, die im Slicing Provider heißen.

Darüber hinaus gibt es stets Code, der das „Eigentliche“ der Software ausmacht. Er ist (weitgehend) unabhängig von der Umwelt und den Kommunikationskanälen. Das ist die Domäne, die ebenfalls in mindestens einem Modul gekapselt werden soll. „Weitgehend“ unabhängig deshalb, weil es sein kann, dass gewisse Technologien insbesondere in Providern erfordern, dass sich die Domäne anpasst. Sie kennt deshalb immer noch nicht das API, aber ist auf Kommunikationsmuster abgestimmt, die einen performanten/skalierbaren Umgang mit der Ressource via Provider leichter machen.

Als Symbole für Portale und Provider – die beide Adapter sind – und Domäne haben sich Rechteck, Dreick und Kreis eingebürgert. Wenn Sie andere Symbole vorziehen, nutzen Sie eben die. Wichtig ist lediglich die visuelle und begriffliche Differenzierung. Slicing teilt diese Sicht auf ein Softwaresystem mit der IODA-Architektur [2]. Das bedeutet, Portale und Provider und Domäne sind innerhalb des Softwaresystems nicht voneinander abhängig. Stattdessen werden sie von einem weiteren Modul verbunden, der sogenannten Integration. Für sie steht im Bild der Innenraum um die Module herum. Doch das ist ein Detail des Entwurfs und spielt für die Anforderungsanalyse keine weitere Rolle. Wesentlich ist, dass dieses Bild vom Softwaresystem die Analyse anleitet, zuerst die Benutzer in ihren Rollen und die Ressourcen zu sammeln. Das informiert den Entwurf, der auf die Analyse folgt. Das hilft beim weiteren Fokussieren der Anforderungsanalyse. Jedes dieser Module hat eine Schnittstelle. Besonders interessant ist für eine Outside-in-Analyse die Benutzerschnittstelle, also wie sich das Softwaresystem vermittels seiner Portale nach außen darstellt. Die Schnittstellen von Domäne und Providern sind Sache des Entwurfs.

Dass seit Visual Basic 1.0 IDEs mit einem User-Interface-Designer so beliebt sind, ist sehr verständlich: Mit ihnen wurde auch von außen nach innen die Implementation vorangetrieben. Die Eventhandler hinter den Steuerelementen wurden als Entry Points genutzt. Dahinter stand die richtige Intuition – nur fehlten dabei die Systematik und das Bewusstsein für sauberen Code. Und so sind viele unwandelbare Codebasen entstanden. Das will Slicing besser machen! Betrachten Sie eine solche „Sollstruktur“ eines Softwaresystems als Checkliste. Während der Anforderungsanalyse können Sie immer wieder fragen:

Kennen wir schon alle Benutzerrollen mit ihren unterschiedlichen Ansprüchen an das Softwaresystem?

Ist die Liste der Ressourcen schon komplett, von denen das Softwaresystem abhängig ist?

Verstehen wir schon wirklich, was die Domäne des Softwaresystems ist?

Die Checkliste erinnert Sie jedoch nicht nur daran, dass Sie nach Portalen und Providern suchen sollten, sondern legt Ihnen auch nahe, deren Ausprägung zu analysieren. Zum Beispiel sollten Sie Antworten auf folgende Fragen mit dem PO erarbeiten:

Welche Technologie soll zum Einsatz kommen?

Gibt es im Team schon Kompetenz für die erforderliche Technologie?

Wie sind die Interaktionsmuster an dieser Stelle zwischen System und Umwelt?

Welche nichtfunktionalen Anforderungen gelten an diesen Kontaktstellen (zum Beispiel Performance, Sicherheit)?

Wie sehen Datenstrukturen aus, die ausgetauscht werden?

Es ist erstaunlich, wie leicht hier etwas duch die Lappen gehen kann. Gerne werden zum Beispiel die Uhrzeit oder ein Zufallszahlengenerator als Ressource übersehen. Und mit einer genauen Vorstellung davon, was eigentlich die Domäne ist, hadern viele POs und Entwickler. Zur Erinnerung: In der Beschreibung der Domäne kann keine Kommunikation mit der Umwelt vorkommen; die Domäne ist unabhängig von der Umwelt. Zu ihr gehört höchstens, was übrig bleibt, wenn man vom Softwareverhalten die Kommunikation subtrahiert. „Höchstens“, weil es auch noch andere Belange als Kommunikation gibt, die zwar in Anforderungen auftauchen, doch nicht zum unverbrüchlichen Kern einer Software gehören, etwa Sicherheit. Solche „supporting concerns“ verdienen ebenfalls Module – doch die ergeben sich nicht so einfach aus der Analyse; dafür ist der Entwurf zuständig.

Die Bewegung in der Slicing-Hierarchie

Was kommt nach dieser „Systemerkundung“? Behalten Sie immer das Ziel im Auge: einzelne „Triggerfunktionen“ mit Signaturen und Zuständen. Jetzt wissen Sie, dass diese Funktionen von Portalen ausgelöst werden. Dort finden die Interaktionen mit den Benutzern statt. Die wollen etwas vom System; das System erfüllt ihre Wünsche, wenn eine dafür zuständige Funktion im System angestoßen wird. Für die Übersetzung der Benutzerwünsche in Funktionsaufrufe sind Portale zuständig. Nur weil Sie aber Benutzerrollen mit zugehörtigen Portalen identifiziert haben, können Sie gewöhnlich nicht „einfach so“ eine Liste der „Triggerfunktionen“ aufstellen. In nichttrivialen Softwaresystemen sind das Hunderte oder Tausende. Deshalb bietet Slicing eine schrittweise Annäherung. Solange Ihnen nicht klar ist, welche „Triggerfunktionen“ nötig sind – und also die Anforderungsinkremente noch zu groß sind für einen Start in die Produktionscodeentwicklung –, können Sie nicht anders, als das Softwaresystem gröber zu zerlegen als in Funktionen. Wie grob, das müssen Sie einschätzen. Ihr Bestreben ist es, so schnell wie möglich an die Ebene „Entry Point“ zu kommen. Wenn ein Sprung dahin nicht vom System mit seinen Portalen möglich ist, überlegen Sie von unten nach oben, ob Sie sich dafür bereit fühlen:

„Wenn ich noch nicht weiß, welche konkreten Entry Points es geben soll, fühle ich mich dann zuversichtlich, zumindest die Interaktionen der Benutzer mit dem Softwaresystem zu sammeln?“

„Wenn ich noch nicht klar sehe bei den Interaktionen, kann ich mir wenigstens schon einen Überblick der Dialoge mit ihren Verbindungen verschaffen?“
„Wenn ich die Dialogebene noch überwältigend finde, kann ich darüber strukturieren, indem ich verschiedene Perspektiven identifiziere, durch die Benutzer das System sehen und mit ihm interagieren?“

Und so weiter.

Sehen Sie, wie Sie durch die Ebenen geleitet werden und aufsteigen bis dorthin, wo Sie sich nach einem ersten Systemüberblick schon wohl fühlen? Sie steigen dort in die Hierarchie ein und arbeiten sich nach unten vor, wo Sie meinen, genügend Informationen zu haben. Es gibt keinen Zwang, immer alle Ebenen zu betrachten. Ihre Expertise und Ihr Verständnis der Anforderungen angesichts einer gewissen Systemkomplexität bestimmen, welche Ebenen der Slicing-Hierarchie für Sie hilfreich sind. Die Hierarchie ist nur so tief, um Ihnen möglichst viele Anlaufpunkte zu geben. Für jeden Grad der Klarheit soll etwas dabei sein. Wichtig zu erinnern: Jede Ebene hat Bezug zu technischen Strukturen; es gibt ein Mapping, das Ihnen die Übersetzung in Code erleichtern soll. Jede Ebene steht auch in einem Zusammenhang: Sie wissen genau, wie Sie anschließend fortfahren. Die Hierarchie ist ein Handlauf – in beide Richtungen. Bei Klarheit bewegen Sie sich nach unten, bei Unklarheit steigen Sie zunächst auf, bis Sie sich eine Zerlegung in übersichtlichere Teile wieder zutrauen.

Zusammenfassung

Natürlich wollen Sie so schnell wie möglich damit beginnen können, Produktionscode zu schreiben, um Anforderungen zu erfüllen. Doch Anforderungen sind meistens sehr umfangreich und auch noch unklar. Sie brauchen daher ein Vorgehen, das diese Komplexität bei den Hörnern packt. Mit einem iterativ-inkrementellen testgestützten Vorgehen tun Sie das. Wie aber finden Sie Inkremente? Wann wissen Sie, dass ein Inkrement eine Mindestqualität hat? Im Slicing ist die Mindestqualität dadurch definiert, dass Sie genau wissen, welche eine „Triggerfunktion“ an der Wurzel der Produktionscodehierarchie steht, die das Verhalten eines Slices herstellt. Zu diesen „Triggerfunktionen“ können Sie allerdings nicht „hinspringen“. Vielmehr müssen Sie sie durch eine schrittweise Verfeinerung der Anforderungen erarbeiten. Dazu leitet Sie die Slicing-Hierarchie an. Sie bietet Ihnen für jedes Gefühl von Klarheit einen Level, auf dem Sie mit einer weiteren Verfeinerung einsteigen können. Ausgangspunkt ist immer das gesamte Softwaresystem in seiner Umwelt. Erst wenn Sie darüber den Überblick haben – also insbesondere die Benutzer mit ihren Portalen identifiziert haben –, kennen Sie Ansatzpunkte für einen Abstieg durch die Anforderungen in Richtung „Triggerfunktionen“.
