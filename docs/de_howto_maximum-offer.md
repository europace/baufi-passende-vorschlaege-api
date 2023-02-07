# Vorschläge-API ermittelt maximal machbaren Kaufpreis für eine Verbrauchersituation

Die Vorschläge-API kann für den Verwendungszweck KAUF aus einer beliebigen Konstellation an Verbraucherdaten einen Vorschlag ermitteln, der auf einem maximal machbaren Kaufpreis basiert. 

Zur Aktivierung der Maximal-Preis-Ermittlung ist in den Metadaten der dafür notwendige Modus "maximum-offer" zu aktivieren:

```
{
    "metadaten": {
        "datenkontext": "TEST_MODUS",
        "mode": "maximum-offer"
    }
}
```

## Anfrage

Die API-Anfrage muss den Verwendungszweck KAUF enthalten. Die minimalen Anforderungen für eine machbare Finanzierung liegen je nach Bundesland zwischen 10.000€ und 15.000€ Eigenkapital sowie 1500€ - 1700€ Nettoeinkommen. Sind diese Bedingungen nicht erfüllt, kann kein Vorschlag generiert werden, da in diesem Mode nur machbare Vorschläge ausgeliefert werden. Weiterhin müssen Geschäftsbeziehungen zu den verwendeten Produktpartnern existieren, zz. sind das die ING und die Deutsche Bank. Die Plattform-Plakette NFR60 (default) erfüllt diese Bedingungen. Der unter _finanzierungsbedarf_ eingegebene Kaufpreis wird bei der Berechnung ignoriert. Die Eingabe-Validierung für das Feld _kaufpreis_ wird im Mode "maximum-offer" deaktiviert, dieser kann auch null sein.

## Antwort

Die API-Antwort enthält im Mode "maximum-offer" das zusätzliche Feld _maximalKaufpreis_. Im Knoten _nebenkosten_ werden die für diesen Kaufpreis notwendigen Nebenkosten ausgegeben. Der im Vorschlag verwendete Darlehensbetrag wird aus den maximal-Werten und dem verfügbaren Eigenkapital ermittelt.

## Randbedingungen

- Zz. werden maximale Kaufpreise bis zu 1 Mio € ermittelt. Für größere Beträge würde sich die Berechnungsdauer unnötig verlängern. Wir gehen davon aus, dass Objektwerte über 1 Mio € immer individuelle Beratung erfordern. 
- Da sich die Erwerbsnebenkosten je Bundesland unterscheiden, gleichzeitig aber den größten Anteil am benötigten Eigenkapital ausmachen, unterscheiden sich auch die maximalen Kaufpreise je nach Budensland bzw. PLZ.
- Die Ermittlung des maximalen Kaufpreises erfolgt iterativ über bis zu 7 Berechnungen. Die API-Antwort steht darum üblicherweise erst nach 3-5 sec zur Verfügung. Die Frontends sollten entsprechende Warte-Hinweise einblenden bis das Ergebnis verfügbar ist.
- Wir verwenden für die Ermittlung des maximalen Kaufpreises keine Nachrang-Finanzierungen. Im Zuge einer individuellen Beratung mit Nachrang-Vorschlägen wäre dann auch ein höherer maximaler Kaufpreis darstellbar als der von der API bereitgestellte.
