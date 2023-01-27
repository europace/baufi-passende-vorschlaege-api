# Vorschläge-API ermittelt unterschiedliche Optionen

Die Vorschläge-API ist in der Lage ein breites Spektrum an an Finanzierungsvorschlägen gleichzeitig zu generieren und zweidimensional sortiert zurückzugeben. Dabei kann sich sowohl die Zinsbindung als auch die Tilgung unterscheiden.

Zur Aktivierung der Optionsermittlung ist in den Metadaten der dafür notwendige Modus "exploration-matrix" zu aktivieren:
```
{
    "metadaten": {
        "datenkontext": "TEST_MODUS",
        "mode": "exploration-matrix"
    }
}
```

Stand 27.01.2023: Aktivierung über "feature": "MINIMAL_SCORING_ENGINE, optionen"

## Anfrage

Die Anfrage muss neben dem Mode keine weiteren besonderen Bedingungen erfüllen. Durch Vorgabe der Präferenzen "zinsbindungInJahren" oder "tilgung" kann der Rahmen der Optionsermittlung vorgegeben werden. Eine Präferenz Zinsbindung von 10 Jahren erzeugt Optionen von 5, 10 und 15 Jahren Zinsbindung. Wird eine Zinsbindung größer 15 Jahre vorgegeben, z.B. 20 Jahre, werden Optionen zu 10, 15 und 20 Jahre erzeugt. 
Eine Vorgabe von Tilgungssätzen für die Optionsermittlung ist nicht empfehlenswert, da hier standardmäßig die minimal möglichen Tilgungen (gewöhnlich 1%) als untere Grenze und eine 1/3 des verfügbaren Haushaltsnettoeinkommen entsprechende Rate als obere Grenze verwendet wird. Bei entsprechend hohem Einkommen werden  automatisch Volltilger-Optionen als Maximal-Tilgung generiert.

## Antwort

Im Modus "exploration-matrix" wird immer versucht, 9 Vorschläge zu generieren und in einer 3x3 Matrix anzuordnen. Die Position in der Ergebnis-Matrix wird über das Feld "vorschlagsOption" definiert:
|| || Tilg niedrig || Tilg mittel  || Tilg hoch ||
|| ZiBi kurz || "kurz-niedrig" | "kurz-mittel" | "kurz-hoch" |
|| ZiBi mittel || "mittel-niedrig" | "mittel-mittel" | "mittel-hoch" |
|| ZiBi lang || "lang-niedrig" | "lang-mittel" | "lang-hoch" |
