### Was sind Präferenzen und wie wirken sie
Über Präferenzen können Verbraucher sowohl die Auswahl aus den möglichen Finanzierungsvorschlägen steuern als auch die Erzeugung bstimmter Vorschläge forcieren. Durch Vorgabe einer Rate, Tilgung oder Zinsbindung ist es möglich die Vorschläge genau nach diesen Parametern zu erzeugen. Sind die Vorgaben nicht erfüllbar, versucht die Market Engine durch Anpassungen machbare Vorschläge um die Präferenz herum zu finden. Präferenzen zur Auswahl beeinflussen das [Ranking](\docs\de_howto_rating_scoring_md) der Vorschläge in der API Antwort. Z.B. kann eine gewünschte Laufzeit von 20 Jahren Vorschläge mit genau dieser oder ähnlicher Laufzeit höher bewerten als Vorschläge mit Laufzeit von 30 oder mehr Jahren.
Es wird aber nicht versucht explizit Vorschläge mit genau 20 Jahren Laufzeit zu erzeugen.

### Welche Präferenzen gibt es
