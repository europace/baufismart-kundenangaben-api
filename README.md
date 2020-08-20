# Importieren von Kundenangaben in einen neuen Vorgang

Version: 1.0-alpha1

> :warning: Diese Schnittstelle wird kontinuierlich weiterentwickelt. Daher erwarten wir
> von allen Nutzern dieser Schnittstelle, dass sie das "[Tolerant Reader Pattern](https://martinfowler.com/bliki/TolerantReader.html)" nutzen, d.h.
> tolerant gegenüber kompatiblen API-Änderungen beim Lesen und Prozessieren der Daten sind:
>
> 1. unbekannte Felder dürfen keine Fehler verursachen
>
> 2. Strings mit eingeschränktem Wertebereich (Enums) müssen mit neuen, unbekannten Werten umgehen können
>
> 3. sinnvoller Umgang mit HTTP-Statuscodes, die nicht explizit dokumentiert sind
>

# Inhaltsverzeichnis

* [Allgemeines](#allgemeines)
* [Authentifizierung](#authentifizierung)
* [TraceId zur Nachverfolgbarkeit von Requests](#traceid-zur-nachverfolgbarkeit-von-requests)
* [Content-Type](#content-type)
* [Fehlercodes](#fehlercodes)
   * [HTTP-Status Errors](#http-status-errors)
   * [weitere Fehler](#weitere-fehler)
* [API-Spezifikation](#api-spezifikation)
* [API-Referenz](#api-referenz)
* [Beispiel](#beispiel)
* [FAQs](#faqs)
* [Tools](#tools)
* [Kontakt](#kontakt)
* [Nutzungsbedingungen](#nutzungsbedingungen)

# Dokumentation

## Allgemeines

Die Schnittstelle ermöglicht es, Kundenangaben[^1] in einen BaufiSmart Vorgang zu importieren. Der Vorgang wird dabei im Rahmen
des Imports automatisch erzeugt. Informationen über den erzeugten Vorgang, wie beispielsweise die Vorgangsnummer sind der
Response zu entnehmen.

Für einen Schnelleinstieg, siehe [Tools](#tools)

[^1]: Kundenangaben sind in anderen APIs von BaufiSmart auch als _Erfasste Daten_ bekannt

<a name="OAuth2"></a>
## Authentifizierung

Für jeden Request ist eine Authentifizierung erforderlich. Die Authentifizierung erfolgt über den OAuth 2.0 Client-Credentials Flow.

| Request Header Name | Beschreibung           |
|---------------------|------------------------|
| Authorization       | OAuth 2.0 Bearer Token |


Das Bearer Token kann über die [Authorization-API](https://github.com/europace/authorization-api) angefordert werden.
Dazu wird ein Client benötigt, der vorher von einer berechtigten Person über das Partnermanagement angelegt wurde,
eine Anleitung dafür befindet sich im [Help Center](https://europace2.zendesk.com/hc/de/articles/360012514780).

Damit der Client für diese API genutzt werden kann, müssen im Partnermanagement die folgenden Berechtigungen aktiviert werden

| Name                                     | Beschreibung                        |
| ---------------------------------------- | ----------------------------------- |
| **baufinanzierung:vorgang:schreiben**    | Baufinanzierungsvorgänge schreiben  |

Schlägt die Authentifizierung fehl, erhält der Aufrufer eine HTTP Response mit Statuscode **401 UNAUTHORIZED**.

Hat der Client nicht die benötigte Berechtigung, um die Resource abzurufen, erhält der Aufrufer eine HTTP Response mit Statuscode **403 FORBIDDEN**.

## TraceId zur Nachverfolgbarkeit von Requests

Für jeden Request soll eine eindeutige ID generiert werden, die den Request im EUROPACE 2 System nachverfolgbar macht und so bei
etwaigen Problemen oder Fehlern die systemübergreifende Analyse erleichtert.
Die Übermittlung der X-TraceId erfolgt über einen HTTP-Header. Dieser Header ist optional,
wenn er nicht gesetzt ist, wird eine ID vom System generiert.

| Request Header Name | Beschreibung                    | Beispiel    |
|---------------------|---------------------------------|-------------|
| X-TraceId           | eindeutige Id für jeden Request | sys12345678 |

## Content-Type

Die Schnittstelle akzeptiert Daten mit Content-Type `application/json`.
Entsprechend muss im Request der Content-Type Header gesetzt werden. Zusätzlich das Encoding, wenn es nicht UTF-8 ist.

Zusätzlich kann die Version der Schnittstelle im Rahmen der _Content Negotiation_ angegeben werden. Fehlt die Angabe wird
die neuste Version der API verwendet.

| Request Header Name |   Header Value                    |
|---------------------|-----------------------------------|
| Content-Type        | application/json;version=@apiVersion@ |

## Strategie für das _Tolerant Reader Pattern_

Einige Felder der Kundenangaben verfügen über einen eingeschränkten Wertebereich.
Beispielsweise sind im Typ `Bauspardarlehen.abschlussgebuehrmodus` nur die Werte
`SOFORTZAHLUNG` und `VERRECHNUNG` erlaubt.
Andere Werte werden von der Schnittstelle ignoriert und so verarbeitet, als wäre
das Feld leer.

## Fehlercodes

### HTTP-Status Errors

| Fehlercode | Nachricht       | weitere Attribute          | Erklärung                            |
|------------|-----------------|----------------------------|--------------------------------------|
| 401        | Unauthorized    | -                          | Authentifizierung ist fehlgeschlagen |
| 400        | Bad Request     | -                          | Der Request hat ein fehlerhaftes Format oder der Datentyp eines Felds weicht von der Spezifikation ab. |

### Weitere Fehler

| Fehlercode | Nachricht                  | Erklärung                                                                                       |
|------------|----------------------------|-------------------------------------------------------------------------------------------------|
| 403        | Insufficient access rights | Es wird versucht auf eine Ressource zuzugreifen, für die keine ausreichende Berechtigung besteht |

## API Spezifikation

Die OpenAPI-Spezifikation ist in der Datei [api-docs.yaml](./api-docs.yaml)
zu finden. Alternativ steht eine JSON-Version [api-docs.json](./api-docs.json) zur Verfügung.

## API Referenz
Die API-Referenz ist der [OpenAPI](./api-docs.yaml) Spezifikation zu entnehmen.


## Beispiel

Folgt

## FAQs

Folgt

## Tools

Für [Postman](https://www.getpostman.com/) stellen wir im [Schnellstarter-Projekt](https://github.com/europace/api-schnellstart/)
auch eine Collection mit einem Beispiel für die Kundenangaben-API zur Verfügung.

## Kontakt
Kontakt für Support: [devsupport@europace2.de](mailto:devsupport@europace2.de)

## Nutzungsbedingungen
Die APIs werden unter folgenden [Nutzungsbedingungen](https://developer.europace.de/terms/) zur Verfügung gestellt.
