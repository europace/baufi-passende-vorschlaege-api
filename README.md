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

### Authentifizierung
Bitte benutze [![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/baufinanzierung/authentifizierung/), um Zugang zur API bekommen. Um die API verwenden zu können, benötigt der OAuth2-Client folgende Scopes:

| Scope                                  | API Usecase                                         |
|----------------------------------------|-----------------------------------------------------|
| `baufinanzierung:angebote:ermitteln`   | passende Finanzierungsvorschläge ermitteln          |


## Beispiel: passende Finanzierungsvorschläge ermitteln
Das Erlebnis bei der Ermittlung von passenden Finanzierungsvorschlägen ist mitentscheidend für den Erfolg des Leads. Damit der Benutzer eine schnelle Rückmeldung bekommt, wird die Ermittlung asynchron angeboten.

Im ersten Schritt übermittelst du die relevanten Daten für die Ermittlung und im zweiten Schritt holst du die Ergebnisse der Ermittlung ab. Dabei kann es vorkommen, dass die Ermittlung noch nicht beendet wurde. Diesen Zustand erkennst du am Statuscode=202 Accepted, ansonsten bekommst du den Statuscode=200 OK.

### Schritt 1: relevante Daten für Ermittlung senden
Request:
``` http
POST /vorschlaege HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]

{
  "haushalte": [
    {
      "kunden": [
        {
          "beschaeftigtSeit": "2010-01-26T13:52:12.177Z",
          "arbeitBefristet": false,
          "einkommenNetto": 5000,
          "geburtsdatum": "1999-05-26T13:52:12.177Z",
          "beschaeftigungsArt": "ANGESTELLTER"
        }
      ],
      "finanzielleSituation": {
        "eigenKapital": 100000,
        "sonstigeEinnahmen": 0,
        "nichtAbgeloestePrivateDarlehenRestschuld": 0,
        "nichtAbgeloesteRatenkrediteRestschuld": 0
      }
    }
  ],
  "finanzierungsbedarf": {
    "finanzierungszweck": "KAUF",
    "grundstueckKaufpreis": 380000,
    "gesamtKosten": 250000,
    "praeferenzen": {
        "wunschRate": 900,
        "einzugsDatum": "2021-09-26T13:52:12.177Z",
        "kreditEntscheiungsZeit": "2021-05-26T13:52:12.177Z",
        "laufzeit": 120
    }
  },
  "finanzierungsobjekt": {
    "objektArt": "EINFAMILIENHAUS",
    "vermietet": false,
    "baujahr": 2000,
    "gewerbeEinheit": false,
    "anschrift": {
      "plz": "10179",
      "ort": "Berlin",
      "strasse": "Klosterstrasse",
      "hausnummer": "8"
    },
    "wohnFlaeche": 150
  }
}
```

Response bei Ermittlung in Arbeit: 
``` http
202 - Accepted
```

Response nach Ermittlung: 
``` http
200 - OK
```
``` json
{
    "anfrageId": "passende-vorschlaege-c5486371-3d1e-43e2-8fc4-db920bde4fef"
}
```
### Schritt 2: Finanzierungsvorschläge abrufen
Mit der `anfrageId` können die passenden Finanzierungsvorschläge abgerufen werden.

Request:
``` http
GET /vorschlaege/passende-vorschlaege-c5486371-3d1e-43e2-8fc4-db920bde4fef HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]]
```

Response:
> Hinweis: Banken und Konditionen sind nur Beispiele
``` json
{
    "vorschlaege": [
        {
            "annahmeFrist": "2021-09-03",
            "darlehen": [
                {
                    "auszahlungsBetrag": 178930.00,
                    "auszahlungsDatum": "2021-09-29",
                    "darlehensBetrag": 178930.00,
                    "sollZins": 0.870,
                    "effektivZins": 0.890,
                    "produktAnbieter": "COMMERZBANK",
                    "gesamtKosten": 210470.61,
                    "gesamtLaufzeit": 460,
                    "rate": 457.76,
                    "tilgung": 2.200,
                    "typ": "ANNUITAETEN_DARLEHEN",
                    "zinsBindung": 15,
                    "zinsZahlungsBeginnAm": "2021-10-30"
                }
            ],
            "darlehensSumme": 178930.00,
            "sollZins": 0.870,
            "effektivZins": 0.890,
            "kennung": "Regional BaufiBest Green",
            "machbarkeit": 11,
            "rank": 11
        },
        {
            "annahmeFrist": "2021-09-07",
            "darlehen": [
                {
                    "auszahlungsBetrag": 178925.00,
                    "auszahlungsDatum": "2021-09-29",
                    "darlehensBetrag": 178925.00,
                    "sollZins": 1.450,
                    "effektivZins": 1.480,
                    "produktAnbieter": "ALLIANZ",
                    "gesamtKosten": 232253.35,
                    "gesamtLaufzeit": 452,
                    "rate": 514.41,
                    "tilgung": 2.000,
                    "typ": "ANNUITAETEN_DARLEHEN",
                    "zinsBindung": 15,
                    "zinsZahlungsBeginnAm": "2021-10-30"
                }
            ],
            "darlehensSumme": 178925.00,
            "sollZins": 1.450,
            "effektivZins": 1.480,
            "kennung": "Baufinanzierung Basis",
            "machbarkeit": 11,
            "rank": 11
        },
        {
            "annahmeFrist": "2021-09-06",
            "darlehen": [
                {
                    "auszahlungsBetrag": 179000.00,
                    "auszahlungsDatum": "2021-09-29",
                    "darlehensBetrag": 179000.00,
                    "sollZins": 1.360,
                    "effektivZins": 1.390,
                    "produktAnbieter": "DKB",
                    "gesamtKosten": 229558.92,
                    "gesamtLaufzeit": 459,
                    "rate": 501.20,
                    "tilgung": 2.000,
                    "typ": "ANNUITAETEN_DARLEHEN",
                    "zinsBindung": 15,
                    "zinsZahlungsBeginnAm": "2021-10-30"
                }
            ],
            "darlehensSumme": 179000.00,
            "sollZins": 1.360,
            "effektivZins": 1.390,
            "machbarkeit": 11,
            "rank": 11
        },
        {
            "annahmeFrist": "2021-09-03",
            "darlehen": [
                {
                    "auszahlungsBetrag": 78930.00,
                    "auszahlungsDatum": "2021-09-29",
                    "darlehensBetrag": 78930.00,
                    "sollZins": 1.000,
                    "effektivZins": 1.020,
                    "produktAnbieter": "COMMERZBANK",
                    "gesamtKosten": 95335.41,
                    "gesamtLaufzeit": 468,
                    "rate": 203.90,
                    "tilgung": 2.100,
                    "typ": "ANNUITAETEN_DARLEHEN",
                    "zinsBindung": 15,
                    "zinsZahlungsBeginnAm": "2021-10-30"
                },
                null
            ],
            "darlehensSumme": 178930.00,
            "sollZins": 0.989,
            "effektivZins": 1.010,
            "kennung": "Regional Green; inkl. KfW-Darlehen 124",
            "machbarkeit": 11,
            "rank": 11
        },
        {
            "annahmeFrist": "2021-09-07",
            "darlehen": [
                {
                    "auszahlungsBetrag": 178925.00,
                    "auszahlungsDatum": "2021-09-29",
                    "darlehensBetrag": 178925.00,
                    "sollZins": 1.500,
                    "effektivZins": 1.530,
                    "produktAnbieter": "ALLIANZ",
                    "gesamtKosten": 233781.76,
                    "gesamtLaufzeit": 448,
                    "rate": 521.86,
                    "tilgung": 2.000,
                    "typ": "ANNUITAETEN_DARLEHEN",
                    "zinsBindung": 15,
                    "zinsZahlungsBeginnAm": "2021-10-30"
                }
            ],
            "darlehensSumme": 178925.00,
            "sollZins": 1.500,
            "effektivZins": 1.530,
            "machbarkeit": 11,
            "rank": 11
        },
        {
            "annahmeFrist": "2021-09-06",
            "darlehen": [
                {
                    "auszahlungsBetrag": 178925.00,
                    "darlehensBetrag": 178925.00,
                    "sollZins": 1.200,
                    "effektivZins": 1.230,
                    "produktAnbieter": "DSL_BANK",
                    "gesamtLaufzeit": 480,
                    "rate": 469.83,
                    "tilgung": 1.951,
                    "typ": "ANNUITAETEN_DARLEHEN",
                    "zinsBindung": 15
                }
            ],
            "darlehensSumme": 178925.00,
            "sollZins": 1.200,
            "effektivZins": 1.230,
            "machbarkeit": 11,
            "rank": 11
        },
        {
            "annahmeFrist": "2021-09-06",
            "darlehen": [
                {
                    "auszahlungsBetrag": 78925.00,
                    "darlehensBetrag": 78925.00,
                    "sollZins": 1.200,
                    "effektivZins": 1.230,
                    "produktAnbieter": "DSL_BANK",
                    "gesamtLaufzeit": 480,
                    "rate": 207.24,
                    "tilgung": 1.951,
                    "typ": "ANNUITAETEN_DARLEHEN",
                    "zinsBindung": 15
                },
                null
            ],
            "darlehensSumme": 178925.00,
            "sollZins": 1.080,
            "effektivZins": 1.110,
            "kennung": "inkl. KfW-Darlehen 124 über 100.000,00 €",
            "machbarkeit": 11,
            "rank": 11
        }
    ]
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

