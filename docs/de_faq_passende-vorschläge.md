FAQ zur Anbindung und Nutzung der #passt Vorschläge API

- Wie kann ich die API Schnittstelle testen?
   >- In den Metadaten der API "Datenkontext": "TEST_MODUS"
   >- In BaufiSmart API Credentials generieren

- Wie erfahre ich wenn die API planmäßig oder unplanmäßig down ist bzw. einige Funktionalitäten nicht verfügbar sind?
   >- Entweder du besuchst unsere Statusseite https://status.europace2.de/ oder du abonnierst einfach unser Status-Update (auch unter https://status.europace2.de/) und bekommst automatisch E-Mails von uns, wenn etwas nicht läuft, wie es soll.
   
- Darf ich Bank-Logos anzeigen und wie erhalte ich die Logos?
   >- Wir haben keine generelle Genehmigung unserer Partner, ihre Logos weitergeben zu dürfen. Insofern bitten wir dich, die Nutzung von logos immer erst mit den entspechenden Partnern abzustimmen.
- Mit welchen Konditionen rechnet die #Passt API?
   >- Wir nutzen die Echt-Konditionen unserer Partner, die wir auf all unseren produktiven Market Engines von EUROPACE 2 - BaufiSmart verfügbar haben.

- Mit welche Handelsbeziehungen wird gerechnet und kann ich festlegen welche angezeigt werden?
   >- Es werden die Handelsbeziehungen der für die API Authentifizierung verwendeten Plakette des Partnermanagements verwendet, also deine bereits auf Europace eingestellten Handelsbeziehungen.

- Wie kann ich mit Leads umgehen die ich nicht selbst bearbeiten kann. Hat Europace eine Lösung wie ich Anfragen verteilen und monetarisieren kann?
   >- Innerhalb unseres Hypoport-Netzwerks arbeiten wir daran, dir für jeden Use Case eine Lösung anbieten zu können. Sprich uns gern einfach über passt@europace.de an.

- Wie kann ich Leads innerhalb meines Vertriebes / Netzwerks verteilen?
   >- ?

- Muss ich ein 34i-Nachweis vorweisen um #passt auf meiner Landingpage einzusetzen?
   >- Nein. #passt-Technologie ersetzt die Beratung nicht, sondern dient vielmehr als self enabling Tool für Verbraucher:innen, sodass für die Anzeige der passenden Vorschläge keine 34i-Zertifikate notwendig sind.

- Muss ich Datenschutzbestimmungen verfügbar machen und wann/wo in meiner Leadstrecke?
   >- Ja, Datenschutz ist enorm wichtig. Du solltest immer darauf achten, deine Datenschutzhinweise für Verbraucher:innen verfügbar zu machen. Bei Nutzung der #passt-Technologie ist es für die reine Ermittlung von Vorschlägen noch nicht notwendig die Europace-Datenschutzhinweise anzuzeigen, da wir nur mit nicht personenbezogenen Daten arbeiten und diese Daten nach sehr kurzer Zeit bereits wieder gelöscht werden. Sobald ein Vorgang auf Europace angelegt wird, muss du VOR Anlage des Vorgangs deine und unsere Datenschutzhinweise bereitstellen.

- Ich möchte eine Zustimmung für Werbezwecke/Beratungen außerhalb der Baufinanzierung einholen. Wie mache ich das?
   >-

- Was kostet die API Nutzung?
   >- Aktuell berechnen wir für die Nutzung der #passt-Technologie nichts. Einzige Bedingung ist die Vorgangsanlage auf Europace und der Abschluss über uns.

- Ich habe keine IT Ressourcen, gibt es einen Standard #passt Rechner oder Widget?
   >- Ja, du kannst hierfür gern unser BaufiPasst nutzen. 

- Ist die #passt Berechnung auch über Maklersoftware verfügbar?
   >- Ja, wenn sie REST-API-fähig ist.

- Mit welchen CRMs arbeitet ihr zusammen?
   >- Alle CRMs, die REST-API-fähig sind.
   
- Welche Verwendungszwecke können berechnet werden?
   >- #passt-Technologie unterstützt alle Verwendungszwecke, die auch in BaufiSmart verügbar sind.

- Erhalte ich ein Reporting über die wichtigsten KPIs (Requests, Conversions, Bearbeitungsstatus etc)?
   >- Aktuell ist dies nachgelagert über den Abnehmer der Leads möglich.

- Was passiert, wenn ein Nutzer beraten werden möchte, wenn ein Angebot ausgewählt worden ist, wenn ein Vorgang angelegt worden ist. Bekomme ich eine Info? Landet der Lead in EP oder in meinem CRM?
   >- Das hängt davon ab, wie deine Leads gemanaged werden. Hier ist viel Spielraum möglich.
   
- Wenn ein Nutzer eine Anfrage gestellt hat und seine Daten aktualisiert, aktualisiert sich auch der Vorgang?
   >- Wie heißt es so schön... Das hängt davon ab... Je nachdem, was du möchtest. Du kannst über die Kundenangaben-API von Europace die Vorgangsdaten aktualisieren lassen, wenn du möchest. Wenn du das aber nicht zulassen möchtest, sobald ein Vorgang angelegt wurde, kannst du diese Funktion auch einfach weglassen.
 
- Die Schnittstellenanbindung funktioniert nicht. Die Angebote aus Starpool werden nicht überspielt.
   >- Starte zunächst den Prozess in deinem Frontend-Tool. Wenn du in dem Anmeldeprozess auf der Europace-Loginseite ankommst und nach deinem Benutzernamen gefragt wirst, gib bitte deine PartnerId ein, mit der du bei Starpool registriert bist. Danach wirst du zum Starpool Portal weitergeleitet, wo du das Starpool-Passwort eingeben musst. Der Benutzername sollte nicht geändert werden. Anschließend kannst du der Nutzung von deinem Frontend-Tool zustimmen und du wirst weitergeleitet.

Noch mehr Informationen rund um die passende-vorschlaege-API findest du in unseren How To´s: https://github.com/europace/baufi-passende-vorschlaege-api/tree/main/docs
