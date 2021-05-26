# baufi-passende-vorschlaege-api

API zur Ermittlung passender Finanzierungsvorschläge anhand einer Verbraucher-Situation und -Präferenzen.

Die API ist in einem pre-alpha-Status und noch nicht funktional verfügbar.

Die Definition der RESTful-Schnittstelle erfolgt über eine [.yaml-Datei](baufi-passende-vorschlaege-api.yaml) im [Open API Version 3](https://swagger.io/specification/) -Format.

## Client generieren

Ein Client kann mit Hilfe der [.yaml-Datei](baufi-passende-vorschlaege-api.yaml) über entsprechende Libraries generiert werden. z.B. :

- [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator)
- [Swagger Codegen](https://github.com/swagger-api/swagger-codegen)
- Plugins, wie dem [OpenAPI Generator für Gradle](https://github.com/OpenAPITools/openapi-generator/tree/master/modules/openapi-generator-gradle-plugin)

In der [build.gradle](build.gradle) ist exemplarisch der Task `openApiGenerate` für JavaScript konfiguriert.

Nach Ausführung via `./gradlew openApiGenerate` finden sich im Ordner `/build/generated/src/` die generierten Modelle und Client.

## Fragen und Anregungen

Bei Fragen und Anregungen kannst du entweder ein Issue in GitHub anlegen oder an [devsupport@europace2.de](mailto:devsupport@europace2.de) schreiben.

## Nutzungsbedingungen

Die APIs werden unter folgenden [Nutzungsbedingungen](https://developer.europace.de/terms/) zur Verfügung gestellt.
