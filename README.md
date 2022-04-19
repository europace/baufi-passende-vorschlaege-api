# BaufiSmart-passende-Vorschlaege-API

Die API ermittelt passender Finanzierungsvorschläge anhand einer Verbraucher-Situation und -Präferenzen.

![Status](https://img.shields.io/badge/Status-preAlpha_Pilot-darkred)

![Vertrieb](https://img.shields.io/badge/-Vertrieb-lightblue)
![Baufinanzierung](https://img.shields.io/badge/-Baufinanzierung-lightblue)

[![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://github.com/europace/authorization-api)
[![GitHub release](https://img.shields.io/github/v/release/europace/baufi-passende-vorschlaege-api)](https://github.com/europace/baufi-passende-vorschlaege-api/releases)
[![Pattern](https://img.shields.io/badge/Pattern-Tolerant%20Reader-yellowgreen)](https://martinfowler.com/bliki/TolerantReader.html)

## Dokumentation

[![YAML](https://img.shields.io/badge/OAS-HTML_Doc-lightblue)](https://europace.github.io/baufi-passende-vorschlaege-api/gh-pages/index.html)
[![YAML](https://img.shields.io/badge/OAS-YAML-lightgrey)](https://raw.githubusercontent.com/europace/baufi-passende-vorschlaege-api/master/baufi-passende-vorschlaege-api.yaml)

## Anwendungsfälle der API

- liefert passende Finanzierungsvorschläge auf Basis der Kundensituation
- merken von bis zu 3 Finanzierungsvorschlägen zur weiteren Beratung

### Authentifizierung

Bitte benutze [![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/baufinanzierung/authentifizierung/), um Zugang zur API bekommen. Um
die API verwenden zu können, benötigt der OAuth2-Client folgende Scopes:

| Scope                                | API Usecase                                 |
| -------------------------------------- | --------------------------------------------- |
| `baufinanzierung:angebote:ermitteln` | passende Finanzierungsvorschläge ermitteln |

## Beispiel: passende Finanzierungsvorschläge ermitteln

Das Erlebnis bei der Ermittlung von passenden Finanzierungsvorschlägen ist mitentscheidend für den Erfolg des Leads. Damit der Benutzer eine schnelle Rückmeldung bekommt, wird die
Ermittlung asynchron angeboten.

Im ersten Schritt übermittelst du die relevanten Daten für die Ermittlung und im zweiten Schritt holst du die Ergebnisse der Ermittlung ab. Dabei kann es vorkommen, dass die
Ermittlung noch nicht beendet wurde. Diesen Zustand erkennst du am Statuscode=202 Accepted, ansonsten bekommst du den Statuscode=200 OK.

Bei der Ermittlung der passenden Vorschläge legen wir Wert darauf schnell einen Vorschlag liefern zu können. Es kann deshalb vorkommen, dass das Lead Rating nicht direkt mit dem
passenden Vorschlag zurückgegeben wird und somit auch nicht in der Response enthalten ist. Zu jedem passenden Vorschlag wird immer auch ein Lead Rating ermittelt. Es kann also,
wenn erforderlich, mit einem weiteren Request abgerufen werden.

### Schritt 1: relevante Daten für Ermittlung senden

Request:

```http
POST /vorschlaege HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]

    {
        "metadaten": {
            "datenkontext": "TEST_MODUS",
            "kundenId": "WER03",
            "clientId": "partner-Mobil-App-Ver.2.32",
            "gewuenschteAnzahlVorschlaege": 5
        },
        "kundenangaben": {
            "finanzierungsbedarf": {
                "finanzierungszweck": "KAUF",
                "grundstueckKaufpreis": 0,
                "modernisierungsKostenInklEigenleistungen": 5000,
                "modernisierungEigenleistung":2000,
                "kaufpreis": 350000,
                "praeferenzen": {
                    "wunschRate": 1000,
                    "faelligkeitsDatum": "2022-05-01",
                    "kreditEntscheidungsZeit": "2022-03-01",
                    "laufzeit": 37
                }
            },
            "finanzierungsobjekt": {
                "objektArt": "EIGENTUMSWOHNUNG",
                "vermietet": false,
                "baujahr": 2014,
                "gewerblicheNutzung": false,
                "anschrift": {
                    "plz": "10245",
                    "ort": "unbekannt",
                    "strasse": "unbekannt",
                    "hausnummer": "unbekannt"
                },
                "wohnFlaeche": 140
            },
            "haushalte": [
                {
                    "kunden": [
                        {
                            "einkommenNetto": 3200,
                            "beschaeftigtSeit": "2021-10-01",
                            "arbeitBefristet": false,
                            "geburtsdatum": "1974-01-01",
                            "beschaeftigungsArt": "ANGESTELLTER"
                        },
                        {
                            "einkommenNetto": 2500,
                            "beschaeftigtSeit": "2020-09-01",
                            "arbeitBefristet": false,
                            "geburtsdatum": "1971-01-01",
                            "beschaeftigungsArt": "ANGESTELLTER"
                        }
                    ],
                    "finanzielleSituation": {
                        "eigenKapital": 48000,
                        "sonstigeEinnahmen": 300,
                        "nichtAbgeloestePrivateDarlehenRestschuld": 5000,
                        "nichtAbgeloesteRatenkrediteRestschuld": 2000
                    }
                }
          
            ]
        }
    }
```

Response bei Ermittlung in Arbeit:

```http
202 - Accepted
```

Response nach Ermittlung:

```http
200 - OK
```

```json
{
  "anfrageId": "passende-vorschlaege-c5486371-3d1e-43e2-8fc4-db920bde4fef"
}
```

### Schritt 2: Finanzierungsvorschläge abrufen

Mit der `anfrageId` können die passenden Finanzierungsvorschläge abgerufen werden.

Request:

```http
GET /vorschlaege/passende-vorschlaege-c5486371-3d1e-43e2-8fc4-db920bde4fef HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]]
```

Response:

> Hinweis: Banken und Konditionen sind nur Beispiele

```json
{
  "vorschlaege": [
    {
      "finanzierungsVorschlagId": "d550a975da78f73d9e3256352ce0f366",
      "annahmeFrist": "2022-01-14",
      "finanzierungsbausteine": [
        {
          "@type": "ANNUITAETENDARLEHEN",
          "darlehensbetrag": 189000.0,
          "annuitaetendetails": {
                        "zinsbindungInJahren": 15,
                        "tilgung": {
                            "@type": "TILGUNG_IN_PROZENT",
                            "tilgungssatzInProzent": 2.5,
                            "tilgungsbeginn": "2022-02-27"
                        },
                        "sondertilgungJaehrlich": 5.0,
                        "auszahlungszeitpunkt": "2022-02-27"
                    },
                    "bereitstellungszinsfreieZeitInMonaten": 12,
                    "sollZins": 1.53,
                    "effektivZins": 1.56,
                    "rateMonatlich": 634.73,
                    "produktAnbieter": "Deutsche Kreditbank AG"
                }
            ],
            "darlehensSumme": 189000.00,
            "sollZins": 1.530,
            "effektivZins": 1.560,
            "machbarkeit": 100,
            "rank": 0,
            "gesamtRateProMonat": 634.73,
            "zinsbindungInJahrenMinMax": "15"
        },
        {
            "annahmeFrist": "2022-01-17",
            "finanzierungsbausteine": [
                {
                    "@type": "ANNUITAETENDARLEHEN",
                    "darlehensbetrag": 189000.0,
                    "annuitaetendetails": {
                        "zinsbindungInJahren": 15,
                        "tilgung": {
                            "@type": "TILGUNG_IN_PROZENT",
                            "tilgungssatzInProzent": 2.5,
                            "tilgungsbeginn": "2022-02-27"
                        },
                        "sondertilgungJaehrlich": 5.0,
                        "auszahlungszeitpunkt": "2022-02-27"
                    },
                    "bereitstellungszinsfreieZeitInMonaten": 12,
                    "sollZins": 1.54,
                    "effektivZins": 1.57,
                    "rateMonatlich": 636.3,
                    "produktAnbieter": "Allianz Lebensversicherung AG"
                }
            ],
            "darlehensSumme": 189000.00,
            "sollZins": 1.540,
            "effektivZins": 1.570,
            "machbarkeit": 100,
            "rank": 1,
            "gesamtRateProMonat": 636.3,
            "zinsbindungInJahrenMinMax": "15"
        },
        {
            "annahmeFrist": "2022-01-13",
            "finanzierungsbausteine": [
                {
                    "@type": "ANNUITAETENDARLEHEN",
                    "darlehensbetrag": 189000.0,
                    "annuitaetendetails": {
                        "zinsbindungInJahren": 15,
                        "tilgung": {
                            "@type": "TILGUNG_IN_PROZENT",
                            "tilgungssatzInProzent": 2.5,
                            "tilgungsbeginn": "2022-02-27"
                        },
                        "sondertilgungJaehrlich": 5.0,
                        "auszahlungszeitpunkt": "2022-02-27"
                    },
                    "bereitstellungszinsfreieZeitInMonaten": 12,
                    "sollZins": 1.33,
                    "effektivZins": 1.36,
                    "rateMonatlich": 603.23,
                    "produktAnbieter": "Commerzbank AG"
                }
            ],
            "darlehensSumme": 189000.00,
            "sollZins": 1.330,
            "effektivZins": 1.360,
            "kennung": "Regional BaufiBest",
            "machbarkeit": 100,
            "rank": 2,
          "gesamtRateProMonat": 603.23,
          "zinsbindungInJahrenMinMax": "15"
        }
  ],
  "leadRating": {
    "rating": "B"
  }
}
```

## Passende Finanzierungsvorschläge merken

Um einen Lead zu einem erfolgreichen Abschluss zu bringen, können bis zu drei Finanzierungsvorschläge in der Europace Platform gemerkt werden, um diese zu einem späteren Zeitpunkt
über BaufiSmart beraten zu können.

Der zu merkende Finanzierungsvorschlag wird einem existierenden Vorgang in der Europace Plattform zugeordnet. Du kannst über die Kundenangaben API einen Vorgang anlegen und den
gelieferten Identifier des Vorgangs für das Ablegen des Finanzierungsvorschlages verwenden.

Es können bis zu drei Finanzierungsvorschläge zu einem Vorgang gemerkt werden. Ein bereits gemerkter Finanzierungsvorschlag wird überschrieben. Wurden bereits drei
Finanzierungsvorschläge gemerkt, wird jeder weitere Finanzierungsvorschlag ignoriert.

### Beispiel: Bis zu 3 der passenden Finanzierungsvorschläge merken

Mit der `vorgangId` zu einem vorhandenen Vorgang, der `anfrageId` aus der Ermittlung der Finanzierungsvorschläge und der `finanzierungsVorschlagId` kann ein passender
Finanzierungsvorschlag gemerkt werden.

Request:

```http
POST /vorschlaege HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]

    {
      "anfrageId": "passende-vorschlaege-c5486371-3d1e-43e2-8fc4-db920bde4fef",
      "finanzierungsVorschlagId": "d550a975da78f73d9e3256352ce0f366",
      "vorgangId": "TEST_VORGANG_ID"
    }
```

Response:

```http
200 - OK
```

```
    {
        "message": "Vorschlag abgelegt"
    }
```

## Client generieren

Ein Client kann mit Hilfe der [.yaml-Datei](api/baufi-passende-vorschlaege-api.yaml) über entsprechende Libraries generiert werden. z.B. :

- [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator)
- [Swagger Codegen](https://github.com/swagger-api/swagger-codegen)
- Plugins, wie dem [OpenAPI Generator für Gradle](https://github.com/OpenAPITools/openapi-generator/tree/master/modules/openapi-generator-gradle-plugin)

In der [build.gradle](build.gradle) ist exemplarisch der Task `openApiGenerate` für JavaScript konfiguriert.

Nach Ausführung via `./gradlew openApiGenerate` finden sich im Ordner `/build/generated/src/` die generierten Modelle und Client.

## Support

Bei Fragen oder Problemen kannst du dich an devsupport@europace2.de wenden.

## Nutzungsbedingungen

Die APIs werden unter folgenden [Nutzungsbedingungen](https://docs.api.europace.de/nutzungsbedingungen/) zur Verfügung gestellt.

## Datenschutz

Die Passende-Vorschläge-API ist fachlich darauf ausgelegt, DSGVO konform ohne personenbezogene Daten auszukommen. Dazu sind folgende Bedingungen auf Seiten des Senders einzuhalten um eine Rückverfolgbarkeit oder ein Tracking prinzipiell auszuschließen. Europace prüft die Anfragen stichprobenartig auf Einhaltung der Bedingungen. Da Europace von der Nicht-Nachverfolgbarkeit der Verbraucher ausgeht, finden auch keine entsprechenden Tracking-Verfahren statt.

### Fachliche Bedingungen für nicht-personenbeziehbare Anfragen

- Metadaten sollen keine Tracking Ids enthalten, technische Ids (Session-Marker) werden nach max. 24h ungültig.
- Grundsätzlich werden die Content-Daten der Requests nur max. 24h gespeichert, es erfolgt keine Archivierung der Anfragen
- Personenbeziehbare Daten der Verbraucher sollen durch Rundung zusätzlich pseudonymisiert werden, dabei sind folgende Mindestanforderungen einzuhalten
- Geburtsdatum als beliebiger Tag im Jahr (Bsp.: 1991-01-01)
- Nettoeinkommen auf 100€ gerundet
- BeschäftigtSeit mindestens auf Monat runden (je länger zurückliegend umso ungenauer kann der Wert sein)
- Eigenkapital auf 1000€ runden, bei Werten ab 1 Mio auch auf 10.000€
- Kredit-Restschuld auf 1000€ runden
- sontige Einnahmen auf 100€ runden
- Detaillierte Adressdaten zur Immobilie werden für die Berechnung nicht benötigt, lediglich die Postleitzahl oder alternativ die Auswahl eines Bundeslandes wird benötigt um realistische Nebenkosten zu berechnen und regionale Angebote zu ermöglichen
- Technische Parameter des Objektes (Wohnfläche, Baujahr) können gerundet werden, sind aber für die Personenbeziehbarkeit des Verbrauchers nicht relevant, da zum Zeitpunkt der Anfrage keine Beziehung zwischen Verbraucher und Objekt besteht.
