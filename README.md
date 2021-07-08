# Google Docs - Beispiel zur Kommunikation mit Google Docs
## 1. Funktion
Alles wird in der Setup()-Sektion ausgeführt. Die loop()-Sektion ist leer.

In Setup() werden drei URL-Strings vorbereitet, die im Weiteren Verwendung finden.
- url2:

    > // Fetch Google Calendar events for 1 week ahead

    > String url2 = String("/macros/s/") + GScriptId + "/exec?cal";

    Das Kürzel *cal* nach *exex?* ist dabei unerheblich, es kann auch iregendein andserse Kürzuel sein (z.B. *dum*). Es wird im Weiteren nicht ausgewertet.

    **url2** wird vervendet , um einen POST-Request zu senden. Der POST-Request erhält einen String als **payload**. Der String Payload besteht aus einen Basisanteil **payload_base**, sowie einem aktuellen Nutzanteil, der vor dem POST-Request aufgebaut wird.

    Der Payload-Basisanteil    
    > String payload_base =  "{\"command\": \"appendRow\", \"sheet_name\": \"Tabellenblatt1\", \"values\":

    baut eine **json**-Struktur auf, die als estes ein Kommando enthält, als zweites das angesprochenen Tabellenblatt definiert, und zuletzt Datenpaare aufnimmt, welche im aktellen Nutzteil dann angefügt werden. 
    Im aktuellen Programmcode sieht der der Payload-String wie folgt aus:

    > {"command": "appendRow", "sheet_name": "Tabellenblatt1", "values": "50464,3804"}

    Der komplette POST-String sieht wie folgt aus:
    >script.google.com/macros/s/AKfycbw4W73gKtMl64jiIIE6hh0lVRn1MKXDJg9VXXJkSI9Wo8oqepw/exec?cal - {"command": "appendRow", "sheet_name": "Tabellenblatt1", "values": "50416,3820"}

    Als erstes wird in *doPost()* die gesendete Payload geparsed. Falls es sich um eine json-Struktur handelt, wird nach den String *appendRow* im Command-Teil gesucht, und dann im sheet_name-Teil der Name des Tabellenblatts gelesen. Schließlich werden die Wertepaare im values-Teil in ein Array kopiert. Danach wird dieses Array per *appendRow(dataArr)* in die Tabelle eingefügt.

- url: 
    
    > // Write to Google Spreadsheet String

    > url = String("/macros/s/") + GScriptId + "/exec?value=Hello";
    
    **url** wird verwendet, um einen GET-Request zu senden. Im zugehörigen AppScript wird doGet() augerufen und entsprechend verarbeitet (siehe Sektion AppScript).

    Im aktuellen Code sieht die URL des GET-Request wie folgt aus:

    > script.google.com/macros/s/AKfycbw4W73gKtMl64jiIIE6hh0lVRn1MKXDJg9VXXJkSI9Wo8oqepw/exec?value=Hello

    Die doGet()-Funktion im zugehörigen AppScript wertet die Parameter entsprechend aus und schreibt letzlich den String *Hello* in Zelle A!, den aktuellen Zeitstempel in Zelle B1 und den Wert 0 in Zelle C1.




- url3: 

    >// Read from Google Spreadsheet

    > String url3 = String("/macros/s/") + GScriptId + "/exec?read";

    Im aktuellen Code sieht die URL des GET-Request wie folgt aus:

    > script.google.com/macros/s/AKfycbw4W73gKtMl64jiIIE6hh0lVRn1MKXDJg9VXXJkSI9Wo8oqepw/exec?read

    Die doGet()-Funktion im zugehörigen AppScript wertet die Parameter entsprechend aus und schreibt den aktuellen Zeitstempel in Zelle D1, sowie inkrementiert den Wert in Zelle C1 um eins.



## 2. AppScript

Die GScripts müssen jeweils als WebApp bereitgestell werden. Dazu gibt es zwei Möglichkeiten.

### 2.1 Alter Editor

Hier muss im Menü "Veröffentlichen --> Als Web-App einrichten" die  WebApp deployed werden:

![Deployment im Alten Editor](/./Deploy_as_template.png)

Hierbei ist es wichtig, jeweils eine **neue** Projekt-Version zu wählen. Die GScriptID bleibt dabei die gleiche.
### 2.2 Neuer Editor
Im Neuen Editor wird die Schaltfläche "Bereitstellen --> Neue Bereitstellung" verwendet.
![Deloyment im Neuen Editor](/./Deploy_As_Template_new_Editor.png)

Dann funktionieren beide Arten der Bereitstellung.

 **Allerdings** wird beim Neuen Editor jeweils eine neue GScriptID erstellt. Vgl. https://groups.google.com/g/google-apps-script-community/c/qhiqjGabQpI?pli=1. Hier heißt es , es sei möglicherweise ein Bug.


### 2.3 Funktionen
  - doGet(), doPost()
  - Logging-Funktionen (z.B. BetterLog())

## 3. Versuche und Variationen

### 3.1 Fingerprint etc.

- client->setInsecure()
  
  Dieser Aufruf muss unbedingt erfolgen, da sonst keine Verbindung zum Host zu Stande kommt. `client->Connect()` führt zu Fehler **fail**.

- Fingerprint

  Mit aktiviertem Fingerprint `client->setFingerprint(fingerprint)` kommt trotz **Certificate match** keine Kommunikation zu Stande. 

  > `Certificate match.`

  > `Connecting to script.google.com`

  > `Connection failed. Retrying...`

  > `Connection failed. Retrying...`

  > `Could not connect to server: script.google.com`

  Mithin muss der Abschnitt zu Fingerprint auskommentiert bleiben.

### 3.2 sonstige Hilfsfunktionen der HTTPSRedirect-Library


- ESP8266 vs ESP32
- 

## 4. Debugging.