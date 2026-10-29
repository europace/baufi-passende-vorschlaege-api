# Features der Passende-Vorschläge-API

Stand: 29.10.2026.

## Übergabe

Features werden unter `metadaten.feature` als kommagetrennter String übergeben. Die Schreibweise einschließlich Groß-/Kleinschreibung muss übereinstimmen.

Beispiel als Ausschnitt eines API-Requests:

```json
{
  "metadaten": {
    "feature": "ALL_TRADE_RELATIONS,DISABLE_PREFERRED_PA_PRIORIZATION"
  }
}
```

## Verfügbare Features

| Feature-Wert | Kurzbeschreibung |
| --- | --- |
| `ALL_TRADE_RELATIONS` | Verwendet alle ursprünglichen Handelsbeziehungen mit `isErmittelnErlaubt = true`, deren Produktanbieter außerhalb von `blacklistHandelsbeziehungen` liegen. Überspringt die Modellauswahl für Handelsbeziehungen. |
| `ONLY_PREFERRED_TRADE_RELATIONS` | Beschränkt die erlaubten Handelsbeziehungen auf die IDs aus `kundenangaben.finanzierungsbedarf.praeferenzen.produktAnbieterIds`. Überspringt die Modellauswahl für Handelsbeziehungen. |
| `DISABLE_PREFERRED_PA_PRIORIZATION` | Schaltet die Sonderberücksichtigung präferierter Produktanbieter bei der Auswahl der Ergebnisvorschläge ab. Die Auswahl erfolgt anhand der Scoring-Rangfolge. |
| `ENABLE_CACHE` | Aktiviert die Wiederverwendung einer vorhandenen Anfrage beim asynchronen Vorschlagsaufruf. Bei einem Cache-Treffer wird deren Anfrage-ID zurückgegeben; bei einem Miss startet eine neue Berechnung. Ergebnisse erhalten die konfigurierte Cache-TTL. |
| `no-logging` | Unterdrückt die Datenablage-Events für Anfrage, Metadaten, Angebote, Scorings und Vorschläge sowie die damit verbundene Logging-Ablage. Betriebslogs und Laufzeitmetriken bleiben aktiv. |
| `DISABLE_LEADRATING` | Überspringt die Berechnung des LeadRatings für Erfolg und Aufwand im PostMe-Workflow. Das Angebots-Scoring bleibt aktiv. |
| `PASST_OPTIONEN` | Aktiviert für unterstützte Anfragen und Anbieter zusätzliche Finanzierungsvarianten mit abweichender Zinsbindung, Tilgung oder deren Kombination. Die Varianten werden im Ergebnis entsprechenden Optionen zugeordnet. |
| `PASST_ENABLE_EXTENDED_MATRIX` | Aktiviert den Matrixmodus im Scoring. Wird beim Modus `exploration-matrix` gemeinsam mit `PASST_OPTIONEN` ergänzt. |
| `ERMITTLUNG_KAUFPREIS_GRENZE` | Aktiviert die iterative Kaufpreisgrenzen-Ermittlung. Bei Kaufvorhaben wird ein machbarer Kaufpreis gesucht; der Response enthält `maximalKaufpreis` und die zugehörigen Nebenkosten. |
| `COMPLETE_SCORING` | Verwendet die vollständige Scoring-Engine anstelle der standardmäßig verwendeten Optionen-Scoring-Engine. |

## Automatische Ergänzungen durch `metadaten.mode`

`mode` wird separat von `feature` übergeben. Explizite Features bleiben erhalten.

| Mode-Wert | Automatisch ergänzte Features |
| --- | --- |
| `exploration-matrix` | `PASST_OPTIONEN`, `PASST_ENABLE_EXTENDED_MATRIX` |
| `maximum-offer` | `DISABLE_LEADRATING`, `ERMITTLUNG_KAUFPREIS_GRENZE` |

## Kombinationen und Besonderheiten

- Bei gleichzeitiger Übergabe von `ALL_TRADE_RELATIONS` und `ONLY_PREFERRED_TRADE_RELATIONS` hat `ALL_TRADE_RELATIONS` Vorrang.
- Bei `ONLY_PREFERRED_TRADE_RELATIONS` führen ungültige oder nicht erlaubte Anbieter-IDs zu einem Fehler. Fehlende oder leere Anbieterpräferenzen lassen den erlaubten Bestand unverändert.
- Beide Handelsbeziehungs-Features liefern `useModel: false` und `maxTradeRelations: 0`. Die Null leert die Anbieterliste nicht. Provisionswünsche und Angebots-Scoring bleiben wirksam.
- `ALL_TRADE_RELATIONS` deaktiviert die bevorzugte Berücksichtigung von Anbietern in den Ergebnisvorschlägen nicht automatisch. Dafür ist zusätzlich `DISABLE_PREFERRED_PA_PRIORIZATION` erforderlich.
- `default` ist der Schema-Standardwert für `feature` und aktiviert selbst keine Funktion.
