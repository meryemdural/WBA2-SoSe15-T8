# WBA2-SoSe15-T8
WBA 2 SoSe15 Team 8 (Meryem Dural, Aylin Mengi, Aziz Kilic)
  
##Aufgaben zum ersten Workshop
Datei "A1.js"

##Exposé

###Accuratus
####Funktionen
Website, auf der Rezepte für zb selbstgemachte Hygieneprodukte
Alle können Rezepte hochladen oder Fragen posten. wenn rezept korrekt setzt admin häkchen
man kann kommentieren
man kann liken

löschen und bearbeiten kann der der gepostet hat und admin


####Anwendungslogik: 
am Ende des Tages wird beliebtestesRezept ausgewertet und die Rangliste wird angepasst.
Fragen die häufiger gestellt werden, werden zusammengepackt und hervorgehoben  
Zutaten für Rezepte können ausgewählt werden.(zb ich habe diese Zutaten, was kann ich daraus herstellen?)  

####Szenarien

#####Szenario1 Ideenqueen

Sara R. (24) stellt seit 2 Jahren ihren eigenen Hygieneartikel her. Sie schmeißt  regelmäßig Rezeptpartys, um ihre Ideen und Rezepte ihren Freundinnen mitzuteilen. Seit sie auf der Website „Accuratus“ postet, sind sie und ihre Rezepte weltweit bekannt. Es erfüllt sie mit Stolz etwas zum Umweltschutz beitragen zu können. 


#####Szenario2 Neuanfänger 

Thomas M. (22) möchte seinen Lebensstil verändern. Er hat es satt seinem Körper durch die ganzen Chemikalien Schaden zuzufügen. Allein der Grund, dass in seinem Deo
entweder Aluminium oder Alkohol enthalten sein muss, macht ihn wahnsinnig. Durch einen Freund erfährt er von der Website „Accuratus“ und fragt nach einem Rezept für selbstgemachtes Deo. Innerhalb von einer Stunde wird seine Frage mehrfach kommentiert, wodurch er 3 Rezepte für ein Deo erhält, die er direkt ausprobiert. 


#####Szenario3 Zutatenvorrat

Maria S. (32) macht es Spaß ihre Produkte selbst herzustellen. Sie benutzt für die Rezepte die Website „Accuratus“, wo sie eingibt welche Zutaten sie zu Hause hat und angegeben bekommt, was sie aus diesen herstellen könnte. Wenn ihr eine Zutat fehlt besorgt sie sich diese unverzüglich. 


#####Szenario4 Admins

Mia M. (22) ist eine der drei Admins von „Accuratus“. Ihre Aufgabe besteht darin, die geposteten Rezepte auszuprobieren und zu bewerten. Wenn die vorgegeben Produkte ihren Zweck erfüllen, versieht Mia diese mit einem grünen Häkchen, sodass die Besucher der Website stets wissen, welchen Rezepten sie vertrauen können.



##1. Dokumentation des Service: 
###1.1 Angabe der Ressourcen, der dafür vorhergesehenen http Verben und deren Semantik
		Tabellen

###1.2 Überlegungen, die angestellt wurden zur Definition der Ressourcen, Alternativen, die betrachtet wurden

Zu Beginn des Projekts wurde überlegt, welche Funktionen der Blog beinhalten sollte und welche Ressourcen dafür gebraucht werden. Die gewünschten Funktionen sind:
einen Blogeintrag erstellen
einen Blogeintrag ändern und oder löschen 
einen Blogeintrag kommentieren
Kommentar zu dem Blogeintrag löschen können

Es ist zu beachten, dass der Kommentar lediglich gelöscht jedoch nicht bearbeitet werden kann, da diese Kommentare zu Diskussionen führen. Es ist nicht erwünscht, dass durch das nachträgliche Bearbeiten eines Kommentars der Sinn dieser Diskussion verfälscht und oder andere User kompromittiert werden. 

Es wurden Überlegungen angestellt, wie man über die Redis-Datenbank Kommentare erstellen und sie nach ihrer Identifikationsnummer (ID) löschen kann. Die Problematik der Kommentare , hinsichtlich einer erstellten Liste, liegt darin, dass ein einzelner Beitrag dieser Liste schwer abzurufen ist. Aufgrund dessen ist eine Hashtabelle verwendet worden.  
Kommentare werden über die Funktionen Sadd und Hset erstellt. Sadd ist eine Tabelle, die angibt, welche Kommentare sich mit welcher ID unter den Blogeinträgen befinden. Mithilfe von Hset wird der Kommentar erstellt und erhält einen HashKeyValue. Über diesen HashKeyValue können einzelne Kommentare gelöscht werden. Mit einer einfachen Redis-Liste, hätte dies nicht funktioniert, da nicht auf einen bestimmten Kommentar ohne ID zugegriffen werden kann.
Jeder erstellte Blogeintrag bekommt eine ID, welche von der Redis-Datenbank mit der Funktion INCR id übergeben wird 


###1.3 Beschreibung der Anwendungslogik und der Datenhaltung mit Überlegungen dazu
Datenhaltung: Ein vollständiger Eintrag des Projekts ist in dem Jason-Format, welcher in der Redis-Datenbank gespeichert wird, wohingegen es hätte so geschrieben werden können, dass dieser Eintrag sukzessiv eingespeichert werden kann. Somit könnten alle Titel, alle Autoren und alle Zutaten separat gespeichert und anschließend zusammengesetzt werden. Diese Möglichkeit wurde aus den Überlegungen ausgeschlossen, da die Vorgehensweise wirkungslos komplex gewesen wäre. 
Das Jason-Format bietet die Möglichkeit die Elemente des Eintrags über verschiedene Module einzeln abzurufen. Hierzu dient das EJS-Modul.
Redis-Datenbank:


Anwendungslogik des Service:
Die Anwendungslogik des Services wurde über das Modul Express angefertigt, welches dem Nutzer ermöglicht, einen Blogeintrag oder einen Kommentar zu erstellen und zu löschen. Der Blogeintrag kann von dem Nutzer geändert werden, sofern er diesen selber verfasst hat. Im Gegensatz zum Blogeintrag, ist dies bei einem Kommentar nicht möglich.

Außerdem bietet der Service eine Such-Funktion über den Query-Parameter  „search“. Es kann nach den Rezepttitel gesucht werden, indem man den Suchbegriff eingibt und die Datenbank nach diesem Begriff abgesucht wird. Dies geschieht über das Modul Reds (https://www.npmjs.com/package/reds) . 
Mithilfe von Reds wird die Datenbank als Jason-Datei ausgegeben.
Der Nachteil bei dem Reds-Modul besteht darin, dass die Namen der Jason-Objekte mit gesucht werden. Wenn ein Nutzer den Begriff „Autor“ eingäbe, bekäme er alle Blogeinträge zurück, das jeder Blogeintrag den Namen des Jason-Objekts „Autor“ beinhaltet.
Ein möglicher Lösungsvorschlag für diese Problematik wäre, die Blogeinträge aufzuteilen, sodass die Jason-Objekte einzeln gespeichert werden würden.

####1.4 Beschreibung der Funktionalität, die aus Zeitmangel nicht umgesetzt werden konnte
Bei dem Rest-Client: 
Dadurch, dass die Grundidee mit ihren Funktionen nur einen Blog mit Kommentaren umfasst, ist die gebrauchte Funktionalität klarstrukturiert gewesen.
Die Rubrik „topcommented“ wurde geschrieben, konnte jedoch aus Zeitmangel nicht eingebunden werden. Sie hätte angezeigt, welcher Blogbeitrag am meisten kommentiert wurde.


##2. Dokumentation des Dienstnutzers
###2.1 Beschreibung der Anwendungslogik und der Datenhaltung mit Überlegungen dazu
Die Anwendungslogik des Dienstnutzers besteht darin, dass Faye verwendet wird, um Push-Nachrichten zu senden und ein Chat-System zu haben. Die Überlegungen hierbei sind gewesen, einen interaktiven Blog zu erstellen. Für diesen Blog hat sich das Faye-Modul gut geeignet.
Des Weiteren wurde für die Datenhaltung das Faye-Redis-Modul verwendet, welches mit dem Redis-Server kommuniziert.
Faye(https://www.npmjs.com/package/faye) nutzt das Faye-Redis-Modul, um Status und Routing-Nachrichten zu speichern. Faye-Redis ist die Engine, die mit dem Redis-Server kommuniziert. 


###2.2 Umsetzung der Präsentationslogik und Überlegungen dazu

Die Präsentationslogik basiert auf  das Modul EJS. (schreiben was EJS ist —> Video 3) 
EJS sind Templates, das sind halt Codes die man immer und immer wieder schreibt. Man kann das halt ersetzen. ( Video 3 nochmal ansehen und das ergänzen)
Zusätzlich zu dem EJS-Modul wird das Modul EJS-Mate verwendet, wodurch ein Layout definiert werden kann, welches wiederholt verwendet wird.
Das freizugängliche Bootstrap CSS wir verwendet.




###2.3 Beschreibung der asynchron implementierten Teile der Nutzungsschnittstelle und Begründungen dazu
Asynchrone Implemetationen des Projekts sind zum einen das Express Modul und zum anderen die Kommunikation mit dem Redis-Server. Dies ermöglicht den Nutzern gleichzeitig Blogeinträge zu erstellen, Blockeinträge zu lesen, Blockeinträge zu kommentieren und zu publischen/subscriben.
(sind die ganzen Module) 
Dies ist die Nutzungsschnittstelle.
Der Grund für die Verwendung des Express Modul ist die Asynchronität.


###2.4 Beschreibung der Funktionalität, die aus Zeitmangel nicht umgesetzt werden konnte
Zu Beginn existierte die Idee, über Tags oder über Zutaten Rezepte zusammenstellen zu können.
Die Suchfunktion, welche es ermöglicht hätte nach Zutaten zu suchen und ein Rezept zu diesen angegeben zu bekommen, wurde aus zeittechnischen Gründen nicht geschrieben. Diese Art der Suche ist zu komplex gewesen.
Des Weiteren war auch eine Schnittstelle geplant, die die Möglichkeit bieten sollte, Blogeinträge als Favoriten zu markieren. Um dies realisieren zu können, wäre eine Anmeldungs-Funktion notwendig gewesen, welche aus zeittechnischen Gründen nicht umgesetzt werden konnte. Aus diesem Grund ist das Projekt aus der Admin-Perspektive, ohne jegliche Aufteilung oder Restriktionen für jeden Nutzer.
