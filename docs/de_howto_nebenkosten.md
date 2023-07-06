## Behandlung und Berechnung von Nebenkosten über die #passt Vorschläge API

# Notar- und Grundbuchkosten
Der Standard-Vorgang zum Erwerb einer Immobilie (KAUF, KAUF_NEUBAU_VOM_BAUTRAEGER) wir mit einer Nebenkosten-Pauschale für Notargebühr und Grundbuchkosten von 1,8% vom Kaufpreis berechnet.
Dabei entfallen ca 1,2% auf Kosten beim Notar (Kaufvertrag, Grundschuldbestellung) und 0,6% auf Kosten beim Amtsgericht. Die Pauschale entspricht den anfallenden Kosten einer Transaktion mit 150.000€ Kaufpreis und 100% Finanzierung.
Aufgrund der Fixkosten-Anteile in der Gebührenrechnung läge die reale Kostenpauschale bei Kaufpreisen unter 100.000€ bei ca. 2% und bei Preisen ab 300.000€ bei ca. 1,6%. Wir haben uns für 1,8% als Kompromiss entschieden.

Beim Verwendungszweck "eigenes Neubauvorhaben" werden die Nebenkosten nur dann auf den Grundstücksanteil der Gesamtkosten berechnet, wenn dieser als noch nicht bezahlt übertragen wird.
Für den Teile Herstellungskosten, Modernisierung und Eigenleistungen werden keine Nebenkosten angerechnet.

# Maklergebühren
Es besteht die Möglichkeit in der API-Anfrage eine Maklergebühr in EUR mitzuschicken. Sobald der Wert _maklergebuehr_ in der Anfrage nicht _null_ ist wird dieser für die Nebenkostenrechnung verwenden. Insbesondere bei Privat-Käufen oder Bauträger Projekten kann hier explizit 0€ übertragen werden um die Einrechnung einer Maklergebühr in die Gesamtkosten zu unterbinden.
Ohne Übertragung der _maklergebuehr_ in Erwerbsfällen (KAUF, KAUF_NEUBAU_VOM_BAUTRAEGER) wir hier die Hälfte der für jedes Bundesland typischen Gebühr angesetzt. Bsp. Berlin: 3,57%
Da die Maklergebühr als signifikanter Anteil der Nebenkosten aus Eigenkapital aufgebracht werden muss, ist insbesondere bei der Maximal-Kaufpreis-Berechnung darauf zu achten ob real Maklergebühren anfallen. Sollte dies nicht der Fall sein, kann mit der Angabe "_maklergebuehr_": 0 der maximal mögliche Kaufpreis aufgrund der Beleihungswertkriterien um bis zu 20% höher ausfallen. 

# Grunderwerbsteuer
Die Grunderwerbsteuer wird je nach Bundesland aus den dort gültigen Sätzen ermittelt. Aus diesem Grund sind in der API-Anfrage auch immer mindestens Bundesland oder Postleitzahl anzugeben. Sollte noch kein Immobilienwunsch existieren, kann über die Auswahl eines beliebigen Bundeslandes die zu erwartenden Nebenkosten-Summe beeinflusst werden.
Bei Neubau-Projekten wird die Grunderwerbsteuer nur auf den noch nicht bezahlten Grundstücksanteil berechnet.
