# Generelle Eigenschaften der Meldungen
Alle in den API Antworten vorkommende Meldungstexte sind für Endverbraucher geeignet. Auf Fremdwörter und Fachbegriffe wird soweit wie möglich verzichtet.
Die Meldungen enthalten keine Hinweise auf Produktanbieter oder Details in den Produktabbildungen. Die Texte sind geeigente um von einem KI Modell verarbeitet und interpretiert zu werden.
Sie sorgen für ein tieferes Verständnis für die Finanzierung bei der Nutzung von LLM für die Beratungsunterstützung und verhindern gleichzeitig, dass unerwünschte Detailinformationen dem Modell übermittelt werden.

# Meldungen an der Ergebnisliste
Für das Ergebnis-Set an Vorschlägen wird in der API Antwort ein Block an Bewertungsmeldungen als separater Knoten mitgegeben.
Diese sind unabhängig von den einzelnen Vorschlägen und bewerten direkt die Eingabedaten nachdem Nebenkosten und Darlehensbetrag ermittelt wurden.

Beispiel:
```
    "erklaerungen": {
        "bewertungen": [
            {
                "code": "bewertung.nekfaktor",
                "text": "Das Verhältnis von Kreditsumme zu Nettoeinkommen beträgt 91 und liegt damit unter dem empfohlenen Höchstwert von 120."
            },
            {
                "code": "bewertung.ekanteil",
                "text": "Die Summe der aufzubringenden Nebenkosten beträgt 38.577,00 € bzw. 11,9 % vom Kaufpreis. Der Anteil des eingebrachten Eigenkapitals am Kaufpreis inkl. Nebenkosten liegt bei 0,4 %."
            },
            {
                "code": "bewertung.kpa",
                "text": "Aus dem eingebrachten Eigenkapital und den aufzubringenden Nebenkosten ergibt sich, dass 111,7 % des Kaufpreises durch das Darlehen finanziert werden müssen. Dieser Wert liegt über dem empfohlenen Höchstwert von 95 %."
            }
        ],
```


# Meldungen am Vorschlag
Für jeden einzelnen Vorschlag werden die zentralen Berechnungsergebnisse in Relationen gesetzt und mit üblichen Faustformeln verglichen. Daraus ergibt sich eine umfassende Bewertung des Ergebnisses.
Sollte der Finanzierungsvorschlag zusätzlich Machbarkeits-Einschränkungen aufweisen, werden diese unter dem Knoten "machbarkeit" aufgelistet und erläutert.

Beispiel:
```
           "optimierungsMeldungen": {
                "bewertungen": [
                    {
                        "code": "bewertung.restschuld",
                        "text": "Nach Ende der Zinsbindung beträgt die Restschuld des Darlehens noch 77,3 % der Darlehenssumme."
                    },
                    {
                        "code": "bewertung.gesamtkosten",
                        "text": "Bei gleichbleibendem Sollzins betragen die Gesamtkosten der Finanzierung bis zum Laufzeitende am 31.05.2053 das 1,83-fache der Darlehenssumme."
                    },
                    {
                        "code": "bewertung.rate",
                        "text": "Die Rate liegt bei 51,4 % des Netto-Haushaltseinkommen und liegt damit über dem empfohlenen Maximalwert von 35 %."
                    }
                ],
                "machbarkeit": [
                    {
                        "code": "finanzierung.beleihungsauslauf",
                        "text": "Das Verhältnis von Darlehenssumme zu Immobilienwert ist zu hoch. Durch Einbringen von mehr Eigenkapital kann der Beleihungsauslauf reduziert werden."
                    }
                ]
            },

```

# Meldungen zur Erklärung und Verwendung im Mode "exploration-matrix"
In diesem Modus werden in jeder Antwort zusätzlich Positionsbeschreibungen für jede der 9 Matrix-Positionen mitgeliefert. Diese sollen helfen, die Matrix-Positionen zu interpretieren und geben Hinweise für wen die jeweiligen Vorschläge geeignet sind. Dabei werden für jede Matrix-Position neben der Beschreibung bis zu 3 Vor- und Nachteile der jeweiligen Vorschlagsposition aufgelistet. Der der erste Teil des Codes der Matrix-Position referenziert dabei auf den Wert "vorschlagsOption" im Finanzierungsvorschlag, z.B.: "mittel-mittel"

Jeweils immmer gleicher Textblox:
```
       "matrix": [
            {
                "code": "kurz-hoch.beschreibung",
                "text": "Geringe Zinskosten, Vorschlag bei hohen Einkommen: Finanzierung mit geringsten Gesamtkosten bei gleichbleibenden Zinsen, Zinsänderungsrisiko, Tilgung auf den maximal möglichen Betrag angepasst, geeignet für flexible Lebensplanung bei hohem Einkommen."
            },
            {
                "code": "kurz-hoch.vorteil.flexibel",
                "text": "Durch die kurze Zinsbindung bleibst du für die Zukunft flexibel."
            },
            {
                "code": "kurz-hoch.vorteil.kosten-niedrig",
                "text": "Durch die niedrigen Zinsen sind die Kosten innerhalb der Zinsbindung eher niedrig."
            },
            {
                "code": "kurz-hoch.vorteil.restbetrag-gering",
                "text": "Der Restbetrag des Kredites verringert sich in kurzer Zeit stark."
            },
            {
                "code": "kurz-hoch.zubedenken.belastung-hoch",
                "text": "Der monatlich zu zahlende Betrag für den Kredit ist ziemlich hoch."
            },
            {
                "code": "kurz-hoch.zubedenken.zinsanstieg",
                "text": "Durch die kurze Zinsbindung kann das Risiko steigen, dass sich die Gesamtkosten bei steigenden Zinsen erhöhen."
            },
            {
                "code": "kurz-mittel.beschreibung",
                "text": "Kompromiss aus besonders flexibel und optimierter Tilgung, kurze Zinsbindung sorgt für hohe Flexibilität, die an das Einkommen und Alter angepasste Tilgung reduziert die Restschuld und die Gesamtkosten der Finanzierung."
            },
            {
                "code": "kurz-mittel.vorteil.flexibel",
                "text": "Durch die kurze Zinsbindung bleibst du für die Zukunft flexibel."
            },
            {
                "code": "kurz-mittel.vorteil.sinkende-zinsen",
                "text": "Bei sinkenden Zinsen kannst du profitieren und die Rückzahlung passend zu deinem Budget anpassen, das hilft die Gesamtkosten für den Kredit zu senken."
            },
            {
                "code": "kurz-mittel.zubedenken.zinsanstieg",
                "text": "Bei steigenden Zinsen ist das Risiko größer, dass die Gesamtkosten für den Kredit steigen."
            },
            {
                "code": "kurz-niedrig.beschreibung",
                "text": "Finanzierung mit hoher Flexibilität aber hoher Restschuld, geeignet für flexible Lebensplanung oder als Überbrückungsfinanzierung wenn Bonitätsreserven bei steigenden Zinsen vorhanden sind, lange Gesamtlaufzeit ist zu erwarten."
            },
            {
                "code": "kurz-niedrig.vorteil.belastung-niedrig",
                "text": "Die monatliche Belastung ist eher niedrig."
            },
            {
                "code": "kurz-niedrig.vorteil.flexibel",
                "text": "Durch die kurze Zinsbindung bleibst du für die Zukunft flexibel."
            },
            {
                "code": "kurz-niedrig.zubedenken.laufzeit-lang",
                "text": "Die Rückzahlung des Kredites kann lange dauern."
            },
            {
                "code": "kurz-niedrig.zubedenken.restbetrag-hoch",
                "text": "Am Ende der Zinsbindung bleibt noch ein hoher Restbetrag vom Kredit übrig."
            },
            {
                "code": "kurz-niedrig.zubedenken.zinsanstieg",
                "text": "Bei steigenden Zinsen kann ggf. die weitere Finanzierung teurer werden als vorher."
            },
            {
                "code": "lang-hoch.beschreibung",
                "text": "Lange Zinssicherheit bei hoher Tilgung, je nach verfügbarem Einkommen auch Volltilgung möglich, Kombination aus minimalem Risiko und geringen Zinskosten, dafür höchste Rate, geeignet für hohe Einkommen bei wenig Bedarf an Flexibilität, Sondertilgung nicht vorgesehen."
            },
            {
                "code": "lang-hoch.vorteil.sicherheit-maximal",
                "text": "Es besteht so gut wie kein Risiko, dass sich die Zinsen nochmal ändern."
            },
            {
                "code": "lang-hoch.vorteil.volltilgung",
                "text": "Der Kreditbetrag ist innerhalb der Zinsbindung wahrscheinlich abbezahlt."
            },
            {
                "code": "lang-hoch.zubedenken.belastung-hoch",
                "text": "Der monatlich zu zahlende Betrag für den Kredit ist ziemlich hoch."
            },
            {
                "code": "lang-mittel.beschreibung",
                "text": "Sehr sichere Finanzierung bei mittlerer Rate und kleiner Restschuld, höhere Gesamtkosten je nach Zinssituation, geringes Anschlusszins-Risiko, an Einkommen und Alter optimierte Tilgung."
            },
            {
                "code": "lang-mittel.vorteil.belastung-moderat",
                "text": "Monatliche Belastung ist für einen langen Zeitraum moderat und passt gut zum aktuell verfügbaren Einkommen."
            },
            {
                "code": "lang-mittel.vorteil.restbetrag-niedrig",
                "text": "Der Restbetrag am Ende der Zinsbindung ist eher niedrig und in einem kurzen Zeitraum bezahlbar."
            },
            {
                "code": "lang-mittel.vorteil.sicherheit-belastung",
                "text": "Durch die lange Zinsbindung besteht eine höhere Sicherheit bei der monatlichen Belastung."
            },
            {
                "code": "lang-mittel.zubedenken.kosten-hoch",
                "text": "Die Gesamtkosten des Kredites können je nach Zinsniveau etwas höher sein."
            },
            {
                "code": "lang-mittel.zubedenken.zinsen-hoch",
                "text": "Die Zinsen sind etwas höher aufgrund der langen Zinsbindung."
            },
            {
                "code": "lang-niedrig.beschreibung",
                "text": "Ausgewogen für Jüngere (geringe Rate): Finanzierung mit einer konstanten Rate für eine längere Zeit, Kompromiss aus günstig und sicher, Sondertilgung kann Flexibilität erhöhen und sowohl Gesamtlaufzeit als auch Gesamtkosten reduzieren."
            },
            {
                "code": "lang-niedrig.vorteil.belastung-niedrig",
                "text": "Die monatliche Zahlung ist über einen langen Zeitraum niedrig."
            },
            {
                "code": "lang-niedrig.vorteil.sicherheit-belastung",
                "text": "Hohe Sicherheit über die Höhe der monatlichen Belastung."
            },
            {
                "code": "lang-niedrig.vorteil.sondertilgung",
                "text": "Wenn mal mehr Budget zur Verfügung steht kann durch Sonderzahlungen die Laufzeit des Kredites kürzer werden."
            },
            {
                "code": "lang-niedrig.zubedenken.nicht-abbezahlt",
                "text": "Der Kredit ist am Ende der Zinsbindung wahrscheinlich noch nicht abbezahlt."
            },
            {
                "code": "mittel-hoch.beschreibung",
                "text": "Sichere Finanzierung und geringe Zinskosten bei hoher Rate, minimales Anschlusszins-Risiko, sehr kleine Restschuld, Sondertilgung nicht nötig."
            },
            {
                "code": "mittel-hoch.vorteil.keine-sondertilgung",
                "text": "Extrazahlungen sind eher nicht nötig."
            },
            {
                "code": "mittel-hoch.vorteil.kosten-niedrig",
                "text": "Durch die geringeren Zinsen sind die Kosten innerhalb der Zinsbindung eher niedrig."
            },
            {
                "code": "mittel-hoch.vorteil.restbetrag-niedrig",
                "text": "Der Restbetrag des Kredites am Ende der Zinsbindung ist niedrig und in einem kurzen Zeitraum bezahlbar."
            },
            {
                "code": "mittel-hoch.zubedenken.belastung-hoch",
                "text": "Der monatlich zu zahlende Betrag für den Kredit ist ziemlich hoch."
            },
            {
                "code": "mittel-mittel.beschreibung",
                "text": "Ausgewogen für Finanzierende im mittleren Alter (an Einkommen angepasste Tilgung), Mittlere Zinsbindung und leicht erhöhte Rate reduzieren die Gesamtkosten und senken das Anschlusszins-Risiko, optimierte Kalkulation zwischen Sicherheit und Kosten."
            },
            {
                "code": "mittel-mittel.vorteil.belastung-moderat",
                "text": "Monatliche Belastung ist moderat und passt gut zum aktuell verfügbaren Einkommen."
            },
            {
                "code": "mittel-mittel.vorteil.restbetrag-bezahlbar",
                "text": "Der Restbetrag des Kredites am Ende der Zinsbindung ist in einem angemessenen Zeitraum bezahlbar."
            },
            {
                "code": "mittel-mittel.vorteil.sicherheit-belastung",
                "text": "Die monatliche Belastung ist über einen längeren Zeitraum sicher."
            },
            {
                "code": "mittel-mittel.zubedenken.nicht-abbezahlt",
                "text": "Der Kredit ist am Ende der Zinsbindung wahrscheinlich noch nicht abbezahlt."
            },
            {
                "code": "mittel-niedrig.beschreibung",
                "text": "Ausgewogene Finanzierungen bei geringem Budget, lange Gesamtlaufzeit möglich, Sondertilgung zur Reduzierung von Laufzeit und Gesamtkosten empfehlenswert."
            },
            {
                "code": "mittel-niedrig.vorteil.budget-niedrig",
                "text": "Auch mit weniger verfügbarem Budget möglich."
            },
            {
                "code": "mittel-niedrig.vorteil.sondertilgung",
                "text": "Wenn mal mehr Budget zur Verfügung steht kann durch Sonderzahlungen die Laufzeit des Kredites kürzer werden."
            },
            {
                "code": "mittel-niedrig.zubedenken.laufzeit-lang",
                "text": "Die Rückzahlung des Kredites kann lange dauern."
            },
            {
                "code": "mittel-niedrig.zubedenken.restschuld-hoch",
                "text": "Am Ende der Zinsbindung bleibt immer noch ein eher hoher Restbetrag vom Kredit übrig (Restschuld)."
            }
        ]
```
