### Was sind Präferenzen und wie wirken sie
Über Präferenzen können Verbraucher sowohl die Auswahl aus den möglichen Finanzierungsvorschlägen steuern als auch die Erzeugung bestimmter Vorschläge forcieren. Durch Vorgabe einer Rate, Tilgung oder Zinsbindung ist es möglich die Vorschläge genau nach diesen Parametern zu erzeugen. Sind die Vorgaben nicht erfüllbar, versucht die Market Engine durch Anpassungen machbare Vorschläge um die Präferenz herum zu finden. 
Präferenzen zur Auswahl und qualitativen Berwertung der Vorschläge beeinflussen das [Ranking](de_howto_rating-scoring.md) in der API Antwort. Z.B. kann eine gewünschte Laufzeit von 20 Jahren Vorschläge mit genau dieser oder ähnlicher Laufzeit höher bewerten als Vorschläge mit Laufzeit von 30 oder mehr Jahren. Im Gegensatz zur Vorgabe einer Zinsbindung oder Rate wird bei einer Laufzeit-Präferenz aber nicht versucht explizit Vorschläge mit genau der gewünschten Laufzeit zu erzeugen.

### Welche Präferenzen gibt es

Die Details der Präferenzen sind in der [YAML](https://github.com/europace/baufi-passende-vorschlaege-api/blob/main/api/baufi-passende-vorschlaege-api.yaml) unter _'#/components/schemas/Praeferenzen'_ ab ca. Zeile 577 beschrieben.

Folgende Präferenzen haben direkten Einfluss auf die Finanzierungswünsche:
- rate
- tilgung
- zinsbindungInJahren
- optionaleSondertilgung (überschreibt den Standardwert von 5%)
- bereitstellungszinsfreieZeit
- produktAnbieterIds (explizit in das Ergebnis aufzunehmende Produktanbieter)
- finanzierungsBestandteile (UNKOMPLIZIERTE_FINANZIERUNG, OEFFENTLICHE_FOERDERUNGEN, WEITERE_PRODUKTE)
  

Die folgenden Präferenzen beeinflussen optional das Ranking der erzeugten Finanzierungswünsche:
Vorraussetzung: es muss in der API-Anfrage unter Metadaten der Wert "feature": "COMPLETE_SCORING" übermittelt werden.
- faelligkeitsdatum (Wann wird der Kaufpreis fällig, bis wann sollte die Bereitstellung des Darlehens kostenlos sein? Bei Umschuldungen und Prolongationen ist das Feld _zinsbindungBis_ in der Liste der abzulösenden Darlehen als Umschuldungstermin zu verwenden, auch wenn eine Sonderkündigung nach §489 BGB vorliegt.)
- kreditEntscheidungsZeit (Bis wann muss die Kreditentscheidung vorliegen? Schnelle Produktanbieter werden hier bevorzugt.)
- laufzeit (Wie lang ist die Dauer bis zur Rückzahlung des am längsten laufenden Darlehens unter Annahme gleichbleibender Zinsen?)
