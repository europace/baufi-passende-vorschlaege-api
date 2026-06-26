# Anschlussfinanzierung: abzulösende Darlehen

Bei einer Anschlussfinanzierung wird im Knoten `kundenangaben.finanzierungsbedarf` der Finanzierungszweck `ANSCHLUSSFINANZIERUNG` verwendet. Die abzulösenden Bestandsdarlehen werden im Knoten `kundenangaben.finanzierungsobjekt.darlehensliste` übertragen.

```json
{
  "kundenangaben": {
    "finanzierungsbedarf": {
      "finanzierungszweck": "ANSCHLUSSFINANZIERUNG"
    },
    "finanzierungsobjekt": {
      "objektArt": "EINFAMILIENHAUS",
      "anschrift": {
        "plz": "10179"
      },
      "marktwert": 300000,
      "darlehensliste": [
        {
          "wirdAbgeloest": true,
          "darlehensgeber": "MUSTERBANK",
          "grundschuld": 180000,
          "zinsbindungBis": "2028-12-31",
          "laufzeitende": "2045-12-31",
          "restschuld": {
            "zumAbloeseTermin": 150000
          },
          "darlehenskontonummer": "Darlehen_1"
        }
      ]
    }
  }
}
```

## Pflichtfelder und Pflichtlogik

- `finanzierungszweck`: Für Anschlussfinanzierung `ANSCHLUSSFINANZIERUNG` setzen.
- `finanzierungsobjekt.marktwert`: Bei Anschlussfinanzierung erforderlich. Wenn kein belastbarer Marktwert vorliegt, kann für einfache Prolongationsfälle ein technischer Wert verwendet werden.
- `finanzierungsobjekt.darlehensliste`: Muss mindestens ein Darlehen enthalten, das abgelöst wird.
- `darlehensliste[].wirdAbgeloest`: Für mindestens ein Darlehen `true` setzen. Nur diese Darlehen bestimmen den Ablösebetrag der Anschlussfinanzierung.
- `darlehensliste[].restschuld.zumAbloeseTermin`: Restschuld des abzulösenden Darlehens zum Ablösetermin in Euro. Der Wert muss größer als `0` sein.
- `darlehensliste[].zinsbindungBis`: Datum des Zinsbindungsendes. Dieses Datum ist der relevante Umschuldungs- bzw. Ablösetermin.
- `darlehensliste[].laufzeitende`: Kalkuliertes Ende der ursprünglichen Gesamtlaufzeit. Das Feld wird für regulatorische Vorgaben der Produktanbieter benötigt.

## Weitere Darlehensfelder

- `darlehensgeber`: Produktanbieter-ID des bisherigen Darlehensgebers, wenn bekannt.
- `grundschuld`: Eingetragene Grundschuld in Euro.
- `darlehenskontonummer`: Bezeichnung oder Kontonummer des abzulösenden Darlehens.
- `referenzId`: ID eines Bestandsdarlehens aus der Kundenangaben-API. Diese ID ermöglicht die Zuordnung zwischen Alt- und Neudarlehen bei automatisierter Weiterverarbeitung in BaufiSmart.
- `rateMonatlich`: Monatliche Rate für Darlehen, die nicht abgelöst werden.
- `restschuld.aktuell`: Aktuelle Restschuld, vor allem für nicht abzulösende Darlehen.

## Mehrere Bestandsdarlehen

Es können mehrere Darlehen in `darlehensliste` übertragen werden. Darlehen mit `wirdAbgeloest: true` fließen in die Anschlussfinanzierung ein. Darlehen mit `wirdAbgeloest: false` bleiben bestehen und werden nicht in den Ablösebetrag eingerechnet.
