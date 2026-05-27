# Herstellungskosten bei Neubau-Anfragen

Bei einem eigenen Neubauvorhaben wird im Knoten `kundenangaben.finanzierungsbedarf` der Finanzierungszweck `NEUBAU` verwendet. Die Baukosten werden nicht als `kaufpreis`, sondern im Feld `herstellungskostenInklusiveEigenleistungen` übertragen.

```json
{
  "kundenangaben": {
    "finanzierungsbedarf": {
      "finanzierungszweck": "NEUBAU",
      "herstellungskostenInklusiveEigenleistungen": 350000,
      "grundstueckKaufpreis": 120000,
      "grundstueckBereitsBezahlt": false
    },
    "finanzierungsobjekt": {
      "objektArt": "EINFAMILIENHAUS",
      "anschrift": {
        "plz": "10179"
      },
      "wohnflaeche": 150
    }
  }
}
```

## Feldverwendung

- `finanzierungszweck`: Für ein eigenes Neubauvorhaben `NEUBAU` setzen.
- `herstellungskostenInklusiveEigenleistungen`: Gesamte Herstellungskosten des Neubaus in Euro, inklusive Eigenleistungen. Bei `NEUBAU` muss der Wert größer als `0` sein.
- `grundstueckKaufpreis`: Grundstücksanteil in Euro, falls ein Grundstück mitfinanziert oder für die Nebenkostenberechnung berücksichtigt werden soll.
- `grundstueckBereitsBezahlt`: `true`, wenn das Grundstück bereits bezahlt ist. `false`, wenn es noch bezahlt oder finanziert werden muss.
- `kaufpreis`: Bei `NEUBAU` nicht für die Herstellungskosten verwenden. Dieses Feld ist für Erwerbsvorgänge wie `KAUF` oder `KAUF_NEUBAU_VOM_BAUTRAEGER` vorgesehen.

## Nebenkosten

Bei `NEUBAU` werden Nebenkosten nur auf den Grundstücksanteil berechnet, wenn das Grundstück noch nicht bezahlt ist. Auf Herstellungskosten, Modernisierungskosten und Eigenleistungen werden keine Nebenkosten angerechnet.

Für die Nebenkostenberechnung muss der Standort des Objekts über `plz` oder `bundesland` angegeben werden.
