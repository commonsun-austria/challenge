#Willkommen zu commonsun 

### Unser Ziel ist es, mehr Solarenergie für Menschen zugänglich zu machen. 

Der Ansatz den wir dabei verfolgen ist vor allem Mehrparteienhäuser (z.B. Zinshäuser) im städtischen Raum dafür zu nutzen. Deswegen haben wir nicht nur ein neues Geschäftsmodell für die Finanzierung entwickelt sondern stellen auch alle nötigen Verträge und organisatorische Unterstützung zur Verfügung (Photovoltaikanlagen auf Mehrparteienhäuser sind in der Umsetzung wesentlich komplexer als “standard” Anlagen).

Photovoltaikanlagen alleine lösen allerdings nicht das Problem (Klimawandel, mehr erneuerbare Energien, etc.). Hierfür benötigt es zusätzlich die Nutzbarmachung von SmartMeter Daten. Zum einen wollen wir unseren Kunden volle Transparenz geben und ihnen zeigen was ihr Investment oder ihr Verbrauch von grüner Energie bewirkt. Zum anderen ist es wichtig die Funktionen von SmartMeter voll zu nutzen und die gesammelten Daten auszuwerten, um zukünftig die Planung und den Ausbau von erneuerbaren Energien leichter und kostengünstiger zu machen.

#Modellierung
Unser Geschäftsmodell und dessen Prozesse in Kombination mit den dazugehörigen Softwarelösungen müssen daher auf einem aus­ge­klü­gelt und wartbaren Datenmodell aufbauen. Wir möchte ein realitätsnahes commonnsun Beispiel hernehmen um Deine Modellierungsexpertise miteinfliessen zu lassen. 

## Szenario

Ein Mehrparteienhaus hat mehrere Eigentümer und entsprechende Wohnungseinheiten. Ein Mieter zeichnet sich durch eine entsprechenden Wohnungseinheit und einem Stromtarif aus. 

Weiters wird eine oder mehrere Photovoltaikanlagen in diesem Objekt installiert. Eine Photovoltaikanlage hat einerseits ein Installationsteam und weiters auch ein Instandhaltungsteam. Bestimmte Tarifinformationen für eine Anlage stehen auch zur Verfügung. Jeder Mieter kann sich entschließen bei commonsun mitzumachen und erhält somit einen entsprechenden Vertrag und Vertragsstatus. Zuzüglich hat jeder Mieter auch einen SmartMeter pro Wohnungseinheit. Ein SmartMeter ermöglicht die kontinuierliche Abfrage von Nutzungsdaten.

Nun soll es z.B. ermöglicht werden auf Mieter und weiters auch auf Hausbasis Nutzungsinformation, welche über die SmartMeter eingespeist werden, einzusehen. Weiters möchten man auch per Mehrparteienhaus einsehen können wie der aktuelle Vertrags und kWh-Verbrauchsstatus über Alle oder Einzelmieter ist.

Diese Szenario versucht grob die primären Business-Bausteine abzubilden. Überlege Dir ein mögliches Datenmodell, welches versucht die zuvor beschriebenen Abhängigkeiten abzubilden.

#Schnittstellen
In diesem Beispiel geht es darum ein paar Schnittstelle zu definieren und weiters die Business-Logik dahinter zu implementieren. Bitte entwerfe Dein Design der REST-API, mögliche Datenabhängigkeiten, kommentiere Deine Tätigkeiten (code level, Git commits), so wie Du es sonst auch machen würdest... Du kannst natürlich bestehende Frameworks und Libraries verwenden, bitte dokumentiere nur Welche und warum.

## Szenario
### Verarbeitung von SmartMeter Nutzungsberichten.

Wie zuvor erwähnt möchten wir einen REST API zur Verfügung stellen, welche ankommende SmartMeter Nutzungsberichte verarbeitet und nach entsprechender Auswertung mit weiteren Daten anreichert. 

Ein Nutzungsbericht besteht aus den folgenden Attributen:

* Anzahl der verbrauchten Kilowattstunde (kWh)
* Verbrauchstyp
* Messungsstart (UTC-Timestamp)
* Messungsende (UTC-Timestamp)
* Eindeutige Kundenidentifikationsnummer

Sobald ein Nutzungsbericht angenommen wurde, retourniere die gleichen Information mit weiterer Anreicherung wie folgt:

* Erstellte Nutzung-Id (Transaction)
* Status der Nutzungsverarbeitung
* Aktueller Gesamtverbrauch des Kunden in kWh
  * Gesamtverbrauch ergibt sich aus allen Nutzungsbericht eines Kunden über alle Verbrauchstypen
* Verbrauchsaufteilung auf Basis des Verbrauchstyps in kWh
  *  Aufteilung des Gesamtverbrauch hinsichtlich der Verbrauchstyps eines Kunden
* Aktuelle Kosten auf Basis des Verbrauchstyps
  * Summierung aller Verbrauchstyps und deren Kosten eins Kunden

#### Verbrauchtstypen

SmartMeter klassifiziert Nutzung in zwei Verbrauchstypen.

##### Booked
- Ein gewisses kWh-Kontingent ist pro Kunde definiert. Befindet sich der Kunde in diesem Rahmen handelt es sich um eine "Booked" Nutzung. Booked Nutzungen haben einen bestimmten Preis.

##### Flexible
- Erweitertes kWh-Kontingent. Befindet sich der Kunde ausserhalb des geschätzten kWh-Kontingents bezieht er den Strom aus dem "Flexible" Kontingent. Flexible Nutzungen haben einen bestimmten Preis.

#### Status der Nutzungsverarbeitung

Die Verarbeitung eines Nutzungsbericht kann aktuell zwei Zustände aufweisen

- Accepted
  - Alles hat gepasst
- Failed
  - Die Verarbeitung war fehlerhaft + Reason

### Gewünschte API Endpunkte

- Endpunkt um einen Nutzungsbericht hinzuzufügen
- Endpunkt um für einen Kunden alle Nutzungsberichte abzufragen
- Endpunkt um einen Nutzungsbericht zu löschen
  - Limitierung auf Nutzungsberichtverarbeitung im Status "Failed"

  
#Formular-Design	
Die Aufgabenstellung zuvor hat den primären Fokus auf entsprechend Backend-Entwicklungen, die nächste Herausforderung beschäftigt sich mit primärer Frontend-Entwicklung im Bereich eines Formulars. Bitte erstelle ein Formular mit den folgenden Feldern und berücksichtige auch eine responsive Darstellung. Du kannst natürlich bestehende Frameworks und Libraries verwenden, bitte dokumentiere nur welche und warum. (M) steht für mandatory und (O) für Optional. Styling ist Dir überlassen. Simple und Clean stehen im Vordergrund.

* Vorname (M)
* Nachname (M)
* Alter (M)
* Kontakt Tel. (M)
* Kontakt Email. (M)
* Adresse (M) (Single Select Drop Down)
* Top# (M)
* Tarif in kWh (M)
* Netzbetreiber (M) (Single Select DropDown)
* Upload Dateianhang (O) (Drag and Drop Support + File selector)
* Captcha Support
* Submit Button



#Architektonische Gedanken
Die vorigen Abschnitte haben sich mit generellen Modellierungs- und Umsetzungsaspekten beschäftigt. Wir möchten nun diesen Abschnitt nutzen um Deinen Input bezüglich einer möglichen System-Architektur und weiteren Aspekten zu erhalten. Natürlich ist es auf Basis der zuvor erwähnten Beispiele schwierig in komplette Architektur aufzustellen und zu definieren, aber uns geht es eher darum wie Du generell die Gesamtarchitektur und weitere Entwicklungspraktiken platzieren würdest. 

Siehst Du dieses Szenario eher in der Cloud, falls ja, welche Cloud-Dienste würdest du hier heranziehen, falls nein, wie würdest du vorgehen? Wie gehts du generell mit Non-Functional Requirements um. Was bedeute für Dich DevOps und welche QA-Praktiken sind für die Essentiell. In welchen Frontend und Backend-TechStack würdest du commonsun und deren Lösungen sehen?
