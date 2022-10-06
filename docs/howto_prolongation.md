# Vorschlaege-API for loan prolongation

As consumer, I can reate a financial proposal for an existing mortgage loan to prolongate them.

![Status](https://img.shields.io/badge/Status-preAlpha_Pilot-darkred)

![consumer](https://img.shields.io/badge/-consumer-lightblue)
![advisor](https://img.shields.io/badge/-advisor-lightblue)
![mortgageLoan](https://img.shields.io/badge/-mortgageLoan-lightblue)

[![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://github.com/europace/authorization-api)
[![GitHub release](https://img.shields.io/github/v/release/europace/baufi-passende-vorschlaege-api)](https://github.com/europace/baufi-passende-vorschlaege-api/releases)
[![Pattern](https://img.shields.io/badge/Pattern-Tolerant%20Reader-yellowgreen)](https://martinfowler.com/bliki/TolerantReader.html)

## Documentation
[![YAML](https://img.shields.io/badge/OAS-HTML_Doc-lightblue)](https://europace.github.io/baufi-passende-vorschlaege-api/docs/swaggerui.html)
[![YAML](https://img.shields.io/badge/OAS-YAML-lightgrey)](https://github.com/europace/baufi-passende-vorschlaege-api/blob/main/api/baufi-passende-vorschlaege-api.yaml)

## Usecases

- create a financial proposal for an existing mortgage loan
- bookmark the proposal for later review
- accept the offer as consumer directly

## Requirements
- authenticated as advisor

## Quick Start
To test our APIs and your use cases as quickly as possible, we have created a [Postman Collection](https://github.com/europace/baufi-passende-vorschlaege-api/tree/main/docs) for you.

### Authentication
Please use [![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/common/authentifizierung/authorization-api/) to get access to the APIs. The OAuth2 client requires the following scopes:

| Scope                                | API Usecase                                 |
| -------------------------------------- | --------------------------------------------- |
| `baufinanzierung:angebote:ermitteln` | to determine financial proposes |
| `baufinanzierung:vorgaenge:schreiben` | to create case and bookmark and/or accept offer |

## Find loan prologation offer

As consumer, I want to get a loan prologation offer and/or debt restructuring offers to find a financial solution for my mortgage.

The experience of determining appropriate financing proposals is one of the deciding factors for the success of the lead. In order for the user to get a quick feedback, the determination is offered asynchronously.

![find](https://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/europace/baufi-passende-vorschlaege-api/tree/main/docs/find-fin-props-seq.puml&fmt=svg)

### request loan prologation offer

To find the right financial proposals for prologation, the data of the old loan(s) (`darlehensliste`) is mandatory .

For prolongation offers no creditworthiness information on consumers is required, only the zip code is needed at the property in order to be able to make a regional restriction.

If you want to create additionally debt restructuring offers (Umschuldungsangebote), creditworthiness information on consumers and property informations are neseccary. 

To adjust the financial solution, you can define preferences in `praeferenzen`.

example request:
``` http
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

example response:
``` json
{
    "anfrageId": "passende-vorschlaege-71e2faa9-4094-4f66-8909-02677e5e9d7f"
}
```

### query loan prologation offer

The `anfrageId` can be used to retrieve the appropriate financing proposals.

example request:
``` http
GET /v1/vorschlaege/{anfrageId} HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]
```

example response:

If finding appropriate financing proposals is already in progress, please try again until code 200.

``` http
202 - Accepted
```

If determination is finished, the financing proposals will be delivered:
``` http
200 - OK
```
``` json
{
    "vorschlaege": [
        {
            "finanzierungsVorschlagId": "20bbd098eeb050dd44e28b6c088e3f1f",
            "annahmeFrist": "2022-10-06",
            "finanzierungsbausteine": [
                {
                    "@type": "ANNUITAETENDARLEHEN",
                    "restschuldNachZinsbindungsEnde": 98983.13,
                    "schlussrate": 558.94,
                    "datumLetzteRate": "2054-07-31",
                    "anzahlRaten": 392,
                    "tilgungssatzInProzent": 1.5,
                    "darlehensbetrag": 189000.0,
                    "annuitaetendetails": {
                        "zinsbindungInJahren": 20,
                        "tilgung": {
                            "@type": "RATE",
                            "rate": 911.93,
                            "tilgungsbeginn": "2023-02-27"
                        },
                        "sondertilgungJaehrlich": 5.0
                    },
                    "bereitstellungszinsfreieZeitInMonaten": 6,
                    "sollZins": 4.29,
                    "effektivZins": 4.4,
                    "rateMonatlich": 911.93,
                    "produktAnbieter": "Musterbank"
                }
            ],
            "darlehensSumme": 189000.00,
            "sollZins": 4.290,
            "effektivZins": 4.400,
            "kennung": "Prolongation",
            "machbarkeit": 0,
            "rank": 1,
            "gesamtRateProMonat": 911.93,
            "zinsbindungInJahrenMinMax": "20"
        }
    ],
    "leadRating": {
        "successRating": "D",
        "effortRating": {
            "rating": false,
            "explanations": []
        },
        "feasibilityRating": 0
    }
}
```

## Bookmark loan prologation offer

As advisor or consumer, I can bookmark financial proposals for later review. Therefor, you will find instructions in the [Vorschlaege-API](https://docs.api.europace.de/baufinanzierung/showcase_conditions/vorschlaege-api/).

## Accept loan prologation offer

As consumer, I can accept the loan prologation offer to get the contract from the loan provider.

example request:
```http
POST /vorschlag/accept HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer [access_token]

    {
      "anfrageId": "passende-vorschlaege-c5486371-3d1e-43e2-8fc4-db920bde4fef",
      "finanzierungsVorschlagId": "d550a975da78f73d9e3256352ce0f366",
      "vorgangId": "ABC123"
    }
```

example response:
```http
200 - OK
```

```json
{
  "message": "Vorschlag akzeptiert."
}
```

## Support

If you have any questions or problems, please contact helpdesk@europace2.de.

## Terms of use

The APIs are provided under the following [Terms of Use](https://docs.api.europace.de/terms/).