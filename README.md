# Neuen Vorgang mit Kundenangaben anlegen
Die API erzeugt einen neuen Vorgang mit Hilfe von Kundenangaben. Als Ergebnis wird eine Referenz des neuen Vorgangs geliefert. 

Die neue Kundenangaben-API ersetzt die alte BEX-API, die hier dokumentiert https://github.com/europace/baufismart-vorgang-anlegen-api ist.
Kundenangaben sind in anderen APIs von BaufiSmart (Vorgaenge-API oder Antraege-API) auch als _Erfasste Daten_ bekannt.


![Vertrieb](https://img.shields.io/badge/-Vertrieb-lightblue)
![Baufinanzierung](https://img.shields.io/badge/-Baufinanzierung-lightblue)

[![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/baufinanzierung/authentifizierung/)
[![GitHub release](https://img.shields.io/github/v/release/europace/baufismart-kundenangaben-api)](https://github.com/europace/baufismart-kundenangaben-api/releases)

[![Pattern](https://img.shields.io/badge/Pattern-Tolerant%20Reader-yellowgreen)](https://martinfowler.com/bliki/TolerantReader.html)

## Dokumentation
[![YAML](https://img.shields.io/badge/OAS-HTML_Doc-lightblue)](https://refined-github-html-preview.kidonng.workers.dev/europace/baufismart-kundenangaben-api/raw/master/reference/index.html)
[![YAML](https://img.shields.io/badge/OAS-YAML-lightgrey)](https://github.com/europace/baufismart-kundenangaben-api/blob/master/kundenangaben-openapi.yaml)
[![JSON](https://img.shields.io/badge/OAS-JSON-lightgrey)](https://github.com/europace/baufismart-kundenangaben-api/blob/master/kundenangaben-openapi.json)

Feedback und Fragen zum Modell sind als [GitHub Issue](https://github.com/europace/baufismart-kundenangaben-api/issues/new) willkommen.

## Anwendungsfälle der API
- Vorgang mit Kundenangaben aus Leadantragstrecken oder CRM-System anlegen

## Schnellstart
Damit du unsere APIs und deinen Anwendungsfall schnellstmöglich testen kannst, haben wir eine [Postman-Collection](https://docs.api.europace.de/baufinanzierung/schnellstart/) für dich zusammengestellt. 

In der Postman-Collection im Ordner "BaufiSmart Kundenangaben-API" findest du zwei Beispiele. Da das Datenmodells in der Kundenangaben-API sehr umfangreich ist, stellen wir mit dem Request "Kundenangaben validieren" eine Möglichkeit zur Verfügung, mit der Anfragen getestet/validiert werden können, ohne dass Daten gespeichert werden. Dieser Endpunkt dient der schnelleren Anbindung, ist aber für die Funktionsweise nicht erforderlich.

### Authentifizierung
Bitte benutze [![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/baufinanzierung/authentifizierung/), um Zugang zur API bekommen. Um die API verwenden zu können, benötigt der OAuth2-Client folgende Scopes:

| Scope                                  | API-Usecase                                                      |
| -------------------------------------- | ---------------------------------------------------------------- |
| `baufinanzierung:vorgang:schreiben`    | Baufinanzierungsvorgänge anlegen                                 |
| `baufinanzierung:echtgeschaeft`        | Vorgänge in Produktion anlegen, ansonsten nur Testmodus möglich  |

## Beispiel

Ein Kunde möchte ein Einfamilienhauses für sich selbst in Berlin kaufen. Er hat bereits Eigenkapital angepart und schon Angaben zu einigen Präferenzen bei der Finanzierung gemacht. Die Rate soll in Höhe seiner jetzigen Miete sein. Er wurde durch seinen Arbeitgeber die Trisalis AG auf den Finanzierungsvermittler aufmerksam, der eine Kooperationsvereinbarung mit ihm hat und für jeden abgeschlossenen Vertrag 50€ Leadgebühr als Aufwandentschädigung für die interne Vermarktung erhält.

Beispiel-Request: 
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

Beispiel-Response:
``` json
{
    "vorgangsnummer": "YX4MDU"
}
```

## Wo wird der Vorgang angelegt?

Besitzer eines Vorgangs ist immer der Kundenbetreuer. Die für ihn geltenden Einstellungen werden auf den Vorgang angewendet und er erhält i.d.R. auch die Vertriebsprovision. Der Bearbeiter des Vorgangs kann abweichen, wenn zum Beispiel die Vervollständigung der Antragsdaten durch eine Teamassistenz erfolgt oder ein Clearing stattfindet.

Ist der Kundenbetreuer nicht unter `betreuung` angegeben, wird im erzeugten Vorgang der Benutzer des API-Clients als Kundenbetreuer eingetragen. Ist kein Bearbeiter angegeben, wird bei der ersten Bearbeitung der Benutzer gefragt, ob er die Bearbeitung übernehmen möchte. 

Soll der Vorgang bei einem anderen Kundenbetreuer angelegt werden als dem API-Benutzer, dann empfehlen wir die Rollen des Vorgangs im Object `betreuung` einzustellen.

``` json
"importMetadaten": {
        "betreuung": {
            "kundenbetreuer": "{{PARTNER_ID}}",
            "bearbeiter": "{{PARTNER_ID}}"
        },
        ...
```

Mit Hilfe der sog. "Impersonierung" ist es auch möglich, den Vorgang im Namen eines anderen Benutzers anzulegen. Wie das genau geht, wird in der [Autorization-API](https://docs.api.europace.de/baufinanzierung/authentifizierung/#wie-authentifiziere-ich-verschiedene-benutzer-mit-einem-client-impersionieren) erklärt.

## Wie finde ich meinen Kunden wieder?

Sowohl für den Vorgang als auch die Kunden lassen sich externe Referenzen mitgeben, die über die [Vorgaenge-API](https://docs.api.europace.de/baufinanzierung/vorgaenge/vorgang-auslesen-api/) und [Antraege-API](https://docs.api.europace.de/baufinanzierung/antraege/antraege-api/) wieder ausgelesen werden können. Damit wird ermöglicht, dass Datensätze in CRM- oder Banksystemen beim Auslesen wiedererkannt werden können. 

Folgende Referenzen sind möglich:
* Vorgang: `externeVorgangsId`
* Kunden: `externeKundenId`

## Was passiert mit falschen Datenfeldern? 

Wir haben versucht, eine möglichst fehlertolerante API zu bauen. Im Zweifel ist es wichtiger einzelne Datenfelder nicht zu haben als gar keine Daten. Fehler würden zu Unterbrechungen in Leadantragstrecken führen und so ggf. zu  frustrierten Kunden.
Um die Fehlertoleranz der API zu erhöhen, wendet die API das Tolerant Reader Pattern an. Das heisst, Felder oder Enum-Werte, die der API unbekannt sind, werden ignoriert. Beispielsweise sind im Typ `Bauspardarlehen.abschlussgebuehrmodus` nur die Werte `SOFORTZAHLUNG` und `VERRECHNUNG` erlaubt. Andere Werte werden von der API ignoriert und so verarbeitet, als wäre das Feld leer. 



## FAQs

Q: Wird die alte BEX-Schnittstelle durch die neue Kundenangaben-API ersetzt werden? 

A: Ja, die neue Kundenangaben-API wird die alte BEX-Schnittstelle ersetzen.

---

Q: Wird die alte BEX-Schnittstelle noch weiterentwickelt?

A: Nein, die alte BEX-Schnittstelle wird nicht mehr weiterentwickelt und Mitte 2021 abgeschaltet.

---

Q: Wie kommt es, das die Kundenangaben-API eine neue Struktur hat?

A: Die Kundenangaben-API ermöglicht zum ersten Mal den Übertrag fast aller in BaufiSmart erfassbaren Daten. Die passende Struktur dazu soll übersichtlicher und besser verständlich sein. Wir haben die Struktur in mehreren Feedbackrunden geschärft. 
 
Die alte Struktur der BEX-Schnittstelle und die Struktur der Vorgänge-API weichen historisch bedingt voneinander ab. Das Datenmodell der Kundenangaben-API setzt basierend auf Feedback den neuen Standard. Wir planen zukünftige APIs in Absprache mit unseren Partnern auf die Struktur der Kundenangaben-API aufzubauen.

---

Q: Wann ist es sinnvoll mit der Anbindung der Kundenangaben-API zu beginnen?

A: Mit Release 1.0.0 ist die API produktiv einsetzbar. Lediglich die individuellen Zusatzangaben der Produktanbieter sind als Schema definiert, werden aber noch nicht gespeichert. Dieser Schritt wird in den nächsten Monaten folgen.

## Kontakt
Kontakt für Support: [devsupport@europace2.de](mailto:devsupport@europace2.de)

## Nutzungsbedingungen
Die APIs werden unter folgenden [Nutzungsbedingungen](https://docs.api.europace.de/nutzungsbedingungen/) zur Verfügung gestellt.
