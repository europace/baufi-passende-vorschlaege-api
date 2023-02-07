# Hinweise zur Gestaltung einer Prolongations-Anfrage

## sonderzahlungZumZinsbindungsEnde
Werden Sonderzahlungen zugelassen, so ist entsprechend hohes Eigenkapital vorzusehen, sonst kommt es in der ProductEngine zu einer Unterdeckung.
Die Summe der Sonderzahlung wird nacheinander auf die abzulösenden Darlehen verteilt, beginnend mit der kleinsten Restschuld.

## marktwert
Der Marktwert ist nur anzugeben, wenn machbarkeits- oder konditionsrelevante Regeln in den ProductEngines darauf aufbauen, z.B. beleihungsauslaufabhängige Konditionen. Im Normalfall kann hier auch 1€ vorgegeben werden.

## laufzeitende
Die Angabe des kalkulierten Ende der Gesamtlaufzeit ist notwendig, um regulatorische Vorgaben der Produktanbieter einzuhalten. Die Tilgung des Anschlussdarlehens muss mindestens so hoch sein, dass die ursprüngliche Gesamtlaufzeit nicht überschritten wird.

## Minimal-Anfrage für eine Prolongation bei der "PROLOSMART" Bank (inkl. Optionserzeugung 3x3 Matrix)
```
{
    "metadaten": {
        "datenkontext": "TEST_MODUS",
        "extKundenId": "YET15",
        "extClientId": "test",
        "mode": "exploration-matrix"
    },
    "kundenangaben": {
        "haushalte": [
            {
                "kunden": [
                    {
                        "geburtsdatum": "1983-01-01",
                        "einkommenNetto": 5000.0
                    }
                ],
                "finanzielleSituation": {
                    "eigenKapital": 50000
                }
            }
        ],
        "finanzierungsbedarf": {
            "finanzierungszweck": "ANSCHLUSSFINANZIERUNG",
            "sonderzahlungZumZinsbindungsEnde": 5000
        },
        "finanzierungsobjekt": {
            "anschrift": {
                "plz": "04860"
            },
            "marktwert": 180000,
            "darlehensliste": [
                {
                    "wirdAbgeloest": true,
                    "darlehensgeber": "PROLOSMART",
                    "grundschuld": 142323.35,
                    "zinsbindungBis": "2023-09-30",
                    "laufzeitende": "2049-02-28",
                    "restschuld": {
                        "zumAbloeseTermin": 132323.35
                    },
                    "darlehenskontonummer": "Darlehen_123"
                }
            ]
        }
    }
}
```

## Bestandsdarlehen und Verknüpfung zwischen Alt- und Neu-Darlehen

In Kombination mit der Kundenangaben-API besteht die Möglichkeit der direkten Zuordnung von Alt- und Neudarlehen. Diese Zuordnung ist Voraussetzung für eine automatisierte Weiterverarbeitung der Vorgänge in BaufiSmart.

Zuerst muss ein Vorgang über die Kundenangaben-API in BaufiSmart angelegt werden. Dabei sind die Daten für alle Bestandsdarlehen zu übermitteln. In der Antwort auf die Vorgangs-Anlage werden die in der Datenbank angelegten Ids an den Client unter _referenzId_ im Knoten _BestehendesDarlehenDesFinanzierungsobjektes_ zurück übermittelt. Diese Ids sind anschließend in der #passt Vorschlags API als Bestandteil der Darlehensliste zu übergeben.

```
"referenzId": "eb2f1288-43c6-4ead-82d4-d63889a8d827"
````

Bsp. der Darlehensliste der Vorschläge-API mit Referenz zu den Bestandsdarlehen aus der Kundenangaben API
```
 "darlehensliste": [
                {
                    "wirdAbgeloest": true,
                    "referenzId": "eb2f1288-43c6-4ead-82d4-d63889a8d827",
                    "darlehensgeber": "PROLOSMART",
                    "grundschuld": 142323.35,
                    "restschuld": {
                        "zumAbloeseTermin": 132323.35
                    },
                    "darlehenskontonummer": "Darlehen_1"
                },
                {
                    "wirdAbgeloest": true,
                    "referenzId": "ac5f458-48c7-8541-82d4-c4388d38d452",
                    "darlehensgeber": "PROLOSMART",
                    "grundschuld": 142323.35,
                    "restschuld": {
                        "zumAbloeseTermin": 45482.80
                    },
                    "darlehenskontonummer": "Darlehen_2"
                }]
```



