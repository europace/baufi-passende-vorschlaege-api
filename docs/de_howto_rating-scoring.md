# Begriffe Rating, Scoring, Rank und feasibility in der #passt Vorschläge-API

## Rating

- Lead Rating: Oberbegriff und Knoten für alle Rating-Konzepte der Vorschläge-API, das Lead Rating bezieht sich immer auf die gesamte Verbraucher-Situation bzw. alle Vorschläge der Ergebnisliste

- successRating (A-D): Klassifizierung der Lead-Qualität nach Abschlusswahrscheinlichkeit (beide unterschreiben)

- effortRating (true,false): Vorgang abzuschließen wird aufwändig, inkl. Beschreibung warum

- feasibilityRating (0-100): Anteil der machbaren Angebote in der Ergebnisliste, Ausdruck der Wahrscheinlichkeit, dass ein Antrag erstellt werden kann (unabhängig davon ob der Verbraucher wirklich abschließen will). Der Wert feasibilityRating entspricht einem Mittelwert über alle Machbarkeitsergebnisse die in der Market Engine berechnet werden. Das sind im default Mode gewöhnlich 30 Produktanbieter mit all ihren Alternativen die auch in BaufiSmart erzeugt werden. Im Mode _exploration-matrix_ [de_howto_matrix.md](de_howto_matrix.md) sind es 9x30 Angebote, allerdings mit weniger Optionen als im default Mode. Die Machbarkeit wird dabei nach folgenden Werten berechnet: rot = 0, gelb = 50, grün = 100. Eine Vorschlagsliste mit 2x grün, 1x gelb und 1x rot ergibt ein feasibilityRating von 62,5. Die unter _gewuenschteAnzahlVorschlaege_ anforderte Anzahl Vorschläge hat keine Einfluss auf das feasibilityRating.

## Scoring

- scoring: Sortierung der einzelnen Vorschläge nach 12 Scoring-Kriterien, für die Nutzer unsichtbar, Ergebnis ist ein technischer Score-Wert zum Sortieren. Dieser Wert wird nicht explizit in der Antwort ausgegeben. 

- rank: sichtbares Ergebnis des Scorings für den Nutzer, niedrigster Score = rank 0, im Mode "exploration-matrix" werden immer nur das jeweils am besten gescorte Angebot für jede Matrixposition ausgeliefert (minimaler rank)

## Machbarkeitsmodell

API Aufrufe von Partnern mit umfangreichen Geschäftsbeziehungen durchlaufen vor der Berechnung in der Market Engine einn Machine Learning Prozess zur Vorhersage der Machbarkeit der konfigurierten Geschäftspartner. Die dabei ermittelten Produktanbieter mit der höchsten Machbarkeits-Wahrscheinlichkeit werden dann in der Market Engine exakt berechnet. Da bei diesem Vorgehen neben dem Machbarkeitsergebnis der Market Engine (Regelwerk) auch eine Machbarkeitswahrscheinlichkeit aus der Vorhersage anfällt, werden wir perspektivisch beide Werte für das feasibilityRating heranziehen.
