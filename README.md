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

### Metadaten
Über die Metadaten kann das Verhalten der API beeinflusst werden. 
Durch die Angabe einer mit uns abgestimmmten kundenId können partnerspezifische Funktionen aufgerufen werden. Die clientId ermöglicht unterschiedliche Antworten oder Verhalten der API bei mehreren Client-Applikationen (web/mobile) eines Kunden/Partners oder das Erkennen bestimmter App-Versionen.
Die "gewünschteAnzahlVorschlaege" ist mit 2 vorbelegt. Die API kann bis zu 10 Vorschläge liefern.
```http
 "metadaten": {
    "datenkontext": "TEST_MODUS",
    "extKundenId": "PartnerBank",
    "extClientId": "prolo-demo-test-dummy-001",
    "gewuenschteAnzahlVorschlaege": 5
  },
```

### Unterscheidung zwischen technischen Pflichtfeldern und fachlich notwendigen Daten für sinnvolle Antworten
Folgende Werte sind technisch nicht als Pflichfeld definiert aber je nach Anwendungszweck notwendig oder zumindest empfehlenswert.

#### Verwendungszweck Kauf, Neubau von Bauträger (Erwerb einer Immobilie)
- die Bonitätsinformationen zum Verbraucher werden benötigt (auch Näherungswerte)

```http
"kunden": [
              {
                "einkommenNetto": 3200,
                "geburtsdatum": "1974-01-01",                
               }
          ],
"finanzielleSituation":
          {
              "eigenKapital": 48000
          }
``` 

- folgende technischen Informationen zur Immobilie werden benötigt

```http
 "finanzierungsobjekt": {
                "objektArt": "EIGENTUMSWOHNUNG",
                "anschrift": {
                    "plz": "10245",
                }
``` 

- Wird die API verwendet um ein LeadRating zur Bewertung der Anfrage zu erzeugen, sind weitere Daten zum Verbraucher und zur Immobilie wichtig aber nicht zwingend notwendig. Damit wird eine exakte Bewertung möglich (Daten zur finanziellen Situation sofern vorhanden).

```http
"kunden": [
           {
              "beschaeftigtSeit": "2021-12-01",
              "arbeitBefristet": false,
              "beschaeftigungsArt": "ANGESTELLTER"
           }
          ],
"finanzielleSituation":
          {
              "sonstigeEinnahmen": 0,
              "nichtAbgeloesteRatenkrediteRestschuld": 0
          },
 "finanzierungsobjekt":
          {
              "vermietet": false,
              "baujahr": 2014,
              "gewerblicheNutzung": false,       
              "wohnflaeche": 158.5
           }
``` 


#### Verwendungszweck Anschlussfinanzierung (Prolongation)

Sollen nur Prolongations-Angebote erzeugt werden sind keine Bonitätsinformationen zu Verbraucher:innen notwendig, an der Immobilie wird dann lediglich die Postleitzahl benötigt um eine regionale Einschränkung treffen zu können.
- sollen zusätzlich auch Umschuldungsangebote generiert werden, sind die Bonitätsdaten wie beim Immobilienerwerb notwendig
- weiterhin sind für Anschlussfinanzierungen die Daten zu den Bestandsdarlehen und der Marktwert/Verkehrswert der Immobilie anzugeben

```http     
     "marktwert": 355000,
     "darlehensliste": [
        {
          "wirdAbgeloest": true,
          "darlehensgeber": "SPARDA_BW",
          "grundschuld": 290000,
          "zinsbindungBis": "2022-06-30",
          "laufzeitende": "2040-09-30",
          "restschuld": {
            "aktuell": 245879.81,
            "zumAbloeseTermin": 232410,30
          },
          "darlehenskontonummer": "0815-4711"
        }
     ]
```


Ein Beispiel-Request für eine Erwerbs-Finanzierung mit den relevanten Daten zur Lead-Bewertung, besonders relevant sind die Informationen zu Einkommen, Eigenkapital, Beschäftigungsart und -Dauer sowie Objekteigenschaften wie Wohnfläche und Baujahr. Auch wenn es keine Pflichtfelder sind, haben sie signifikanten Einfluss auf die Lead-Bewertung:

```http
POST /vorschlaege HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]

    {
    "metadaten": {
        "datenkontext": "TEST_MODUS",
        "extKundenId": "github-partner",
        "extClientId": "PostmanRuntime-github-partner",
        "gewuenschteAnzahlVorschlaege": 1,
        "stage":  "default"
    },
    "kundenangaben": {
        "finanzierungsbedarf": {
            "finanzierungszweck": "KAUF",
            "grundstueckKaufpreis": null,
            "modernisierungsKostenInklEigenleistungen": null,
            "modernisierungEigenleistung":null,
            "kaufpreis": 487000,
            
            "praeferenzen": { 
                "faelligkeitsDatum": "2022-08-30",
                "kreditEntscheidungsZeit": "2022-08-30",
                "bereitstellungszinsfreieZeit": 3
            }
        },
        "finanzierungsobjekt": {
            "objektArt": "EINFAMILIENHAUS",
            "vermietet": false,
            "baujahr": 2005,
            "gewerblicheNutzung": false,
            "anschrift": {
                "plz": "15711",
                "ort": "KWH",
                "strasse": "unbekannt",
                "hausnummer": "unbekannt"
            },
            "wohnflaeche": 150
        },
        "haushalte": [
            {
                "kunden": [
                    {
                        "einkommenNetto": 2684,
                        "beschaeftigtSeit": "2008-01-01",
                        "arbeitBefristet": false,
                        "geburtsdatum": "1983-05-29",
                        "beschaeftigungsArt": "ANGESTELLTER"
                    },
                    {
                        "einkommenNetto": 3300,
                        "beschaeftigtSeit": "2005-05-01",
                        "arbeitBefristet": false,
                        "geburtsdatum": "1990-03-08",
                        "beschaeftigungsArt": "ANGESTELLTER"
                    }
                ],
                "finanzielleSituation": {
                    "eigenKapital": 70000,
                    "sonstigeEinnahmen": 0,
                    "nichtAbgeloestePrivateDarlehenRestschuld": 0,
                    "nichtAbgeloesteRatenkrediteRestschuld": 0
                }
            }
           
        ]
    }
}
```


Beispiel Request für eine Prolongations-Anfrage mit reduziertem Datenset

```http
POST /vorschlaege HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]

{
  "metadaten": {
    "datenkontext": "TEST_MODUS",
    "extKundenId": "PartnerBank",
    "extClientId": "prolo-demo-test-dummy-001",
    "gewuenschteAnzahlVorschlaege": 5
  },
  "kundenangaben": {
    "haushalte": [
      {
        "kunden": [
          {
            "einkommenNetto": 3500
          }
        ],
        "finanzielleSituation": {
         }
      }
    ],
    "finanzierungsbedarf": {
      "finanzierungszweck": "ANSCHLUSSFINANZIERUNG",
      "praeferenzen": {
        "tilgung": 2
       },
      "darlehenswunsch": 230000
    },
    "finanzierungsobjekt": {
      "objektArt": "EINFAMILIENHAUS",
        "anschrift": {
        "plz": "01855"
       },
      "marktwert": 255800,
      "darlehensliste": [
        {
          "wirdAbgeloest": true,
          "darlehensgeber": "SPARDA_BW",
          "grundschuld": 290000,
          "zinsbindungBis": "2022-06-30",
          "laufzeitende": "2044-09-30",
          "restschuld": {
            "aktuell": 245879.81,
            "zumAbloeseTermin": 230000
          },
          "darlehenskontonummer": "0815-4711"
        }
      ]
    }
  }
}
```

#### Präferenzen in der Anfrage bestimmen das Verhalten des Finanzierungswunschbestimmers

Werden in der Anfrage konkrete Finanzierungswünsche angegeben, so werden die Wünsche bei der Angebotsermittlung berücksichtigt.

```http
     "praeferenzen": {
        "rate": 900,
        "faelligkeitsdatum": "2022-06-30",
        "kreditEntscheidungsZeit": "2022-05-26",
        "laufzeit": 30,
        "zinsbindungInJahren": 15,
        "sonderzahlung": 20000,
        "bereitstellungszinsfreieZeit": 3,
        "tilgung": 2
      },
      "darlehenswunsch": 200000
```

### HTTP Status Codes des Response

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
    "successRating": "C",
    "effortRating": {
      "rating": false,
      "explanations": []
    }
  }
}
```

## Passende Finanzierungsvorschläge bookmarken

Um einen Lead zu einem erfolgreichen Abschluss zu bringen, können bis zu 10 Finanzierungsvorschläge in der Europace Platform gebookmarked werden, um diese zu einem späteren Zeitpunkt
über BaufiSmart beraten zu können.

Der zu bookmarkende Finanzierungsvorschlag wird einem existierenden Vorgang in der Europace Plattform zugeordnet. Du kannst über die Kundenangaben API einen Vorgang anlegen und den
gelieferten Identifier des Vorgangs für das Ablegen des Finanzierungsvorschlages verwenden.

Ein bereits gebookmarkter Finanzierungsvorschlag wird überschrieben. Wurden bereits 10
Finanzierungsvorschläge gebookmarked, wird jeder weitere Finanzierungsvorschlag ignoriert.

### Beispiel: Bis zu 10 der passenden Finanzierungsvorschläge bookmarken

Mit der `vorgangId` zu einem vorhandenen Vorgang, der `anfrageId` aus der Ermittlung der Finanzierungsvorschläge und der `finanzierungsVorschlagId` kann ein passender
Finanzierungsvorschlag gebookmarked werden.

Request:

```http
POST /vorschlag/bookmark HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]

    {
      "anfrageId": "passende-vorschlaege-c5486371-3d1e-43e2-8fc4-db920bde4fef",
      "finanzierungsVorschlagId": "d550a975da78f73d9e3256352ce0f366",
      "vorgangId": "TESTID"
    }
```

Response:

```http
200 - OK
```

```json
{
  "message": "Vorschlag abgelegt."
}
```

## Passende Finanzierungsvorschläge akzeptieren

Um einen Lead zu einem erfolgreichen Abschluss zu bringen, kann maximal ein Finanzierungsvorschlag in der Europace Platform akzeptiert werden, um diese zu einem späteren Zeitpunkt
über BaufiSmart beraten zu können. Der Finanzierungsvorschlag kann so einer sein, der vorher schon gebookmarked wurde oder auch ein neuer.

Der zu akzeptierende Finanzierungsvorschlag wird einem existierenden Vorgang in der Europace Plattform zugeordnet. Du kannst über die Kundenangaben API einen Vorgang anlegen und den
gelieferten Identifier des Vorgangs für das Ablegen des Finanzierungsvorschlages verwenden.

Ein akzeptierter Finanzierungsvorschlag kann mit delete gelöscht werden. So könnte ein anderer Finanzierungsvorschlag akzeptiert werden.

### Beispiel: Bis zu einem der passenden Finanzierungsvorschläge akzeptieren

Mit der `vorgangId` zu einem vorhandenen Vorgang, der `anfrageId` aus der Ermittlung der Finanzierungsvorschläge und der `finanzierungsVorschlagId` kann ein passender
Finanzierungsvorschlag akzeptiert werden. Sollte der Finanzierungsvorschlag bereits gebookmarked sein, ist die `anfrageId` optional.

Request:

```http
POST /vorschlag/accept HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]

    {
      "anfrageId": "passende-vorschlaege-c5486371-3d1e-43e2-8fc4-db920bde4fef",
      "finanzierungsVorschlagId": "d550a975da78f73d9e3256352ce0f366",
      "vorgangId": "TESTID"
    }
```

Response:

```http
200 - OK
```

```json
{
  "message": "Vorschlag akzeptiert."
}
```

## Finanzierungsvorschläge, welche gebookmarked oder akzeptiert worden sind, auflisten

Um zu sehen welche Finanzierungsvorschläge man gebookmarked oder akzeptiert hat, kann man list verwenden. 

### Beispiel: 

Mit der `vorgangId` zu einem vorhandenen Vorgang können alle Vorschläge aufgelistet werden

Request:

```http
GET /vorschlaege/list/TESTID HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]

```

Response:

```http
200 - OK
```

> Hinweis: Banken und Konditionen sind nur Beispiele
```json
{
  "vorschlaege": [
    {
      "finanzierungsVorschlagId": "72027af9f2890bad4506998f37fa7b49",
      "annahmeFrist": "2022-07-13",
      "finanzierungsbausteine": [
        {
          "@type": "ANNUITAETENDARLEHEN",
          "restschuldNachZinsbindungsEnde": 113938.09,
          "schlussrate": 366.82,
          "datumLetzteRate": "2050-12-31",
          "anzahlRaten": 352,
          "tilgungssatzInProzent": 2.03,
          "darlehensbetrag": 189000.0,
          "annuitaetendetails": {
            "zinsbindungInJahren": 15,
            "tilgung": {
              "@type": "RATE",
              "rate": 900.9,
              "tilgungsbeginn": "2022-11-30"
            },
            "sondertilgungJaehrlich": 5.0,
            "auszahlungszeitpunkt": "2022-10-30"
          },
          "bereitstellungszinsfreieZeitInMonaten": 12,
          "sollZins": 3.69,
          "effektivZins": 3.78,
          "rateMonatlich": 900.9,
          "produktAnbieter": "Deutsche Kreditbank AG"
        }
      ],
      "darlehensSumme": 189000.00,
      "sollZins": 3.690,
      "effektivZins": 3.780,
      "gesamtRateProMonat": 900.9,
      "zinsbindungInJahrenMinMax": "15"
    },
    {
      "finanzierungsVorschlagId": "c2d526946ad73efcf27385a363bdbafc",
      "annahmeFrist": "2022-07-05",
      "finanzierungsbausteine": [
        {
          "@type": "ANNUITAETENDARLEHEN",
          "restschuldNachZinsbindungsEnde": 211525.79,
          "schlussrate": 1022.09,
          "datumLetzteRate": "2060-05-31",
          "anzahlRaten": 465,
          "tilgungssatzInProzent": 1.0,
          "darlehensbetrag": 270000.0,
          "annuitaetendetails": {
            "zinsbindungInJahren": 15,
            "tilgung": {
              "@type": "RATE",
              "rate": 1253.25,
              "tilgungsbeginn": "2022-11-30"
            },
            "sondertilgungJaehrlich": 5.0
          },
          "bereitstellungszinsfreieZeitInMonaten": 6,
          "sollZins": 4.57,
          "effektivZins": 4.69,
          "rateMonatlich": 1253.25,
          "produktAnbieter": "ING-DiBa AG"
        }
      ],
      "darlehensSumme": 270000.00,
      "sollZins": 4.570,
      "effektivZins": 4.690,
      "gesamtRateProMonat": 1253.25,
      "zinsbindungInJahrenMinMax": "15"
    },
    {
      "finanzierungsVorschlagId": "feac4e6004939e52dd925d8414ec0a9",
      "annahmeFrist": "2022-01-26",
      "finanzierungsbausteine": [
        {
          "@type": "ANNUITAETENDARLEHEN",
          "restschuldNachZinsbindungsEnde": 104065.92,
          "schlussrate": 463.13,
          "datumLetzteRate": "2053-08-31",
          "anzahlRaten": 392,
          "tilgungssatzInProzent": 2.5,
          "darlehensbetrag": 179000.0,
          "annuitaetendetails": {
            "zinsbindungInJahren": 15,
            "tilgung": {
              "@type": "TILGUNG_IN_PROZENT",
              "tilgungssatzInProzent": 2.5,
              "tilgungsbeginn": "2022-02-28"
            },
            "sondertilgungJaehrlich": 5.0,
            "auszahlungszeitpunkt": "2022-02-27"
          },
          "bereitstellungszinsfreieZeitInMonaten": 3,
          "sollZins": 1.45,
          "effektivZins": 1.48,
          "rateMonatlich": 589.21,
          "produktAnbieter": "Commerzbank AG"
        }
      ],
      "darlehensSumme": 179000.00,
      "sollZins": 1.450,
      "effektivZins": 1.480,
      "kennung": "Regional",
      "gesamtRateProMonat": 589.21,
      "zinsbindungInJahrenMinMax": "15"
    }
  ]
}
```

## Passende Finanzierungsvorschläge löschen

Zum Löschen von gebookmarkten oder akzeptierten Finanzierungsvorschlägen.

### Beispiel: Bis zu einem der passenden Finanzierungsvorschläge akzeptieren

Mit der `vorgangId` zu einem vorhandenen Vorgang und der `finanzierungsVorschlagId` kann der passende
Finanzierungsvorschlag gelöscht werden. 
Request:

```http
POST /vorschlag/delete HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]

    {
      "finanzierungsVorschlagId": "d550a975da78f73d9e3256352ce0f366",
      "vorgangId": "TESTID"
    }
```

Response:

```http
200 - OK
```

```json
{
  "message": "Vorschlag d550a975da78f73d9e3256352ce0f366 erfolgreich gelöscht."
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
