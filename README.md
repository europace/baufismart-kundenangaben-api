# Kundenangaben API
As advisor with Kundenangaben API you can create new cases or update cases with customer data from your crm system or lead applications for a seamless and fast start of advising.

Kundenangaben are also named as `Erfasste Daten` in Mortgage APIs (Vorgaenge-API or Antraege-API) .

![advisor](https://img.shields.io/badge/-advisor-lightblue)
![mortgageLoan](https://img.shields.io/badge/-mortgageLoan-lightblue)

[![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/common/authentifizierung/authorization-api/)

[![GitHub release](https://img.shields.io/github/v/release/europace/baufismart-kundenangaben-api)](https://github.com/europace/baufismart-kundenangaben-api/releases)

[![Pattern](https://img.shields.io/badge/Pattern-Tolerant%20Reader-yellowgreen)](https://martinfowler.com/bliki/TolerantReader.html)

## Documentation
[![YAML](https://img.shields.io/badge/OAS-HTML_Doc-lightblue)](https://refined-github-html-preview.kidonng.workers.dev/europace/baufismart-kundenangaben-api/raw/master/reference/index.html)
[![YAML](https://img.shields.io/badge/OAS-YAML-lightgrey)](https://github.com/europace/baufismart-kundenangaben-api/blob/master/kundenangaben-openapi.yaml)
[![JSON](https://img.shields.io/badge/OAS-JSON-lightgrey)](https://github.com/europace/baufismart-kundenangaben-api/blob/master/kundenangaben-openapi.json)

Feedback und Fragen zum Modell sind als [GitHub Issue](https://github.com/europace/baufismart-kundenangaben-api/issues/new) willkommen.

## Usecases
- create case with customer data from CRM system or lead applications
- get case with customer data 
    - to update CRM system or lead applications or
    - to create own financing proposals with your structure, story and design
- overwrite existing case with customer data

## Quick Start
To test our APIs and your use cases as quickly as possible, we have created a [Postman Collection](https://github.com/europace/api-quickstart) for you.

In the Postman collection in the folder "BaufiSmart Kundenangaben-API" you will find two examples. Since the data model in the Kundengaben API is very extensive, we provide a way to test/validate requests without storing data in Europace with the „validate customer data" request. This endpoint is for faster connectivity, but is not required for functionality.
### Authentication
Please use [![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/common/authentifizierung/authorization-api/) to get access to the APIs. The OAuth2 client requires the following scopes:

| Scope                               | API Use case                    |
|-------------------------------------|---------------------------------|
| `baufinanzierung:echtgeschaeft`     | to use api in production mode   |
| `baufinanzierung:vorgang:schreiben` | create cases for mortgage loans |

## Create case
As Advisor you can create a case with your customers data to start seamless and fast advising.

> **IMPORTANT: Provide privacy statement** \
> Before you transfer consumer data to Europace please note our [terms of use](https://docs.api.europace.de/terms/) and [provide the privacy statement of the advisor](https://docs.api.europace.de/common/privacystatement/) (MUST).

Requirements:
* OAuth Token has the scope `baufinanzierung:vorgang:schreiben`
* caller is advisor of the case

A customer wants to buy a single-family home for himself in Berlin. He has already saved up equity and has already given details of some preferences for financing. The rate should be in the amount of his current rent. He became aware of the financing intermediary through his employer Trisalis AG, which has a cooperation agreement with him and receives € 50 lead fee for each contract concluded as compensation for internal marketing.

example-request:
``` http
POST /kundenangaben HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer eyJraWQiOiJZUUZ...
```
``` json
{
    "importMetadaten": {
        "datenkontext": "ECHT_GESCHAEFT",
        "externeVorgangsId": "Lead 7c78de13-da02-4bdd-b4b2-c37855abfccc",
        "importquelle": "Landinpage Trisalis AG 2021",
        "leadtracking": {
            "kampagne": "Trisalis AG",
            "keyword": "Altersvorsorge - stark wie Beton",
            "trackingId": "be8d40b0-25af-48ae-8c57-a1b72527f903"
        },
        "zusaetzlicherEreignistext": "Premium-Kunde",
        "prioritaet": "HOCH",
        "betreuung": {
            "kundenbetreuer": "ABC12",
            "bearbeiter": "ABC12"
        },
        "tippgeber": {
            "tippgeberPartnerId": "{{PARTNER_ID}}", <-- PartnerID der Trisalis AG
            "tippgeberprovisionswunsch": {
                "@type": "BETRAG",
                "betrag": 50
            }
        }
    },
    "kundenangaben": {
        "haushalte": [
            {
                "kunden": [
                    {
                        "externeKundenId": "extKunde1",
                        "referenzId": "jsonRef1",
                        "personendaten": {
                            "person": {
                                "titel": {
                                    "prof": false,
                                    "dr": false
                                },
                                "anrede": "HERR",
                                "vorname": "Max",
                                "nachname": "Mustermann"
                            }
                        },
                        "wohnsituation": {
                            "anschrift": {
                                "strasse": "Teststr.",
                                "hausnummer": "55",
                                "plz": "10179",
                                "ort": "Berlin"
                            }
                        },
                       "finanzielles": {
                            "beschaeftigung": {
                                "@type": "ANGESTELLTER",
                                "beschaeftigungsverhaeltnis": {
                                    "arbeitgeber": {
                                        "name": "Trisalis AG",
                                        "inDeutschland": true
                                    }
                                }
                            },
                            "einkommenNetto": 4825
                        },
                        "kontakt": {
                            "telefonnummer": {
                                "vorwahl": "030",
                                "nummer": "420860"
                            },
                            "email": "max.mustermann@europace.de",
                            "weitereKontaktmoeglichkeiten": "beste Erreichbarkeit von 18-20Uhr"
                        }
                    }
                ],
                "finanzielleSituation": {
                    "vermoegen": {
                        "summeBankUndSparguthaben": {
                            "guthaben": 154000,
                            "verwendung": {
                                "@type": "AUFLOESUNG_ALS_VERWENDUNG",
                                "maximalEinzusetzenderBetrag": 78000,
                                "kommentar": "einzusetzendes Eigenkapital"
                            }
                        }
                    },
                    "ausgaben": {
                        "summeMietausgaben": {
                            "betragMonatlich": 1150,
                            "entfaelltMitFinanzierung": true
                        }
                    }
                }
            }
        ],
        "finanzierungsobjekt": {
            "immobilie": {
                "adresse": {
                    "strasse": "Immobilienstraße",
                    "hausnummer": "16",
                    "plz": "82784",
                    "ort": "Spandau"
                },
                "typ": {
                    "@type": "EINFAMILIENHAUS",
                    "gebaeude": {
                        "baujahr": 2018,
                        "nutzung": {
                            "wohnen": {
                                "gesamtflaeche": 150,
                                "nutzungsart": {
                                    "@type": "EIGENGENUTZT"
                                }
                            }
                        }
                    }
                }
            }
        },
        "finanzierungsbedarf": {
            "finanzierungszweck": {
                "@type": "KAUF",
                "kaufpreis": 358000,
                "nebenkosten": {
                    "maklergebuehr": {
                        "wert": 3.75,
                        "einheit": "PROZENT"
                    },
                    "notargebuehr": {
                        "wert": 1.25,
                        "einheit": "PROZENT"
                    },
                    "grunderwerbsteuer": {
                        "wert": 6,
                        "einheit": "PROZENT"
                    }
                }
            },
            "praeferenzen": {
                "finanzierungsdetailspraeferenzen": {
                    "zinsbindung": {
                        "@type": "ZINSBINDUNGSSPANNE",
                        "vonJahre": 10,
                        "bisJahre": 15,
                        "kommentar": "Einstellung in Leadstrecke: normal"
                    },
                    "ratenhoehe": {
                        "@type": "WARMMIETE_VON_HEUTE"
                    },
                    "tilgungssatzwechsel": {
                        "@type": "NICHT_EINGEPLANT_ODER_ABSEHBAR"
                    }
                },
                "bestandteileDerFinanzierung": {
                    "praeferenz": "UNKOMPLIZIERTE_FINANZIERUNG"
                },
                "absicherungUndVorsorge": {
                    "praeferenz": "KEINE_PRAEFERENZ"
                },
                "zeitlicherRahmen": {
                    "@type": "SO_SCHNELL_WIE_MOEGLICH",
                    "kommentar": "Es gibt noch weitere Interessenten"
                }
            },
            "finanzierungsbausteine": [
                {
                    "@type": "ANNUITAETENDARLEHEN",
                    "darlehensbetrag": 319380,
                    "annuitaetendetails": {
                        "zinsbindungInJahren": 10,
                        "tilgungswunsch": {
                            "@type": "RATE",
                            "rate": 1150,
                            "tilgungsbeginn": "2020-09-20"
                        },
                        "sondertilgungJaehrlich": 0,
                        "auszahlungszeitpunkt": "2021-07-19"
                    },
                    "bereitstellungszinsfreieZeitInMonaten": 2,
                    "provision": 1.5
                }
            ]
        }
    }
}
```

example-response:
``` json
201 - created
{
    "vorgangsnummer": "YX4MDU"
}
```

## Get case 
As advisor I can read out the data of the case, to create an individual financial proposal for a convincing sales story.

Requirements:
* authenticated as advisor, editor or sales organisation with access to the case
* OAuth token has the scope `baufinanzierung:vorgang:lesen`

In the example you'll get customer data of case YX4MDU.

example-request:
``` http
GET /kundenangaben/YX4MDU HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer eyJraWQiOiJZUUZYT...
```

example-response:
``` json
{       
    "haushalte": [
        {
            "kunden": [
                {
                    "externeKundenId": "extKunde1",
                    "referenzId": "jsonRef1",
                    "personendaten": {
                        "person": {
                            "titel": {
                                "prof": false,
                                "dr": false
                            },
                            "anrede": "HERR",
                            "vorname": "Max",
                            "nachname": "Mustermann"
                        }
                    },
                    "wohnsituation": {
                        "anschrift": {
                            "strasse": "Teststr.",
                            "hausnummer": "55",
                            "plz": "10179",
                            "ort": "Berlin"
                        }
                    },
                    ...
                    ...
                    ...
                }
            ]
        }
    ],
    "finanzierungsobjekt": {
        ...
    },
    "finanzierungsbedarf": {
        ...
    },
    "bankverbindung": {
        ...
    }
}
```

## Replace case
As Advisor the input data of the case has to be replaced, to use the current customer data from a crm system or digital self-disclosure.
> Attention: The data will be completely replaced by the specified values. If data fields are not transferred, the data in the case will be replaced by ’null' and thus deleted.

Requirement:
* OAuth token has the scope `baufinanzierung:vorgang:schreiben`
* caller is editor of the case

In the example the entered customer data of case A65JS6 will be replaced.

example-request:
``` http
PUT /kundenangaben/A65JS6 HTTP/1.1
Host: baufinanzierung.api.europace.de
Content-Type: application/json
Authorization: Bearer eyJraWQiOiJZUUZYT...
```
```json
{
  "updateMetadaten": {
    "tippgeber": {
      "tippgeberPartnerId": "AAV43",
      "tippgeberprovisionswunsch": {
        "@type": "BETRAG",
        "betrag": 567.1
      }
    }
  },
  "kundenangaben": {
    "haushalte": [
      {
        "kunden": [ ...
```
[Body as in create case (POST)]


example-response:
``` http
204 - no content
```

## FAQ
### Where is the case created?

The owner of a case is always the advsior. His settings are applied to the case and teh advisor  usually also receives the sales commission. The editor of the case may differ, for example, if the completion of the customer data is done by a team assistant or a clearing takes place.

If the advisor is not specified under ‚betreuung', the subject of the API client is entered as advisor in the generated case. If no editor is specified, the user will be asked if he/she wants to take over the editing during the first editing.

If the case is to be created for a different advisor than the API user, then we recommend setting the roles of the case in the object ‚betreuung'.

``` json
"importMetadaten": {
        "betreuung": {
            "kundenbetreuer": "{{PARTNER_ID}}",
            "bearbeiter": "{{PARTNER_ID}}"
        },
        ...
```

With "impersonation" it is also possible to create the case in the name of another user. How to do this exactly is described in the [Autorization-API](https://docs.api.europace.de/baufinanzierung/authentifizierung/#wie-authentifiziere-ich-verschiedene-benutzer-mit-einem-client-impersionieren) erklärt.

### How do I find my customer again?

External references can be specified for both the case and the customers, which can be read out again via the [Vorgaenge-API](https://docs.api.europace.de/baufinanzierung/vorgaenge/vorgang-auslesen-api/) and [Antraege-API](https://docs.api.europace.de/baufinanzierung/antraege/antraege-api/). This makes it possible to recognize data records in CRM or banking systems when they are read out.

The following references are possible:
* case: `externeVorgangsId`
* customer: `externeKundenId`

### How can I open the created case in the browser?

Open the created case via: `https://www.europace2.de/vorgang/oeffne/XXXXXX` \
The Kundenangaben-API  returns a case number. XXXXXX stands for the case number.

### What happens to incorrect data fields?

We offer the most error-tolerant API possible. Errors would lead to interruptions in lead processes and thus possibly to frustrated customers. In case of doubt, it is more important not to have individual data fields than no data at all.

To increase the error tolerance of the API, the API applies the Tolerant Reader Pattern. That is, fields or enum values that are unknown to the API are ignored. For example, in the type `Bauspardarlehen.abschlussgebuehrmodus` only the values `SOFORTZAHLUNG` and `VERRECHNUNG ` are allowed. Other values are ignored by the API and processed as if the field was empty.

## Terms of use
The APIs are provided under the following [Terms of Use](https://docs.api.europace.de/nutzungsbedingungen).

## Support
If you have any questions or problems, you can contact devsupport@europace2.de.
