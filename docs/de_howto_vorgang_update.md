# Aktualisierung von Vorgangsdaten in Zuge eines Bookmark Requests
Beim Aufruf des bookmark Endpunktes wird der im Request Body als "vorgangId" verwendete BaufiSmart Vorgang mit Daten aus dem Finanzierungsvorschlag angereichert.
Dazu wird die Bearbeitung des Vorgangs kurzfristig von einem techn. User (PMZ31) übernommen und anschließend wieder an den ursprünglichen Bearbeiter übergeben.

Der techn. User aktualisiert dann den Vorgang über die Vorgang-API mit Informationen die über die Kundenangaben API nicht möglich sind.
Insbesondere werden Kommentare zum Vorgang als Ereignis abgelegt.
