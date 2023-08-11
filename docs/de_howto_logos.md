# Verwendung von Produktanbieter-Logos in Client-Anwendungen

## Logo API
Unter folgender URL können Produktanbieter-Logos abgerufen und bei der Anzeige von Finanzierungsvorschlägen verwendet werden:

https://www.europace2.de/produktanbieter-logos/logo/[PRODUKTANBIETER-BEZEICHNUNG/-ENUM].[png/svg]

Für den Platzhalter [PRODUKTANBIETER-BEZEICHNUNG/-ENUM] können die in der Vorschläge API unter dem Knoten _produktAnbieter_ gelieferten Namen verwendet werden.
Je nach verwendeter Endung wird entweder die svg-Version des Logos oder ein png-Thumbnail ausgeliefert. Wie empfehlen die svg-Version.

Bsp: [https://www.europace2.de/produktanbieter-logos/logo/Commerzbank.svg], dabei ist Commerzbank der Wert des Feldes _produktAnbieter_.

## Encoding
Da einige Prduktanbieter-Namen Umlaute oder Sonderzeichen enthalten können, empfehlen wir, vor der Verwendung des Namen in der URL diese zu encoden.
Dazu stehen in allen gängigen Sprachen passende Funktionen zur Verfügung, z.B. in JavaScript _encodeURIComponent()_

Bsp.: PSD Hessen-Thüringen wird zu PSD%20Hessen-Th%C3%BCringen
Die komplettte URL sieht dann folgendermaßen aus: [https://www.europace2.de/produktanbieter-logos/logo/PSD%20Hessen-Th%C3%BCringen.svg]

## Besonderheiten
- Für Produktanbieter wie Sparkassen oder Volksbanken wird gewöhnlich das Verbund-Logo verwendet. So wird für alle Sparkassen das gleiche Logo ausgeliefert.
- Sollte für einen Produktanbieter kein Logo gefunden werden, so wird ein transparentes leeres Logo mit dem Namen _\_LEER.svg_ geliefert
- Bei Kombinations-Vorschlägen (Nachrang, KfW, Verbund-Angebote) hat jeder Baustein einen anderen Produktanbieter, so dass auch alle Bausteine ein separates Logo benötigen.
  Es können bis zu 3 Bausteine in Kombination auftreten, so dass der Client auch bis zu 3 Logos darstellen sollte.
- Neben der Produktanbieter-Bezeichnung können auch die ENUMS der ProduktanbieterId oder pauschale Kurznamen (Bsp.: spk.svg) als Abrufparameter verwendet werden.

## Rechtliches
- Die Verwendung der Logos ist im Rahmen der Anzeige von Vorschlägen und Konditionen des jeweiligen Produktanbieters auf den Websites der Partner erlaubt. Die weitere Verarbeitung der Leads und Finanzierungsvorschläge muss auf den Plattformen der Hypoport-Unternehmen erfolgen. Eine Verwendung für andere Zwecke wie Content-Anreicherung oder Marketing ist nicht zulässig.
