# Begriffe Rating, Scoring und Rank in der #passt Vorschläge-API

## Rating
Lead Rating: Oberbegriff und Knoten für alle Rating-Konzepte der Vorschläge-API, das Lead Rating bezieht sich immer auf die gesamte Verbraucher-Situation bzw. alle Vorschläge der Ergebnisliste

successRating (A-D): Klassifizierung der Lead-Qualität nach Abschlusswahrscheinlichkeit (beide unterschreiben)

effortRating (true,false): Vorgang abzuschließen wird aufwändig, inkl. Beschreibung warum

feasibilityRating (0-100): Anteil der machbaren Angebote in der Ergebnisliste, Ausdruck der Wahrscheinlichkeit, dass ein Antrag erstellt werden kann (unabhängig davon ob der Verbraucher wirklich abschließen will)

## Scoring:

scoring: Sortierung der einzelnen Vorschläge nach 12 Scoring-Kriterien, für die Nutzer unsichtbar, Ergebnis ist ein technischer Score-Wert zum Sortieren. Dieser Wert wird nicht explizit in der Antwort ausgegeben. 

rank: sichtbares Ergebnis des Scorings für den Nutzer, niedrigster Score = rank 0, im Mode "exploration-matrix" werden immer nur das jeweils am besten gescorte Angebot für jede Matrixposition ausgeliefert (minimaler rank)
