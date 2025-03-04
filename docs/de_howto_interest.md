h1. Zinsvergleich als schneller synchroner API Endpunkt

Folgender synchroner API Endpunkt liefert eine Übersicht über bis zu 6 Zinsbindungen, die über einen Matadaten-Parameter vorgegeben werden können

```
https://baufinanzierung.api.europace.de/v1/interests/
```

Übergabe der Wunsch-Zinsbindungen:
```
{
  "metadaten": {
    "datenkontext": "TEST_MODUS",
    "gewuenschteZinsbindungen": [5, 10, 15, 20, 25, 30]
  },
```

Alle anderen Requestparameter entsprechen einem Standard-Aufruf.
Es wird jeweils der günstigste machbare Vorschlag je Zinsbindung geliefert.

