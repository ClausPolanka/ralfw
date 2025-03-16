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


ANFORDERUNGSANALYSE FÜR ENTWICKLER, TEIL 3

Das Messer richtig ansetzen

Feine Schnitte durch die Anforderungen von der Interaktion bis zur CQS-Funktion.

Ziel der Anforderungsanalyse für Sie als Entwickler ist, Ihnen einen glasklaren Ansatzpunkt für den darauf folgenden Entwurf und dann die Codierung zu geben. Sie müssen nicht nur das Gefühl haben, die Anforderungen zu verstehen, sondern auch die Möglichkeit, den Code zu überprüfen, der daraus entstanden ist. Hat Ihr Gefühl guten Verständnisses Sie getrogen, oder war es berechtigt? Eine solche Überprüfung kann nur skalierbar mit automatisierten Tests erfolgen. Allein mit ihnen können Sie sowohl zügig die Reife des Codes in ganzer Breite für die aktuelle Anforderung feststellen („Habe ich die Anforderung schon erfüllt?“) wie auch die Stabilität des Codes in Bezug auf alle vorher schon erfüllten Anforderungen („Werden alle anderen Anforderungen immer noch erfüllt?“). Automatisierte Tests erfordern als Ansatzpunkt eine Funktion und Testfälle. Deshalb bin ich davon überzeugt, dass das Ziel jeder Anforderungsanalyse eine Liste von Funktionen mit präziser Signatur, aufgedeckten Abhängigkeiten von Zustand beziehungsweise Ressourcen und Testfällen sein muss. Wenn Sie also mit einem Product Owner (PO) oder Kunden über Anforderungen sprechen, haben Sie immer im Blick, dass Sie die Frage beantwortet bekommen: „Welche Funktion soll ich implementieren?“

Diese Funktion nenne ich auch Triggerfunktion, denn sie wird von Anwendern der Software im Rahmen ihrer Interaktion durch eine Benutzerschnittstelle angestoßen. Anwender wünschen sich von Software ein gewisses Verhalten. Dazu „reizen“ sie die Software „an der Oberfläche“ und erwarten eine Reaktion: Input-Daten werden in irgendeiner Form eingegeben, eine Funktion wird ausgelöst, Output-Daten werden angezeigt. Jede Software ist ganz grundsätzlich in dieser Weise aufgebaut, vergleiche Bild 1. Dabei ist es unwesentlich, ob ihre Nutzer Menschen oder andere Software sind.

User stellen an Software vermittels deren Frontend eine Anfrage (Input) und bekommen von der Software ein Ergebnis (Output). Ob das Frontend konsolenbasiert ist, ein GUI hat, eine HTML-Seite ist oder einen REST-Endpunkt veröffentlicht, ist einerlei.

Das Frontend ist der Vermittler zwischen dem User in der Umwelt der Software und „der Maschinerie“ in der Software, die für ihn die wahre Blackbox ist. Dort wird das gewünschte Verhalten erzeugt. Das Frontend übersetzt dazu die Anfrage in einen Request für „die Maschinerie“ und deren Response in etwas Verständliches für den User.

Jedes Frontend besteht in Bezug auf die Input-Output-Paare, die es akzeptiert, jeweils aus einem Teil, der die Input-Daten vom User sammelt (collect) und in einen Request für die eigentliche Verarbeitung übersetzt, sowie einem Teil, der deren Response zum User hin in verständlicher Weise projiziert (project). Sammlung und Projektion kompensieren die Unfähigkeit beziehungsweise den Unwillen von Usern, direkt mit verarbeitbaren Request-Response-Daten-strukturen umzugehen. Menschliche Benutzer wollen strukturierte Eingabefelder für ihren Input haben und nicht JSON-Datenstrukturen formulieren; sie wollen auch nicht YAML interpretieren, sondern zum Beispiel eine 3D-Grafik mit den produzierten Daten sehen.

Der von der Sammlung zusammengestellte Request wird an eine Funktion im Inneren der Software zur Verarbeitung (process) übergeben, die anschließend mit einem Response antwortet. Es kann sein, dass diese Funktion der Blackbox auf internen oder externen Zustand (state) lesend und/ oder schreibend zugreift.

Wie Sie sich denken können, ist die Funktion, die ich mit Triggerfunktion bezeichne, in Bild 1 process(). Wenn sie aufgerufen wird, sind die Feinheiten der Request-Sammlung abgeschlossen, bei der die Frontend-Technologie eine Rolle spielt, die schlecht automatisiert zu testen ist. Und die Feinheiten der Projektion von Ergebnissen für den User mittels Frontend-Technologien, die schlecht zu testen sind, spielen auch für sie keine Rolle; sie sind nachgelagert in der Interaktion.

In Bezug auf die im zweiten Teil der Artikelserie [1] vorgestellte Slicing-Hierarchie sind damit drei Ebenen im Blick (Slicing Part 3 Bild 2):

Im Gesamtsystem ist verortet, dass und wo Benutzer mit ihm umgehen. Das geschieht immer durch einen Adapter, der Portal genannt wird. Dieses Portal kapselt die Details von Sammlung und Projektion. Es ist strikt vom Code zu trennen, der die eigentliche Arbeit der Transformation von Input in Output übernimmt.

Jede Input-Output-Wandlung findet im Rahmen einer Interaktion statt, die innerhalb eines Portals verortet ist.

Und jede Interaktion triggert eine Funktion, den Entry Point für die Verarbeitung des Inputs, der in Form eines Requests angeliefert wird.

Die drei Ebenen sind in jedem Softwaresystem im Spiel. In jeder Anforderungsanalyse ist es mithin zentral, Folgendes zu identifizieren:
Wer sind die Benutzer des Systems, und über welche Art von Frontend wollen sie mit ihm interagieren? Die Antwort besteht aus einer Anzahl von Portalen mit zugehörigen Technologien.

Welche Interaktionen sollen zwischen Benutzern und System ablaufen? Wie wollen User „das System reizen“, um ihm das gewünschte Verhalten zu entlocken? Die Antwort besteht aus einer Anzahl von technologiespezifischen Frontend-Elementen.

Welche Form soll die in jeder Interaktion getriggerte Funktion haben? Die Antwort besteht mindestens aus einer Funktionssignatur. Um automatisierte Tests dort ansetzen zu können, muss allerdings auch geklärt sein, welcher Zustand wie von der Funktion gebraucht wird. Außerdem sind dazu passende Testfälle zu sammeln. Hier geht es um die Entry Points.

Interaktionen

Auch wenn eine Hierarchie die Entwicklung des Verständnisses von einem Ende her nahelegt – entweder top-down oder bottom-up –, möchte ich für die Erkundung der Slicing-Hierarchie anders vorgehen. Die oberste Ebene des Big Picture hat der vorherige Artikel [1] beschrieben: Dort oben geht es ums ganze Softwaresystem (Slicing Part 3 Bild 3).

Im Sinne einer Outside-in-Analyse soll zunächst geklärt werden, welcher Kontaktpunkte mit der Umwelt es bedarf. Denn jedes Softwaresystem hat nur einen Zweck in Bezug auf seine Umwelt, das heißt für seine User. Ihr Bedarf treibt Verhalten und Struktur im Inneren des Softwaresystems. Immer. Deshalb Outside-in-Analyse.

Darüber hinaus kann jedes Softwaresystem diesen Bedarf nur erfüllen, wenn es Zugriff auf alle nötigen Ressourcen in der Umwelt hat. Diese Abhängigkeiten müssen also auch von Anfang gesammelt werden.

Wie es nach einer solchen initialen Analyse weitergeht, hängt allerdings von der Komplexität eines Softwaresystems ab. Kann nur ein kleiner Schritt gemacht werden, weil die Anforderungen so umfangreich sind? Oder kann in der Hierarchie tiefer gesprungen werden, weil schon Klarheit herrscht? Für mich ist ein Kriterium für die Sprungtiefe, inwiefern ich wichtige beziehungsweise interessante Entry Points erkennen kann. Liegen die auf der Hand, muss ich mich nicht zwangsläufig durch Context, App und so weiter nach unten graben. Aus diesem Grund springe ich auch mit der Beschreibung der Slicing-Hierarchie nun über einige Ebenen hinweg zu den Interaktionen. Zu denen haben Sie sofort einen Bezug; damit kann ich Sie hoffentlich auf den Haken nehmen für eine spätere Reise zu höherliegenden Ebenen. 

Interaktionen sind das sichtbare und technologische Pendant zu Entry Points. Wo ein Entry Point immer nur eine Funktion darstellt, die nur Sie als Entwickler interessiert, ist eine Interaktion etwas, das für den Benutzer unmittelbar relevant ist. Er kann sie sehen und spüren. Ein Mausklick, ein Tastendruck, eine Geste kann eine Interaktion starten, die durch eine Reaktion der Software mit einer Ausgabe in Form von Text, Bild, Ton oder auch noch anderen Modalitäten endet. Die unmittelbar einleuchtende Frage an POs, Benutzer und Kunden während der Anforderungsanalyse ist deshalb: „Welche Interaktionen wünscht ihr euch denn?“ Leider ist die Antwort auf diese Frage meistens unmöglich. Interaktionen in nicht trivialer Software sind einfach so zahlreich. Sie gehen in die Hunderte, gar Tausende (Slicing Part 3 Bild 4). Sie lassen sich nicht aus dem Stand aufzählen. Selbst ein kleines Beispiel macht schon deutlich, wie schwierig es ist, sie zu identifizieren. 

Als Szenario mag eine Aufgabenverwaltung dienen:

Aufgaben sollen in Listen angelegt werden können.

Wenn eine Aufgabe überfällig ist, soll daran erinnert werden.

Zählen Sie jetzt einfach nur die Interaktionen auf, die Ihnen dazu einfallen. Eine Interaktion ist eine Antwort auf die Frage: Wann soll was passieren? Welche Interaktionen stehen in Ihrer Liste? Ich komme nach der sehr knappen Anforderungsbeschreibung mindestens auf diese:

Liste einrichten
Liste umbenennen
Liste löschen
Listen anzeigen
Liste auswählen, um sie mit ihren Aufgaben anzuzeigen
Aufgabe einrichten in einer Liste
Aufgabe bearbeiten
Aufgabe löschen
Benachrichtigen über überfällige Aufgabe

Das sind vor allem grundlegende CRUD-Interaktionen. Nicht spannend, da steckt keine Kreativität drin – dennoch darf keine vergessen werden und der Code dazu muss fehlerfrei sein. Aber es gibt noch weitere Interaktionen:

Programm starten
Aufgabe in einer Liste verschieben
Liste in der Liste der Listen verschieben
Programm beenden

Was noch? Ist Ihnen noch etwas eingefallen? Nein? Das bedeutet nicht, dass es nicht weitere Interaktionen gibt. Welche das sind, kann nur ein Gespräch mit dem Kunden oder PO ergeben. Worin unterscheidet sich das von der üblichen Anforderungsanalyse? Im Ziel! Ziel des Gesprächs ist immer eine Liste von Triggerfunktionen, denn diese stehen hinter den Interaktionen. Deshalb sollte nach meiner Ansicht jede Anforderungsanalyse das Ziel haben, dem PO die Liste der Interaktionen zu entlocken. Auf welchem Weg auch immer.

Hier sind typische Fragen, mit deren Hilfe Sie das Gespräch leiten können:

„Welche Buttons möchtest du klicken können?“
„Welche Menüpunkte brauchst du?“
„Sollen Tastendrücke bei Eingabe der Daten das Verhalten auslösen?“
„Wann und wie soll Verhalten X eigentlich ausgelöst werden?“
„Kommt es zu Verhalten Y durch einen ‚Reiz‘ des Anwenders oder automatisch?“

Alle laufen darauf hinaus, dass der PO ganz konkret werden muss. Das ist wichtig! Nur wenn er konkret wird, kann es Klarheit für Sie geben. Je umfangreicher die Anforderungen, desto weniger kann ein PO allerdings dazu früh in der Analyse sagen. Deshalb die Slicing-Ebenen oberhalb der Interaktionen. Doch dazu später mehr. 

Manchmal ist die Entscheidung, ob ein Verhalten durch Klick auf einen Button oder Menüpunkt ausgelöst werden soll, natürlich nicht so wichtig. Vor allem geht es darum, die Auslösung des einen Verhaltens vom anderen zu unterscheiden. Insofern ist es auch nicht immer wichtig, sofort die finale Benutzerschnittstelle zu kennen. Es kann auch mit einer prototypischen Benutzerschnittstelle angefangen werden. 

Wieder das Beispiel Aufgabenverwaltung: Nach Klärung der Interaktionen wird das Verhalten implementiert, doch die Benutzerschnittstelle ist lediglich in der Konsole. Dort ist sie sehr einfach und ohne spezielle UI-Technologiekenntnisse zu implementieren. Die Interaktionen werden durch Kommandoeingaben angestoßen (Slicing Part 3 Bild 5). Mit einem solchen Frontend-Prototyp fällt die Konzentration auf das Wesentliche leichter: die Domänenlogik. Sie steht hinter den Triggerfunktionen. Später kann das Frontend – Sammlung und Projektion – ausgetauscht werden, ohne den getesteten Code hinter den Triggerfunktionen anfassen zu müssen. 

Es kann am Produktionsfrontend auch parallel zur Verhaltenserzeugung durch die Triggerfunktionen gearbeitet werden, wenn die Entry Points präzise definiert sind. Das Vorgehen mit Slicing wirkt auf diese Weise produktivitätsfördernd: In Arbeitsteilung können unabhängige Bereiche unabhängig vorangetrieben werden.

Bei Webanwendungen passiert das in gewisser Weise selbstverständlich durch die Schnittstelle zwischen Frontend (HTML/JS) und zum Beispiel REST-Backend. Ich plädiere jedoch dafür, auch innerhalb eines solchen Frontends noch genauer hinzuschauen. Mehr dazu, wenn wir auf die Worker-Ebene der Slicing-Hierarchie schauen. 

Ab einem gewissen Punkt wird das vereinfachte UI natürlich auch umständlich. Seien Sie sensibel für die Grenze zwischen Nutzen und Nachteil. Vor allem wollte ich dadurch klarmachen, welche Optionen es eröffnet, wenn die Anforderungsanalyse sich von vornherein auf die Herausarbeitung von Interaktionen konzentriert:

Der PO wird zu Klarheit gezwungen.

Ein beteiligter UX-Designer beziehungsweise UI-Technologiespezialist kann bei der Entscheidung helfen, damit optimale Interaktionen und dahinter Triggerfunktionen entstehen.

Sie als Entwickler bekommen bestmögliche Startpunkte für Entwurf und Implementation von Code.

Ein weiterer zu berücksichtigender Punkt bei der Entwicklung der Interaktionen – denn nicht weniger ist es als eine Entwicklung; Interaktionen müssen aus den Vorstellungen über das Softwareverhalten herausgeschält werden – ist der Zustand des Frontends. Hält das Frontend selbst Zustand, oder bekommt es immer alle nötigen Daten von den Triggerfunktionen? 

Schauen Sie Bild 5 erneut an. Aus der Systemanalyse ist klar geworden, dass es mindestens zwei Codeteile gib, nämlich das Frontend und den Rest, die Blackbox dahinter, die getriggert wird. 

Nach dem Kommando lc Liste 2 zeigt es die Liste der bisher angelegten Listen für Aufgaben an. Welcher der beiden Codeteile kennt diese Liste? Es ist anzunehmen, dass die Blackbox die Listen kennt und verwaltet, wahrscheinlich sogar persistent. Wie soll dann die Triggerfunktion aussehen? 

Zwei Varianten als Beispiel:

list_create(name:string):List

list_create(name:string):List[]

Eine Diskussion, ob List ein Domänendatentyp ist oder nur einer für die Schnittstelle zwischen Frontend und Blackbox, erspare ich Ihnen an dieser Stelle. Mir geht es darum, ob nur die neu angelegte Liste oder alle Listen zurückgegeben werden sollen. Wenn es immer alle sind, benötigt das Frontend keinen eigenen Zustand, um als Reaktion auf lc alle Listen aktualisiert zu zeigen. Wenn hingegen nur die neu angelegte zurückkommt, muss es sich zumindest die Namen aller anderen Listen gemerkt haben, um sie anzeigen zu können. 

Ich will hier keiner der Varianten den Vorzug geben. Manchmal ist nur eine möglich, manchmal liegt eine nahe. Welche Variante ist für die eine Interaktion günstig, welche für die andere? Wichtig ist vor allem, dass im Rahmen der Interaktionsentwicklung genau darüber gesprochen wird. Welche UI-Technologie soll genutzt werden, welche Herangehensweise unterstützt sie? Zumindest Sie als Entwickler müssen sich darüber Klarheit verschaffen. Davon hängt die Form der Triggerfunktion ab.

Entry Points

Interaktionen finden dort statt, wo Anwender Ihre Software „reizen“. Heutzutage geschieht das oft im Rahmen von Events, die von Eventhandlern bedient werden. Das ist seit Visual Basic 1.0 (VB) so. Der Erfolg von VB ist aus meiner Sicht zu einem guten Teil darauf zurückzuführen, dass die Integration der Benutzerschnittstellengestaltung mit der Codierung des Verhaltens so eng war. Wer wusste, wie das UI aussah, hatte sofort eine simple Grundstruktur für den Code: Jedem Button, jedem Menüpunkt und so weiter stand sofort eine Funktion gegenüber, in die Logik „gekippt“ werden konnte, die das getriggerte Verhalten erzeugte. Das war der Hit! Dass das leider nicht auto- matisch zu sauberem Code führte, mussten Entwickler erst leidvoll lernen. Deshalb muss ich noch einmal klarstellen: Mit Entry Points meine ich keine Eventhandler einer Benutzerschnittstellentechnologie! Eventhandler-Funktionen sind technologiespezifisch und gehören zum Portal als Adapter für die Verbindung zwischen der Umwelt und der Innenwelt des Softwaresystems.

Eventhandler lassen sich nicht gut testen. Bei einer echten Trennung von „Model“ wie bei WPF sieht es besser aus, dennoch rechne ich zum Beispiel ein dortiges Command immer noch zur UI-Technologie. Darin darf nicht die Musik spielen, die automatisiert getestet werden muss. Triggerfunktionen sind mithin Funktionen, die getrennt von Eventhandlern existieren. Sie gehören „zum Rest“ der Software und könnten von Eventhandlern aufgerufen werden (auch wenn ich eine andere Anbindung vorziehe; doch das ist ein Detail). Im Slicing sähe ein Eventhandler wie in Bild 6 aus: Er kümmert sich um die Sammlung von Input, um den Request zu schnüren. Er ruft die Triggerfunktion. Er kümmert sich um die Projektion des Response vom Entry Point. Bei HTTP-Endpunkten sind Sammlung und Projektion oft trivial, weil sie von der Infrastruktur übernommen werden. Das können Sie dankend annehmen. Doch der Controller bleibt ein Adapter, der Verhalten nicht selbst erzeugt, sondern dafür den eigentlichen Entry Point aufruft. Was unterscheidet den Begriff Triggerfunktion von Entry Point (Slicing Part 3 Bild 7)? Es geht um dieselbe Funktion. Im Rahmen der Diskussion um die Interaktionen ist diese Funktion aber noch relativ schwammig. Vor allem ist wichtig zu verstehen, dass es sie gibt und mit welchen Daten sie grundsätzlich arbeitet:

Es muss einen Request aus einer Input-Sammlung geben.
Es wird ein Response geliefert.
Es ist vielleicht Zustand/ein Seiteneffekt im Spiel.

Bei der Ebene des Entry Points darunter geht es um die Verfeinerung dieser Funktion: Wie genau sieht die Signatur aus? Welche Daten sollen in welcher Struktur in den Entry Point hineingegeben werden, welche kommen am besten heraus, welche braucht er woher darüber hinaus, und auf welche hat er welchen Einfluss? Erst wenn das geklärt ist, können auch Testfälle bestimmt werden.

Auf der Ebene der Entry Points wird es wirklich relevant, dass Sie sich Gedanken über die Zustandshaltung machen: Frontend oder Blackbox? Dies hat Einfluss auf die Signatur des Entry Points, wie oben gezeigt. Ich tendiere dabei zu besserer Testbarkeit. Meine Frage ist: Wie kann ich mehr Code leichter testen? Code im Frontend ist tendenziell schwerer zu testen, also versuche ich ihn zu minimieren. Wenn also Zustandshaltung im Frontend Aufwand bedeutet, ziehe ich den Code lieber in die Blackbox, wo er unabhängig von Frontend-Technologie testbar ist. Wenn die Zustandshaltung im Frontend hingegen trivial ist, deklarativ beziehungsweise intentional funktioniert und keinen (testwürdigen) Code braucht, dann lasse ich Zustand auch mal im Frontend. Das erspart mir Datenverkehr mit der Blackbox. 

Die Entry Points können sich mehr auf das Wesentliche konzentrieren. Wenn zum Beispiel die Listen einer Aufgabenverwaltung in einer GUI-Liste einer Desktop-Anwendung angezeigt werden sollen, erübrigt sich eine Zustandshaltung in der Blackbox jenseits der zu leistenden Persistenz. Auch hier ist wieder zu sehen, wie UI/UX-Entscheidungen nach innen wirken. 

Die Blackbox, also den Kern eines Softwaresystems, gänzlich unabhängig von der Umwelt, von den Interaktionen, von den Frontend-Technologien zu halten, ist eine durchaus schädliche Optimierung. Natürlich soll die konkrete Nutzung von UI-Frameworks in Portalen gekapselt sein – doch deren Paradigma darf nach innen wirken Das muss allerdings eine bewusste Entscheidung sein. Denn je mehr die äußere Form nach innen wirkt, desto abhängiger ist das Innen natürlich. Die äußere Form zu wechseln mag dadurch erschwert werden. Hier ist eine Balance zwischen Effizienz und Flexibilität zu finden..

Oben habe ich eine Anzahl von möglichen Interaktionen einer Aufgabenverwaltung gelistet. Zu jeder gehört eine Triggerfunktion. Hier nochmal ein Auszug:

Liste einrichten: list_create()
Liste umbenennen: list_rename()
Liste löschen: list_delete()
Listen anzeigen: lists_all()
Programm starten: program_start()

Das ist trivial und bedarf eigentlich keiner Dokumentation oder sogar Erwähnung. Das haben Sie immer im Hinterkopf. Nur die letzte Interaktion lässt fragen, ob sich eine eigene Triggerfunktion lohnt oder vielleicht bei Programmstart lists_all() aufgerufen werden könnte. Um eine vorzeitige Optimierung zu vermeiden, würde ich allerdings dafür plädieren, zunächst mit program_start() zu planen. Vielleicht ergibt eine spätere Analyse, dass bei Programmstart noch mehr oder etwas anderes getan werden soll, als nur die Aufgabenlisten zu listen. 

Für den Level „Entry Points“ in der Slicing-Hierarchie ist nun etwas mehr Detail gefragt: Wie soll denn die Signatur ganz präzise aussehen? Ich zeige Ihnen einfach meine Entscheidungen und verzichte auf eine Diskussion, warum sie so ausgefallen sind. Es geht hier nicht darum, die absolut beste Signatur zu erarbeiten, sondern den Unterschied zwischen den Levels bei Slicing zu verdeutlichen.

Bild 8 zeigt die Entry Points für die obigen Triggerfunktionen, wie ich sie mir beispielhaft vorstelle. Sie sehen, ich habe mich dafür entschieden, das Frontend durchaus Zustand halten zu lassen. Jetzt sind alle Funktionen mit Parametern und Typen versehen. Zustand ist implizit. Die Sprache ist Typescript. Eine Klasse habe ich für die Blackbox nicht angelegt. Die Funktionen werden einfach von einem dateibasierten Modul exportiert. Ob das die beste Idee ist, lasse ich dahingestellt. Es soll nicht um die Feinheiten des Entwurfs und der Modularisierung gehen, sondern darum, dass die Entry Points eben eine Schnittstelle darstellen, die dem Frontend bekannt ist. Die Entry Points isolieren die Blackbox von der Umsetzung der Interaktionen mit dem Benutzer. Zu den konkreten Schnittstellen gehören natürlich auch Datentypen. List als Struktur für eine Aufgabenliste ist Teil der Schnittstelle, die die Entry Points definieren. Und was ist mit den persistenten Daten?

Die persistenten Daten sind nicht Teil der Schnittstelle. Von ihnen eine Idee zu haben ist nicht falsch. Sicher werden sie in diesem Fall nah an List sein. Doch ich denke, für die Formulierung von Testfällen, die ja auch noch fehlen, sind die persistenten Daten gar nicht so wichtig. Überhaupt ist Persistenz nicht entscheidend, um ein Frontend an eine Blackbox mit dieser Oberfläche zu binden. Sie muss nur zusichern, den Zustand korrekt zu bewahren. 

Jeder Entry Point könnte einzeln getestet werden. In diesem Fall glaube ich jedoch, dass ein „Szenariotest“ einfacher ist. So nenne ich einen Test, der mehrere Funktionen im Verein auf die Probe stellt. Ein Szenariotest simuliert eine Folge von Interaktionen, zum Beispiel

Das Programm wird gestartet; noch gibt es keine Listen.
Es wird eine Liste hinzugefügt.
Die Listen werden alle abgerufen.
Es wird noch eine Liste hinzugefügt.
Die Listen werden wieder abgerufen.
Eine Liste wird umbenannt.
Eine andere Liste wird gelöscht.
Die Listen werden wieder abgerufen.

Zu jedem Schritt lassen sich Erwartungen formulieren. Bild 9 zeigt alle Entry Points in einem solchen Zusammenspiel. Dass die Implementation dahinter derzeit nur In-Memory Zustand hält, ist für das Prinzip, um das es mir hier geht, unerheblich. Die Anforderungen des Beispiels haben es erlaubt, schnell auf eine ganze Reihe von Interaktionen mit ihren Entry Points zu kommen. Manchmal ist das möglich. In anderen Fällen ergeben sich für zum Beispiel eine User Story mehrere Interaktionen, doch die können nicht alle näher betrachtet werden (breadth-first), sondern auf eine der Interaktionen ist besonders die Aufmerksamkeit zu lenken (depth-first), um für sie Signatur, Zustand/Seiteneffekte und Testfälle zu definieren. Wie sehr sie in die Breite gehen, ist auch für die Methode unerheblich. 

Letztlich sollen Sie bei einem Entry Point ankommen, dessen Ausgestaltung Ihnen erlaubt, dahinter mit Entwurf und Codierung zu beginnen. Und dann ist der nächste Entry Point dran, und danach der nächste und so weiter. Der Weg zu den Entry Points für die kleine Beispielsoftware bestand aus drei Sprüngen in der Slicing-Hierarchie (Slicing Part 3 Bild 10):

Der Einsprung erfolgt für die System-Ebene. Dort geht es immer los: Überblick gewinnen, Frontends identifizieren.

Da das Beispiel so klein ist, sind wir sofort zur Interaktion-Ebene gesprungen. Wir konnten alle Interaktionen ohne weiteres aufzählen.

Für jede Triggerfunktion hinter den Interaktionen haben wir dann den Sprung zur Entry-Point-Ebene gemacht.

Sprünge über Ebenen hinweg sind okay, wenn das, was auf der nächsten Ebene gesucht wird, auf der Hand liegt. Das war bei den Interaktionen für die Aufgabenverwaltung klar. Zu den Entry Points weiterzugehen war der nächste nötige Schritt. Was aber nun? Können wir schon mit den Entry Points zufrieden sein? Wenn die dahinterstehende Funktionalität überschaubar ist, ja. Dann einfach umsetzen. Und was, wenn sie immer noch unhandlich groß ist? Dann gilt es, eine Ebene tiefer zu steigen.

Command Query Separation (CQS)

Wie können Entry Points formal (!) weiter zerlegt werden? Was wäre für Sie als Entwickler ein relevanter struktureller (!) Schnitt? Ich denke, das Prinzip der Command Query Separation von Martin Fowler [2] ist hier hilfreich. Mit ihm kann Funktionalität zumindest auf einen Aspekt der Zustandsveränderung fokussiert werden:

Command: Eine Funktion verändert Zustand und liefert kein Resultat zurück. Oder wenn ein Resultat geliefert wird, dann sind es Metadaten zur Aktivität, zum Beispiel die Zahl der gelöschten Datensätze oder die ID eines neu angelegten Datensatzes. Bei REST entspricht dem ein POST-Request. Ein Command kann inhaltlich fehlschlagen, weil zu verändernde Daten nicht im erwarteten Zustand sind.

Query: Eine Funktion verändert keinen Zustand und liefert ein Resultat zurück, das auch auf Zustand basieren kann. Dem entspricht bei REST ein GET-Request. Queries können inhaltlich nicht fehlschlagen.

Die Unterscheidung von Veränderungen und Abfragen hat mehrere Vorteile:

Getrennte Testbarkeit dieser unterschiedlichen Weisen des Umgangs mit Daten.

Technologische Trennung dieser Modi, zum Beispiel kann ein Command Locks erfordern, Queries aber nicht.

Wiederverwendbarkeit in unterschiedlichen Zusammenhängen wegen feingranularerer Funktionen.

Durch diese Brille betrachtet sind Entry Points oft nicht eindeutig. Sie bieten Potenzial für eine Verfeinerung, das heißt Verkleinerung des Scopes durch Konzentration auf Command versus Query. Wie steht es in dieser Hinsicht mit den bisherigen Entry Points der Aufgabenverwaltung? Bild 11 zeigt, dass das Urteil nur in drei von fünf Fällen eindeutig ist:

list_delete() ist ein Command, da Zustand verändert wird – eine Liste wird gelöscht –, aber keine Daten zurückgeliefert werden. Die Annahme ist, dass die per id identifizierte Liste existiert.

lists_all() und program_start() sind beide Queries. Sie fragen lediglich die Liste der Listen ab und liefern Einträge zurück.

Sowohl list_create() wie auch list_rename() hingegen sind keine reinen Kommandos, auch wenn das durch die Benennung suggeriert wird. Der Widerspruch zum CQS besteht in ihren Responses, die die veränderten Daten zurückliefern. Das ist naheliegend, jedoch streng genommen nicht erlaubt. 

Wie reagieren Sie darauf? Das Wichtigste ist, diesen Widerspruch überhaupt entdeckt zu haben. Jetzt können Sie eine bewusste Entscheidung treffen, ob sie diese beiden Aspekte gekoppelt lassen oder sie aufdröseln. 

In manchen Fällen kann es bleiben, wie es ist, in anderen lohnt sich eine Trennung. Insbesondere, wenn die zurückzuliefernden Daten schon bei der Kommandoausführung ganz natürlich „anfallen“ und nicht zu umfangreich sind, wäre es dumm, sie nicht gleich zurückzuliefern. Das erspart dem Aufrufer einen zweiten Zugriff, um sie zu beschaffen. Aber, wie gesagt, es kommt auf den Einzelfall an. Hier ist jedenfalls Potenzial für feinere Schnitte. Wie könnten diese aussehen? Ein Blick hinter die Kulissen der Implementation, die ich schon (vorschnell) vorgenommen hatte, um die Szenariotests oben vorzustellen, zeigt Potenzial für die Trennung: Es gibt nicht nur einen Widerspruch zu CQS, sondern auch zum Prinzip Don’t Repeat Yourself (DRY) (Slicing Part 3 Bild 12). In zwei Funktionen wird eine Liste über ihre ID herausgesucht. Wenn diese Logik in eine eigene Funktion wandert, kann dem CQS Genüge getan werden (Slicing Part 3 Bild 13):

list_rename() und list_delete() können sich besser auf ihre eigentliche Aufgabe konzentrieren.

Aufrufer von list_create() und list_rename() können sich die bisher zurückgelieferte Liste über einen weiteren Entry Point list_get() beschaffen. Alle Entry Points entsprechen nun dem CQS:

list_delete() hat sich am wenigsten verändert. Die Funktion sucht nur einfach nicht mehr selbst die zu löschende Liste heraus.

list_rename() delegiert die Beschaffung der umzubenennenden Liste nun auch – liefert sie allerdings nicht mehr zurück. Die Funktion ist damit zu einem klaren Command geworden.

list_create() retourniert jetzt nicht mehr die neue Liste, sondern nur deren ID. Auch hier liegt jetzt ein reinrassiges Command vor.

)
list_get() ist der neue Query Entry Point zur Beschaffung einer einzelnen Liste. Er kapselt die interne Methode, die noch sowohl Liste als auch Index liefert. Auf diese Weise konnten die Bedürfnisse beim Umbenennen und Löschen mit einer Funktion befriedigt werden.

Im Szenariotest ist diese Beschaffung natürlich hinzuzufügen (Zeile 12 im rechten Ausschnitt in Slicing Part 3 Bild 13). Aber wird dadurch nicht auch etwas mehr Klarheit hergestellt? Solange nicht deutlich spürbare Performance-Verschlechterungen dagegen sprechen, ziehe ich ein Slicing nach CQS unterhalb der Entry Points vor. Ich möchte jeden Entry Point so dünn geschnitten haben, wie es möglich ist. 

Allerdings … ich mag auch die Bequemlichkeit, die ein hybrider Entry Point wie zum Beispiel das ursprüngliche list_rename() geboten hat. Deshalb finde ich es okay, wenn Sie ganz bewusst nach einer Verfeinerung entlang des CQS die entstandenen Entry Points zu einem Convenience Entry Point zusammenfassen (Slicing Part 3 Bild 14). In dem kommen Command und Query (auch mehrere) zusammen, um eine abstraktere Funktion zu bilden. Die muss auch eigentlich nicht mehr separat getestet werden, falls die darin vereinten Entry Points separat getestet wurden; dass Ihnen bei der Integration von zwei oder drei Entry Points ein Fehler unterläuft, ist kaum zu erwarten.

Zwischenstand

Slicing leitet in kleinen Schritten durch eine für Sie als Entwickler relevante Hierarchie von Inkrementen. Alles beginnt immer beim kompletten Softwaresystem – doch das Ziel ist am Ende die einzelne CQS-fokussierte, testbare Funktion (Slicing Part 3 Bild 15), die unabhängig von Frontend-Technologien ist. Hinter ihr steht die Erzeugung von Verhalten mit einem beliebig tiefen Baum weiterer Funktionen, die die Logik enthalten, um gegebenenfalls unter Rückgriff auf Ressourcen die nötigen Datentransformationen durchzuführen, die der User wünscht.

Der Sprung vom Softwaresystem zu einer tieferen Ebene ist jederzeit möglich, wenn Sie fühlen, dass Sie auf der Zielebene Klarheit haben. Slicing ist ein Tool, um Ansatzpunkte für Ihren Softwareentwurf zu identifizieren, damit Sie möglichst geradlinig zu lauffähigem, korrektem Code kommen. Die Gestaltung der Benutzerschnittstelle ist dabei lediglich ein Hilfsmittel, falls die Entry Points für Sie noch nicht auf der Hand liegen. Deshalb hat sie in diesem Artikel noch keine Rolle gespielt.

Wenn Sie mit einer User Story oder einem Use Case konfrontiert sind, fragen Sie sich stets: „Weiß ich schon genug, um einen Entry Point zu formulieren?“ Wenn nein, bohren Sie hinein in die User Story. Brechen Sie den Use Case herunter. „Grillen“ Sie den PO. Lassen Sie ihn nicht ziehen, bevor Ihnen nicht die konkreten Interaktionen sonnenklar sind, in denen User das Softwaresystem triggern wollen. Interaktionen sind das, was Anwender wollen. Sie wollen das Softwaresystem so kontrollieren, dass es tut, was sie sich wünschen. Aber nicht nur, dass Software etwas Bestimmtes tun soll, sondern wie das konkret angestoßen werden soll, ist entscheidend. 

Die Granularität der Interaktionen ist bestimmend für die Form der Entry-Point-Funktionen. Bei der Anforderungsanalyse werden Sie deshalb den größten Teil der Zeit auf den Slicing-Ebenen ab Interaction verbringen. Insbesondere das Aushandeln der Testfälle benötigt viel Zeit. POs legen sich ungern fest. 

Um es Ihnen und POs in der Hinsicht einfacher zu machen, können Sie allerdings noch eine Ebene tiefer steigen als CQS. Features sind Inkremente innerhalb derer für Entry Points; die Funktionen sind definiert, dennoch kann der Scope feiner geschnitten werden. Das soll im nächsten Artikel geschehen.

ANFORDERUNGSANALYSE FÜR ENTWICKLER, TEIL 4

Die Benutzerschnittstelle treibt das Slicing

Die Struktur der Oberfläche gibt wertvolle Hinweise darauf, wo der Entwurf beginnen sollte. Klarheit gewinnen Sie besten durch eine schrittweise Annäherung.

Das Verhalten von Software wird durch Interaktionen mit dem Benutzer getriggert. Jeder Interaktion steht eine Funktion gegenüber, ein Entry Point. In HTTP-Servern wird das durch das Protokoll nahegelegt und ist mit MVC-Frameworks leicht umsetzbar. Slicing Part 4 Bild 1 zeigt ein sehr simples Beispiel dafür:

Die eigentliche Arbeit wird vom Code hinter der „Processor-Fassade“ verrichtet. Dessen Entry Points sind Wurzeln eines womöglich sehr tiefen Funktionsbaums, in dem das gewünschte Verhalten durch Logik [1] hergestellt wird.

Hinter der Grenze der Entry Points gibt es keine Abhängigkeit mehr von der Frontend-Technologie – hier ein MVC-Framework.

Um die Aufrufe der Entry Points herum sorgt das Frontend explizit oder implizit dafür, Daten vom Benutzer zu beschaffen (collect) beziehungsweise für ihn aufzubereiten (project). Beides ist abhängig von der Frontend-Technologie. In Slicing Part 4 Bild 2 sehen Sie ein Beispiel dafür.

Die Datensammlung vor dem Entry Point ist nötig, weil Benutzer Entry Points nicht direkt ansprechen können; sie sind ja keine Programmierer. Der collect-Schritt macht den Entry Point vermittels einer Frontend-Technologie zugänglich. Der project-Schritt leistet dasselbe für das Ergebnis des Entry Points; er rückübersetzt es in die Welt des Anwenders. Dass im Beispiel der Anwender eine andere Software ist, die Daten nicht „eingibt“, sondern über ein Protokoll sendet beziehungsweise empfängt, tut der Allgemeinheit dieses Verarbeitungsflusses keinen Abbruch. Genauso sieht es aus, wenn eine Software eine grafische Benutzeroberfläche hat. Die Entry Points werden in diesem Fall nur durch eine andere Technologie zugänglich gemacht. Sie als Softwareentwickler sind primär verantwortlich für die Herstellung des Verhaltens hinter Entry Points. Benutzerschnittstellen sind so betrachtet lediglich ein notwendiges Übel – machen dafür jedoch leider oft viel Mühe. Deshalb legt der Slicing-Ansatz für die Anforderungsanalyse so viel Wert darauf, die Entry Points einer Software herauszuarbeiten. Ab dort herrscht einfach Klarheit, und Sie sind in Ihrem Element. Sobald En­try Points mit Signatur und Testfällen definiert sind, kann ein Entwurf mit anschließender Codierung ernst- haft beginnen.

Jenseits des Trivialen ist es allerdings so, dass Software Dutzende, gar Hunderte von Entry Points hat, weil auf dutzendfache, gar hundertfache Weise mit ihr interagiert werden kann (Slicing Part 4 Bild 3). Angesichts dessen stellt sich die Frage, wie all diese Entry Points gefunden und ausreichend spezifiziert werden können. Denn eines ist klar: Sie lassen sich nicht einfach aufzählen. Sie können den PO nicht fragen: „Wie lauten die Entry Points, die du dir wünschst?“ Und selbst eine Frage nach allen Interaktionen läuft ins Leere.

Die Lösung kann nur darin bestehen, die Menge aller In- teraktionen mit ihren zugehörigen Entry Points zu untertei- len. Sogar mehrfach zu unterteilen, je nachdem, wie umfang- reich und unübersichtlich die Anforderungen sind. Im Slicing gibt es dafür eine konkrete Hierarchie (Bild 4), die orthogonal zu User Stories oder Use Cases ist. Orthogonal zu User Stories und Use Cases ist die Hierar- chie der Scopes beim Slicing deshalb, weil sich erstens die- se Scopes vertikal und horizontal in jenen verorten lassen (Bild 5). Und zweitens, weil diese auf technische Artefakte ab- gebildet sind, jene mit Technik jedoch nichts zu tun haben. User Stories und Use Cases bieten interessante Blickwin- kel auf Anforderungen. Aber sie sind nicht angebunden an die Realität der Arbeit eines Softwareentwicklers nach der Anforderungsanalyse. Sie lassen ihn ohne Ansatzpunkte für den darauf folgenden Entwurf und die Codierung zurück. Deshalb empfehle ich Ihnen, nach Gesprächen über User Stories und Use Cases das Slicing-Messer in die Hand zu nehmen und den Scope, der nach Arbeit mit diesen Werkzeu- gen abgesteckt wurde, feiner und feiner zu schneiden, bis Sie zumindest Entry Points herausgearbeitet haben (Bild 6). Slicing liefert einfach konkrete Strukturelemen- te für die Programmierung.

Applikationen

Wenn ein Softwaresystem zu groß ist, um gleich vom Gesamtüberblick – der sogenannten Softwarezelle – direkt bis hinunter zur Ebene der Interaktionen zu springen, dann können Sie sich dem System schrittweise annähern. Meine Empfehlung: Schauen Sie zuerst die Rollen an, die Sie als Nutzer des Systems identifiziert haben. Fragen Sie sich für jede Rolle: Wäre es für diese Rolle vorteilhaft, eine eigene Applikation anzubieten?

Mit Applikation meine ich eine „getrennte Software“, die der Benutzer zum Beispiel auf dem Desktop über ein eigenes Icon starten kann oder für die es einen eigenen URL gibt. Ein Beispiel sind Produktivitätsapplikationen wie E-Mail-Client, Kalender, Aufgabenverwaltung, Notizen und Adressenverwaltung. Alle diese Themen gehören auf hoher Abstraktionsebene zusammen (Slicing Part 4 Bild 7), und Microsoft hat mit Outlook auch alles unter einem Applikationsdach versammelt.

Doch es gibt auch noch einen anderen Weg, um mit der umfangreichen Anforderung „Produktivitätsunterstützung“ umzugehen. Sie können jeder Rolle mit ihren Anforderungen eine eigene Applikation geben. Das ist der Ansatz, den Sie auf iPhones ganz natürlich umgesetzt sehen (Slicing Part 4 Bild 8). Sobald Sie mehrere Rollen als Nutzer eines Softwaresystems identifiziert haben, müssen Sie entscheiden, ob Sie einen One-size-fits-all-Ansatz umsetzen oder lieber ein Best-of-breed-Angebot machen wollen:

One size fits all: Alle Rollen arbeiten mit derselben Anwendung. Die Anwendung integriert die Teil-Lösungen für die unterschiedlichen Rollen unter dem Dach einer Benutzerschnittstelle und auf der Basis einer Technologie.

Best of breed: Alle Rollen bekommen ihre eigenen Anwendungen. Die Anwendungen werden über Ressourcen, allgemeiner: über Standards integriert. Jede Anwendung kann eine optimale Benutzerschnittstelle anbieten und die für sie am besten geeignete Technologie nutzen (etwa Web versus Desktop versus Mobile).

Zusätzlich bietet eine Best-of-breed-Umsetzung mehr Möglichkeiten paralleler Entwicklung. Die Arbeitstei- lung ist einfacher, und auch eine jeweils eigene Entwick- lungsgeschwindigkeit (Release-Zyklus) ist möglich. Hier sind Anforderungen und Umsetzungen stärker voneinander entkoppelt.

Ich halte den Best-of-breed-Ansatz für unterschätzt. Zu selten werden große Softwaresysteme zum Nutzen für Anwender und Entwickler wirklich in separate Anwendungen aufgeteilt. Anwender profitieren von besseren Benutzer- schnittstellen, Entwickler profitieren von übersichtlicheren Codebasen. Es müssen weniger Kompromisse eingegangen werden, um unterschiedliche Rollen zufriedenzustellen. Der Nachteil von explizitem Aufwand für die Verbindung der Apps ist im Grunde ein Vorteil: Darin manifestiert sich Entkopplung. Veränderungen in einem Bereich schlagen weniger schnell auf andere Bereiche durch. 

Jede Applikation ist wiederum ein System für sich mit eigenen Portalen, Providern und einer spezifischeren Domäne als das Ganze. Die Zerlegung des Gesamtsystems in Applikationen ist also selbstähnlich. Slicing Part 4 Bild 9 zeigt, wie dabei eine Ressource zu einem Verbindungsglied werden kann; die ist vorher schon zu sehen (A) oder wird bei der Aufspaltung zur Integration eingeführt (B). Jede Applikation steht für einen kleineren Scope als das Gesamtsystem. Jede Applikation ist ein Inkrement, das heißt, sie bietet bei Lieferung für sich genommen dem Anwender einen Nutzen.

Der kleinere Scope besteht aus einer reduzierten Anzahl von Interaktionen je Applikation. Auch wenn die Interaktionen für ein ganzes Softwaresystem nicht bekannt sind und auch für die einzelnen Applikationen noch nicht klar sind, so ist dennoch gewiss, dass es je Applikation weniger sind als für das Gesamtsystem (Slicing Part 4 Bild 10). Auf diese Weise können Sie sich auf jeden Fall besser konzentrieren: bei der weiteren Analyse und bei der späteren Implementation.

Perspektiven

Aber was, wenn selbst je Applikation die Interaktionen noch nicht deutlich zu erkennen sind? Wie können Sie dann den Scope weiter einengen? Für mich ist dann die nächste Ebene innerhalb der Applikationen die der Perspektive. Innerhalb des gesamten Frontends einer Applikationen lassen sich oft „Bereiche“ unterscheiden, die sehr verschieden sind. Dieselbe Benutzerrolle nimmt in ihnen noch mal unterschiedliche Perspektiven ein. Ihr Blickwinkel wechselt zum Beispiel von Fachlichkeit zu Sicherheit, oder innerhalb der Fachlichkeit wechselt sie von den Stammdaten zu den Bewegungsdaten (Slicing Part 4 Bild 11).

Sehen Sie ein Frontend nicht als „monolithisch“ an. Was Anwender grundsätzlich durch das Frontend mit einer Applikation tun wollen, ist durchaus sehr verschieden. Indem Sie diese Perspektiven identifizieren, bilden Sie innerhalb des Scopes einer Applikation wiederum Untermengen von Interaktionen: die im einen Bereich versus die im anderen Bereich. 

Wieder gilt: Sie kennen die Interaktionen ja noch nicht. Nur eines ist gewiss: Wenn Sie das ganze Frontend in sinnvolle Teilbereiche zerlegen, wird jeder weniger davon enthalten, die dann auch leichter zu identifizieren sein werden. 

Wie manifestieren sich Perspektiven in einer Applikation? Meistens sind es Gruppen von Dialogen, Forms beziehungsweise Seiten. Sie starten zum Beispiel eine Desktop-Anwendung und finden darin im Menü verschiedene Bereiche, in die Sie „einsteigen können“. Denken Sie etwa an Microsoft Word:

Die zentrale Perspektive in Word ist die Bearbeitung des Textes; dazu gehören auch Dialoge wie Absatzformatierung oder die Formatierung mit Nummerierungen oder Aufzählungen.

Smartart und Diagramme hingegen rechne ich einer anderen Perspektive zu. 

Sendungen (Serienbriefe, Etiketten, Um- schläge) gehören ebenfalls zu einer anderen Perspektive.

Und so weiter.

Ich denke, wenn Sie es zulassen, so zu denken, dass die Benutzerschnittstelle einer Applikation eben kein Monolith sein muss, sondern aus Teilen, Modulen, Bereichen bestehen kann, dann werden Sie bei der Diskussion über Anforderungen erspüren, wo sich Grenzen ziehen lassen.

Der Zweck im Rahmen des Slicings ist auch hier, es Ihnen leichter zu machen, auf die Interaktionen zu kommen. Beim Slicing geht es nicht um UX. Sie sollen die Perspektiven nicht visuell gestalten. Wenn ein UX-Spezialist Ihnen in den Analysegesprächen zur Seite steht, ist das wunderbar. Zu früh können UI und UX nicht eingebunden werden. Dennoch ist das primäre Interesse beim Slicing nicht die Gestaltung, nicht das Wie, sondern
das Was. Was sollen Benutzer durch das Frontend anstoßen können? In welcher Granularität sollen sie Verhalten aus der Software „herauskitzeln“?

Dialoge

Wenn Ihnen Perspektiven etwas abstrakt vorgekommen sind, atmen Sie hoffentlich nun auf, wenn ich mit Ihnen eine Ebene tiefer in der Slicing-Hierarchie steige; dort finden sich die Dialoge. Jede Perspektive umfasst einen oder mehrere Dialoge, die konkrete Benutzerschnittstellenelemente zusammenfassen. 

Dialoge sind Fenster, Formulare, Seiten oder auch Tabs darin. Gewöhnlich sind es rechteckige Bildschirmausschnitte, die Sie minutiös mit Controls oder Widgets füllen, in denen Anwender entweder Daten erfassen, Verarbeitung triggern oder Daten angezeigt bekommen. Auf der Ebene der Dialoge werden Interaktionen das erste Mal verlässlich sichtbar. Ohne Interaktion öffnet sich kein Dialog. Das beginnt mit dem Applikationsstart. 

Als Beispiel kann eine simple Fakturierungsanwendung dienen, die aus folgenden Perspektiven mit ihren Dialogen besteht:

Sicherheit

Login
Passwort zurücksetzen

Stammdaten

Kundenübersicht
Kundendetails
Produktübersicht
Produktdetails

Bewegungsdaten

Rechnungsübersicht
Rechnungsdetails
Überfällige Zahlungen
Zahlungseingang

Jede Perspektive hat es leichter gemacht, sich auf die Frage zu konzentrieren: Welche Dialoge helfen dem Benutzer, seine Arbeit zu erledigen? Bei der Definition der Dialoge hilft es schon sehr, UI/UX-Spezialisten zur Seite zu haben. Sie können sagen, wie sich Daten am besten erfassen beziehungsweise präsentieren lassen; dazu gehört die Zusammenfassung in Dialoge. 

Auf der Dialog-Ebene der Slicing-Hierarchie geht es darum, welche Dialoge es gibt und wie sie zusammenhängen (Slicing Part 4 Bild 12). Von welchem Dialog kommt der Benutzer zu welchem anderen? Jeder Übergang steht schon für eine Interaktion. Endlich kommen Sie also bei den ersten Entry Points eines Softwaresystems an. 

Manchmal sind die Übergänge unidirektional wie zum Beispiel vom Dialog der Rechnungsübersicht zu den überfälligen Zahlungen. Das bedeutet nicht, dass der Benutzer nicht von dort zurück kann – er könnte zum Beispiel einen modalen Dialog einfach schließen –, sondern dass der Rückweg kein Verhalten triggert, das Sie speziell codieren müssten. Die UI-Technologie übernimmt für Sie den Rückweg. 

Manchmal gibt es Hin- und Rückwege zwischen Dialogen, die getrennt sind, wie zum Beispiel zwischen Login und Rechnungsübersicht. Das bedeutet, diese Interaktionen sind unabhängig voneinander. Die Benutzerin kommt vom Login-Dialog zur Rechnungsübersicht, muss nicht zurück (logout), kann aber. 

Manchmal kommt ein Dialog „aus dem Nichts“ (zum Beispiel Login). Das bedeutet, hier wird die Applikation gestartet und der Dialog erscheint als Erstes. 

Manchmal führt ein Dialog „ins Nichts“ (zum Beispiel Login, Passwort zurücksetzen oder Rechnungsübersicht). Das bedeutet, hier beendet eine Interaktion die Applikation. 

Und schließlich gibt es bidirektionale Überhänge wie zum Beispiel von Kundenübersicht zu Kundendetails. Dann ist der Rückweg nicht optional und auch bedeutungsvoll. Sie wollen über das UI-Verhalten hinaus – ein Fenster wird geschlossen – die Software etwas tun lassen, zum Beispiel sollen beim Rückweg veränderte Daten gespeichert werden. 

Ein solches Übergangsdiagramm könnten Sie auch schon auf der Perspektivenebene anfertigen. Doch dort ist gewöhnlich so wenig klar, von wo aus die Übergänge getriggert werden, dass diese Interaktionen eher spekulativ als nützlich sind.

Sobald Sie jedoch Dialoge in den Blick nehmen, wird Ihnen sehr schnell klar, wie sie zusammenhängen – und wie darüber dann auch Perspektiven verbunden sind. Vielleicht spüren Sie bereits am Beispiel in Slicing Part 4 Bild 12, wie sich Fragen aufdrängen. Klarheit will hergestellt werden. Wenn es für Sie zum Beispiel klar ist, dass es die Perspektiven Stammdaten und Bewegungsdaten gibt mit den Dialogen Kundenübersicht, Produktübersicht und Rechnungsübersicht, dann ist noch nicht offensichtlich, wie sie zusammenhängen. Es ist eine aktive Entscheidung, die Stammdaten aus der Rechnungsübersicht zugänglich zu machen und nicht unabhängig davon. Die Idee dahinter: Meistens wollen Anwenderinnen Rechnungen erfassen. Das führt zu Geldeingängen, das sollte im Vordergrund stehen. Stammdatenverwaltung ist nur eine notwendige, „lästige“ Aufgabe daneben. Diese Entscheidung hätte aber auch anders ausfallen können. Alle drei Dialoge könnten gleichberechtigt von einem „Shell-Dialog“ aus zugänglich sein, in dessen Rahmen sie angezeigt werden (MDI). 

Ohne sich in Details zu verlieren, können Sie mit Dialogübergangsdiagrammen schon darüber nachdenken, wie die Bedienungswege in Ihrer Applikation laufen sollen. Sie bauen sie auf hohem Abstraktionsniveau modellhaft auf. Praktikabel sind dafür auch Post-It-Notizzettel, deren Beziehungen Sie leicht verändern können, um zu erspüren, wie sich die grobe Bedienung wohl anfühlen würde (Slicing Part 4 Bild 13). 

Dialoge sind für Sie als Entwickler greifbar. Sie wissen, wie Sie einen Dialog implementieren. Hier können Sie sich wohlfühlen; es gibt konkret etwas zu codieren. 

Sie spüren etwas von dem Appeal, den Visual Basic 1.0 nach seinem Release im Jahr 1991 unmittelbar für viele gehabt hat: Es schien, als sei nun endlich visueller Softwareentwurf möglich. Software konnte scheinbar mit Dialogen und Steuerelementen strukturiert werden. Ein Doppelklick auf einem Menüeintrag oder Button hat sofort den Ort geliefert, an dem der Code geschrieben werden konnte, der den Benutzerwunsch erfüllt. 

Doch leider, leider war das ein Missverständnis. Denn dieses Vorgehen skaliert nicht. So lässt sich für wachsende Applikationen kein sauberer Code entwickeln. Dennoch steckte darin eine Wahrheit, nämlich die, dass die Gestaltung der Benutzerschnittstelle Klarheit darüber schafft, was denn an Verhalten wann wie getriggert werden soll. Deshalb ist für mich die Dialog-Ebene im Slicing so wichtig. Sie gibt Ihnen dafür konkrete Anhaltspunkte – allerdings ohne den Anspruch, die Lösung in einem Button-Click-Eventhandler zu schreiben. Die gehört hinter einen Entry Point, der eben kein solcher Eventhandler ist, sondern nur der Hebel, über den eine Logik-Maschinerie in Gang gesetzt und getestet werden kann. Eventhandler gehören noch zum Frontend; sie basieren auf UI-Technologie. 

Wenn Sie mit User Stories oder Use Cases arbeiten, ist es wichtig, dass Sie am Ende „abbiegen“ in Richtung Perspektiven und Dialoge (Slicing Part 4 Bild 6). Werden Sie ganz konkret. Lassen Sie den PO entscheiden, was er wo eingeben, auslösen und sehen will. Das darf er nicht mit einem Handwedeln an Sie abschieben. Sie riskieren sonst, dass Sie endlose teure Iterationen durchlaufen. Dialoge und ihre Übergänge sollten nicht erst mit Produktionscode für den PO erfahrbar gemacht werden, sondern vorher. Arbeiten Sie mit Papierprototypen, Mockups oder auch codierten Prototypen. Aber Hände weg von Produktionscode!

Interaktionen

Alle Interaktionen finden auf (beziehungsweise zwischen) Dialogen statt. Sie sind der Ort für die UI-Steuerelemente, die Interaktion überhaupt ermöglichen. UI-Steuerelemente können Ereignisse auslösen, und in den Eventhandlern dieser Ereignisse können Entry Points angestoßen werden. 

Wenn Sie die Dialoge einer Applikation identifiziert haben (oder auch nur eine Untermenge, die Ihnen für ein Inkrement reicht), kennen Sie schon erste Interaktionen: Das sind die, die einen Übergang zwischen Dialogen beinhalten. Für sie stellt sich die Frage, wie genau sie ausgelöst werden und wie die Entry Points dahinter aussehen. 

Darüber hinaus werden die meisten Dialoge allerdings noch weitere Interaktionsmöglichkeiten anbieten. Deshalb müssen Sie nun in jeden Dialog hineinzoomen. Das ist der nächste Level in der Slicing-Hierarchie. Sie machen die Dialoge auf und skizzieren, wie es darin aussieht. Was soll darin wann geschehen, das relevant für Sie im Sinne einer Codierung „hinter der Fassade“ eines Entry Points ist? Als Beispiel soll der Dialog zur Rechnungsübersicht in der Variante (A) dienen (Slicing Part 4 Bild 13). Bekannt sind schon die Interaktionen der Dialogübergänge:
Kundenübersicht öffnen

Produktübersicht öffnen

Rechnungsdetails öffnen

Zahlungseingang verbuchen

Überfällige Zahlungen anzeigen

Was könnte noch hinzukommen? Offensichtlich ist die Unterscheidung zwischen:

Neue Rechnung anlegen

Bestehende Rechnung öffnen

In beiden Fällen werden die Rechnungsdetails angezeigt. Was könnten Anwenderinnen aber noch in einer Rechnungsübersicht tun wollen? Ich kann mir vorstellen, dass sie zum Beispiel …

die Rechnungen filtern wollen, zum Beispiel nur die eines bestimmten Kunden oder nur die in einem Zeitraum,

die Rechnungen sortieren wollen, 

eine Rechnung archivieren wollen, wenn sie bezahlt ist, oder

eine Rechnung löschen wollen, solange sie noch nicht an den Kunden geschickt wurde.

Ihnen fallen vielleicht noch weitere Interaktionen ein. Oder nennen Sie es Funktionalitäten. 

Im Dialog haben Sie „Dinge“ vor sich, mit denen, an denen, durch die Anwender etwas tun wollen. Das entzündet bestimmt Ihre Fantasie. Fachlichkeit trifft auf Software. An der Oberfläche, in den Dialogen müssen alle fachlichen Aktivitäten und Prozesse steuerbar beziehungsweise im Ergebnis spürbar gemacht werden. Das wirft natürlich nicht nur die Frage nach den Interaktionen mit ihren Entry Points auf, sondern auch die nach der Struktur:

Wie ist die Erfassung des Inputs für jede Interaktion strukturiert?

Wie ist die Darstellung des Outputs für jede Interaktion strukturiert?

Wie genau soll die Verarbeitung getriggert werden?

Das bedeutet, Sie müssen sich jetzt doch einige Gedanken über das Layout der Benutzerschnittstelle machen. Spätestens auf der Dialog-Ebene der Slicing-Hierarchie ist es also wichtig, jemanden von UI/UX hinzuzuziehen. Mit einer solchen Expertin an der Seite finden Sie die beste Darstellung und die optimalen Entry Points. Denn je nachdem, wie leistungsfähig eine UI-Technologie ist, sind manche Entry Points nötig oder eben nicht. 

Betrachten Sie das Beispiel der Sortierung in der obigen Liste: Ja, Benutzer sollen die Rechnungen in der Übersicht sortieren können – aber müssen Sie dafür einen Entry Point anbieten und die Sortierung codieren? Oder kann das UI-Steuerelement für die Übersicht selbst sorgen? Was das UI „kostenlos“ bietet, müssen Sie nicht (unbedingt) als explizite Interaktion modellieren. 

Bei den Dialogübergängen haben Sie viele Dialoge im Überblick ohne großartige Details. Slicing Part 4 Bild 14 zeigt ein Beispiel, wie ich ein solches Diagramm oft anfertige: als Skizze auf einem Blatt Papier. Schnell hingeworfen bekomme ich auf diese Weise einen Eindruck vom Big Picture der Interaktionen. Anschließend kann ich entscheiden, wo ich hineinzoome. Die visuellen Details, die ich auf einer solchen Skizze mitgebe, haben höchstens den Zweck, dass ich leicht erkennen kann, um was für eine Kategorie von Dialog es sich grundsätzlich handelt, zum Beispiel Übersicht, Formular. Dann muss ich mir weniger Mühe beim Entziffern der Beschriftung geben.

Anschließend gehe ich die Übersicht Dialog für Dialog durch und überlege ganz genau, welche Interaktionen gerade hier gebraucht werden. Als Beispiel dient die Rechnungsübersicht in Slicing Part 4 Bild 15, ein Zoom-in auf den Dialog zur Rechnungsübersicht mit seinen Interaktionen. Für die ist nun sichtbar, wo genau sie ausgelöst werden. Und weitere Interaktionen kommen hinzu:

Manche Interaktionen übernehme ich dabei von der Dialogübersicht, zum Beispiel (1) und (2).

Allerdings starten die Pfeile nun an unterschiedlichen Punkten: Wenn ein Dialog überhaupt von dem im Fokus erreichtbar ist, beginnt der Pfeil weiterhin an dessen Rand (1). Manche Dialoge werden hingegen sehr spezifisch über Elemente angestoßen; dann beginnt der Pfeil nun im Dialog im Fokus (2), zum Beispiel wählt der Benutzer genau eine Rechnung aus, um für sie einen Zahlungseingang zu buchen.

Andere Interaktionen kommen hinzu. Sie waren im Überblick nicht zu sehen (3). Sie beginnen meistens deshalb auch bei einem Steuerelement im Dialog im Fokus. Und sie führen zurück zum Dialog. Zum Beispiel beim Filtern wechselt ja nicht der Dialog, sondern nur die Projektion der Daten innerhalb des Dialogs.

Entry Points

Die Skizze eines Dialogs mit seinen Interaktionen ist dann auch der Ort, an dem Sie zu den Entry Points übergehen. Sie sehen ja genau, was an Daten zur Verfügung steht und was angezeigt werden soll. Das ist die Information, die Sie brauchen, um informierte Entscheidungen darüber zu treffen, wie die Schnittstellen der Entry-Point-Funktionen aussehen sollen. Als Beispiel in Slicing Part 4 Bild 16 nochmal einige Interaktionen der Rechnungsübersicht. Jeder Interaktionspfeil hat nun …

einen Namen (1), der die Intention hinter der Aktion beschreibt und der Name der Entry-Point-Funktion wird,

Parameter (2), die an die Funktion übergeben werden und vom UI aus den Eingabe-Steuerelementen „zusammengesammelt“ werden, und

einen Rückgabewert (3), den der Zieldialog für die Anwenderin geeignet projiziert. Ich greife mal beispielhaft die Filtern-Interaktion heraus. Für sie gibt es einen eigenen Button im Dialog, und die Intention ist klar:

Es soll nach Rechnungsdatum und/oder Kundenname gefiltert werden. Mir scheint es am einfachsten, wenn all diese Angaben aus den Eingabefeldern (von & bis Datum, Name oder Namensteil des Kunden) immer in den Entry Point hineingehen. Dort wird genau geschaut, was vorhanden ist und wie die interne Abfrage dann am besten läuft.

Als Resultat liefert der Entry Point eine Liste von Rechnungsobjekten. Allerdings sind das nicht die Domänenobjekte, sondern speziell für die Darstellung im UI zusammengestellte Objekte. Deshalb heißt der Typ RechnungPo; Po steht dabei für ProjektionsObject, das heißt ein Objekt, dessen Zweck allein ist, Daten zu transportieren, die einer Darstellung genügend Futter bieten. Das Objekt ist also ein DTO (Data Transfer Object), allerdings eines zwischen Frontend und Domäne. Wahrscheinlich stehen darin nur die Angaben, die auch in der Tabelle der Übersicht zu sehen sind, und eine ID. Für alles Weitere muss das UI wieder bei einem Entry Point anfragen, wie es zum Beispiel mit ÖffnenDetails(id:string):RechnungsdetailsPo geschieht. Das führt zur Öffnung eines neuen Dialogs, der mehr Daten braucht. 

Ich liebe digitale Tools. Doch die Analyse der Anforderungen ist ein kreativer Prozess, der von vielen schnellen Iterationen profitiert. Da will ich schnell sein, wenn ich einen Gedanken habe. Deshalb benutze ich gern analoge Tools wie Papier und Stift, um das, was ich im Kopf habe, für mich oder auch andere greifbarer zu machen. Wenn es später sauberer nötig sein sollte, kann ich immer noch eine Reinzeichnung anfertigen. Meistens entfällt das jedoch, weil aus den Skizzen gleich Code wird.

Zwischenstand

Jetzt haben Sie die wesentlichen Ebenen der Slicing-Hierarchie kennengelernt. Alles beginnt beim Gesamtsystem mit dem Ziel, die einzelnen Interaktionen der Benutzer mit ihm herauszuarbeiten. Die sind auf solcher Flughöhe meistens nicht zu erkennen und kaum abzählbar. Deshalb ist eine schrittweise Annäherung über mehrere Zerlegungsebenen nötig (Slicing Part 4 Bild 17). Auf jeder tieferen Ebene zerfallen die Elemente der darüberliegenden in Teile, die jeweils einen überschaubareren Scope, also einen engeren Anforderungshorizont haben. Jedes dieser Teile auf jeder Ebene stellt ein Inkrement im Sinne der Agilität dar. 

Die Lieferung des Gesamtsystems befriedigt den Kunden vollständig, aber auch die Lieferung nur eines Dialogs mit all seinen Interaktionen oder sogar die Lieferung eines Entry Points stellt einen kleinen, werthaltigen Fortschritt dar. Keine der Ebenen des Slicings ist für Sie Pflicht. Sie steigen ein, wo Sie Klarheit gewinnen möchten. 

Das ist eigentlich immer beim System, das Sie zumindest einmal in seine Umwelt stellen sollten, um Rollen und Ressourcen zu identifizieren. Anschließend können Sie direkt zur Ebene der Interaktionen hinunterspringen – doch meistens werden Sie die noch nicht klar aus der großen Höhe des Systems erkennen können. Also überlegen Sie, ob eine Zerlegung in Applikationen Ihnen helfen kann, sich zu fokussieren. Oder ob Perspektiven im Frontend helfen, Sie zu inspirieren. 

Spätestens bei den Dialogen werden Sie jedoch praktischerweise landen, denn die haben immer eine Entsprechung im Code. Interaktionen und Entry Points pro Dialog zu sammeln ist für jedes Softwaresystem hilfreich. Dorthin sollten Sie deshalb auch von anderen Ansätzen wie User Stories oder Use Cases abbiegen. 

Tun Sie sich diesen Gefallen einer Konkretisierung mit dem Product Owner. Ohne Dialoge mit ihren Interaktionen erhalten Sie keine Klarheit über den ganz handfesten Umgang der Anwender mit einem Softwaresystem; das ist zentral für Akzeptanz und Nutzen. Ohne Dialoge mit ihren Interaktionen gibt es keine Klarheit über die Entry Points, mit denen Sie anschließend in Entwurf und Codierung starten.
