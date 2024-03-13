# Aktualisierung von Vorgangsdaten in Zuge eines Bookmark Requests
Beim Aufruf des bookmark Endpunktes wird der im Request Body als "vorgangId" verwendete BaufiSmart Vorgang mit Daten aus dem Finanzierungsvorschlag angereichert.
Dazu wird die Bearbeitung des Vorgangs kurzfristig von einem techn. User (PMZ31) übernommen und anschließend wieder an den ursprünglichen Bearbeiter übergeben.

Der techn. User aktualisiert dann den Vorgang über die Vorgang-API mit Informationen die über die Kundenangaben API nicht möglich sind.
Insbesondere werden Kommentare zum Vorgang als Ereignis abgelegt, Berechnungen am Vorhaben durchgeführt und zum Finanzierungswunsch des Verbrauchers passende Präferenzen gesetzt.
In der Ereignis Historie wird ein Eintrag zur Speicherung von Rating-Ergebnissen abgelegt. Darin sind die Werte Feasibility Rating, Lead Rating und Effort Rating enthalten.

Bei der Übergabe des Bearbeiters werden Status-Informationen in die Ereignis Historie geschrieben und dazu passende Status-E-Mails generiert.



