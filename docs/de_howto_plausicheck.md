# Eingabevalidierung und Plausiblitätscheck

Der Großteil der Dokuumentation für die API Grenzwerte ist in der YAML festgehalten:
https://github.com/europace/baufi-passende-vorschlaege-api/blob/main/api/baufi-passende-vorschlaege-api.yaml

Ab ca. Zeile 300 sind die Eingabeparameter mit Grenzwerten, Default-Werten und Beispielen.

Daneben gibt es weitere Plauliblitätsprüfungen im Quellcode:

## Finanzierungsbedarf
```
BigDecimal MAX_TILGUNG = 20;
BigDecimal MIN_TILGUNG = 0.5;
int MAX_ZINSBINDUNG_IN_JAHREN = 35;
int MIN_ZINSBINDUNG_IN_JAHREN = 1;
int MAX_FAELLIGKEITSDATUM_IN_JAHREN = 4;
int MAX_KREDITENTSCHEIDUNGSZEIT_IN_TAGEN = 60;
```
isEigenleistung > Modernisierungskosten 
(Eigenleistungen sind als Teil der Herstellungs oder Modernissierungskosten zu erfassen  und dürfen nicht höher als die gesamten Modernisierungskosten sein)

FINANZIERUNGSZWECK IN "KAUF", "KAUF_NEUBAU_BAUTRAEGER" dann kaufpreis > 0
(Bei Erwerbs-Vorgängen ist immer ein Kaufpreis anzugeben)

FINANZIERUNGSZWECK = "NEUBAU" dann herstellungskostenInklEigenleistungen > 0

FINANZIERUNGSZWECK IN "ANSCHLUSSFINANZIERUNG", "MODERNISIERUNG", "KAPITALBESCHAFFUNG" dann marktwert > 0 
(Bei Nicht-Erwerbs-Vorgängen ist ein Marktwert anzugeben, dieser kann bei Prolongationen auch 1€ sein)

Anschlussfinanzierung nur mit abzulösendem Darlehen ( wirdabgeloest = true) und Restschuld > 0
(die Darlehenssumme der Anschlussfinanzierung bestimmt sich aus der Restschuld aller abzulösenden Darlehen zum Zinsbidungsende)

## Kunden
```
int MAX_AGE_IN_YEARS = 90;
int MIN_AGE_IN_YEARS = 16;
int MIN_JAHR_DER_BESCHAEFTIGUNG = 1950;
isBeschaeftigungSeit >TODAY;
isBeschaeftigungSeit < GeburtsDatum;
```

## Immobilie
```
baujahr.minimum: 1600;
baujahr.maximum: objektBaujahr > naechstesJahr
```
Die PLZ der Immobilie muss sich auf der Liste der Dt. Post befinden
Es ist entweder eine PLZ oder ein Bundesland für den Standort der Immobilie anzugeben.
(Die Erwerbsnebenkosten bestimmen zu einem großen Teil den Eigenkapitalbedarf und werden aus dem jeweiligen Bundesland ermittelt.)
