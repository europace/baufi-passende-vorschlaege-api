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
