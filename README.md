# DaWeSys Technical Report

<!-- TODO: Inhaltsverzeichnis, wenn fertig. -->

## Tech Stack

* **Wer?** Oscar
* Grundlegende Entscheidungen ([Midterm slides] 5-8)
    * Android
    * Expo
    * React Native
    * Typescript
* warum? was macht die Technologie/ wie funktioniert sie? Unsere Erfahrung mit ihrer Anwendung
* Was für Privacy Conerncs gab es aufgrunddessen, dass es sich um eine Health Application handelt. Wie haben sich diese auf die Wahl von Technologien und oder Architektur der Applikation ausgewirkt. (Storage, Cloud, Backups,..)
* Apple Dev Account Problematik erklären
* Prettier / ESLint

## ~~Organisation / Agile Entwicklung (GH Projekte) - (Nochmal Elias fragen ob das in den Report gehört - tut es nicht)~~


## CI

* **Wer?** Oscar oder Max
* [Midterm slides] 12-19

## Testing

* **Wer?** Simon oder Max
* [Final slides] 7-10
* Frameworkwahl und ihre Vor-/Nachteile (Jest/Jasmin,..)
* Welches Framework für welche Art von Test
    * E2E
    * Snapshot
    * Unit Test?
* Umsetzung der Test mit Codebeispielen

## Design

* **Wer?** Dino

Eine weitere Herausforderung war es, dass von Kopfsachen vorgegeben Design nach Standars der Benutzerfreundlichkeit (Usability) anzupassen, sowie dieses barrierefrei (Accessibility) zu gestalten.
Zur Beurteilung, ob etwas benutzerfreundlich ist, wurde sich an der DIN EN ISO 9241-11 orientiert. Diese beinhaltet, unter anderem, die Definition das Benutzerfreundlichkeit ein Produkt aus Effektivität, Effizienz und zufriedenstellende Benutzung sei.

Im Falle von Kopfsachen bedeutete dies, dass eine Aufgabe schnell und eindeutig lösbar ist, ohne das die Anwendenden hinterher frustriert sind.

Durch die Entwicklung einer App gab es andere Herausforderungen, als bei der Entwicklung einer Desktop Anwendung. Zum einen durch die deutlich kleineren Bildschirme, zum anderen die Darstellung von Informationen (hauptsächlich Text in diesem Fall).

In den nachfolgenden Graphiken wird am Beispiel der Sozialen Unterstützung eine benutzerfreundliche Umgestaltung skizziert.
Die sich in der Graphik befinden roten Punkte sowie Pfeile symbolisieren hier jeweils eine Touch- sowie Scrollinteraktion durch einen Nutzenden.

Schnell ist zu erkennen, dass wichtige Informationen, die für das Bearbeiten der Aufgabe relevant sind, sich weit weg von dem zu bearbeitendem Elementen (Kreise) befinden.
Der User ist somit gezwungen, ständig zwischen scrollen zu relevanten Informationen, sowie dem eigentliche Bearbeiten der Aufgabe zu wechseln.

Dies erscheint weder effizient, noch ist das ständige Wechseln sonderlich zufriedenstellend und kann demnach ein erhöhtes Frustrationspotenzial aufweisen, desto mehr der Bildschirm in die Länge wächst.

Ein Lösungsansatz war es, die Informationen in mehrere Bildschirme einzuteilen, sodass die Aufgabe Schritt für Schritt bearbeitet werden kann, ohne zwischen Aktivitäten (scrollen, lesen, bearbeiten) wechseln zu müssen.

Bei der Unterteilung wurde sich an Designprinzipen wie, Gestalt der Ähnlichkeit (Gruppierung ähnlicher Inhalte), sowie Hierarchie durch Priorität, gehalten.

Somit konnten den Nutzenden die wichtigsten Informationen zu erst präsentiert werden, sodass die Aufgabe, effektiv, effizient sowie zufriedenstellen lösbar ist.

Eine weitere benutzerfreundliche Umgestaltung spiegelt sich im Wiki wieder. Hier war die Herausforderung, das Design für mobiles Lesen zu optimieren.

Wie aus … hervorgeht, lesen Anwendende Artikel nicht von Anfang bis zum Ende, sondern scannen schriftliche Inhalte nach Informationen, die für sie gerade relevant sind und lesen erst dann.
Um dieses Verhalten zu unterstützen, erfolgte eine Optimierung des Layouts, sodass Artikel in Unterschriften grob zusammengefasst wurden und diese bei Bedarf ausklappbar sind.
Die Umsetzung erfolgte mithilfe von Markdown Dateien, welche im Backend hinterlegt sind und im Frontend entsprechend geparst werden.

Des weiteren wurde sich dafür entschieden, das Design für eine möglichst barrierefreie Nutzung zu optimieren. Hierfür wurden die Richtlinien der WCAG sowie von Matarial.io, da dies Primär eine für Android entwickelte App ist, umgesetzt. Diese Standars beinhalteten unter anderem Vorgaben für Farbkontraste, Abstände, Größen und Beschriftungen.
In der nachfolgenden Graphik kann die Umsetzung dieser Richtlinien nachvollzogen werden.

Für die Entwicklung wurden diese Vorgaben als globale Konstanten angelegt, sodass diese immer schnell zur Hand waren.

## Authentifizierung

**Wer?** Ludwig

Bei der Implementierung der Authentifizierung musste sowohl auf die Vorgaben des Auftraggebers Kopfsachen, als auch auf die Spezifikation des Backends Rücksicht genommen werden.

Kopfsachen wünscht sich eine einmalige Einleitung in die App. Einleitend wird der User bei der erstmaligen Öffnung der App auf einem Willkommens Bildschirm landen. Hier wird der User durch eine Erklärung in den allgemeinen Nutzen und die Funktionen der App eingeführt.

Wenn sich der User weiter durch die Einleitung klickt, landet er auf einer weiteren Seite, auf welcher er schließlich durch einen einfachen Klick auf den „Weiter“ Button einen neuen Account anlegt. Ebenfalls hat er hier die Option mit Hilfe eines QR Codes einen bereits vorhanden Account auf dieses Gerät zu übertragen. Nach dem Klicken des Buttons soll der User auf dem Home Screen der App landen. Dieser wird nun bei jeder neuen Öffnung der App angezeigt.

Das Backend gibt nach dem abgeschlossenen Authentifizierung-API-Flow einen accountKey zurück, welcher dem User eindeutig zugeordnet ist.

### React Context:

Um den accountKey in verschiedenen Bereichen der App benutzen zu können, wurde bei der Implementierung React Context benutzt. React Context ermöglicht es Daten innerhalb der App in verschiedenen Komponenten zu benutzen, ohne diese über Props von Komponente zu Komponente explizit zu übertragen. Ein Vorteil davon ist, dass accountKey nur in den Komponenten benutzt wird, die dafür auch vorgesehen sind.

Um dies umzusetzen wurde eine Komponente UserProvider erstellt, die den Rest der App auf höchster Ebene „verpackt“. Über die Funktion useUserContext kann schließlich innerhalb jeder Komponente innerhalb des UserProvider auf den accountKey zugegriffen werden.

Ebenfalls ist React Context nützlich, um die von Kopfsachen gewünschte Einleitung umzusetzen. So kann die von React zur Verfügung gestellte Funktion UseCallback innerhalb der selbst geschriebenen addUser Funktion des UserProviders benutzt werden, sodass sich die App automatisch neu lädt sobald der accountKey neu angelegt wird.

In der Komponente AppWrapper wird der Context benutzt um das bedingte Anzeigen der Einleitung, oder des Home Bildschirms zu realisieren. Wenn diese Komponente neu geladen wird, landet der User also bei gesetzten accountKey auf dem Home Bildschirm und bei nicht gesetzten accountKey auf dem Willkommens Bildschirm.

Mit Hilfe der Funktion useEffect von React wird bei unserer Implementierung der localStore (umgesetzt mit SecureStore von Expo) automatisch erneuert, wenn sich der accountKey ändert, sodass der accountKey im globalen Kontext persistiert wird.


### Authentifizierung-API-Flow:

Der API-Flow für die Authentifizierung läuft folgendermaßen ab:

Nachdem der Button „Weiter“ am Ende der Einleitung geklickt wurde, wird eine GET Anfrage an den Endpoint self-service/registration/api geschickt.

Aus der Response wird die URL für die nächster POST Anfrage ausgelesen. Daraufhin wird die POST Anfrage geschickt, aus deren Antwort der accountKey des Users beschafft wird. Schließlich wird der accountKey mit der Funktion setAccountKey im State des UserProviders aktualisiert.

Es wurde ein Authentication Client  geschrieben und verwendet, welcher vom allgemeinen Base Client erbt. Dieser setzt die URL zusammen, die für den API Call gebraucht wird und stellt asynchrone Funktionen bereit, die den Ablauf der Registration, oder des Logins abwickeln.


## Derselbe Account auf mehreren Geräten

Bei traditionellen Auth-Konzepten mit Benutzername oder E-Mail und Password kann Nutzer:innen zugemutet werden, sich ihre selbst gewählten Zugangsdaten selbst zu merken oder zu sichern, und sie auf neuen Geräten einfach wieder einzugeben. Bei dem bereits beschriebenen Auth-Konzept des Mindtastic-Backends entfallen solche selbstgewählten Pseudonyme und Passwörter jedoch zugunsten eines zufällig generierten, 36-stelligen Codes, dem "Account Key". Hier wären das Abschreiben oder gar das Merken sowie die manuelle Eingabe ein schreckliches Nutzerinnenerlebnis.

Trotzdem halten wir es jedoch für unabdingbar, dass Nutzer:innen das gleiche Profil auf mehreren Geräten nutzen können. Deshalb bedarf es einer Methode, den Account Key auf einen anderen Client zu übertragen. Da wir eine mobile App entwickeln, können wir hier Gebrauch von der Kamera machen. Inspiriert von Anwendungen wie Whatsapp Web, Telegram Web oder 2FA-Apps haben wir uns für einen QR-Code entschieden.

### Implementierung

Die Implementierung dieses Features bestand aus zwei Teilen:

* Ein neuer Screen soll den Account Key in einem QR-Code kodieren und anzeigen.
* Als Alternative zur erneuten Registrierung soll beim ersten Start (oder nach Löschung des Profils) die Möglichkeit gegeben werden, einen QR-Code scanner zu öffnen. Nach Einscannen eines korrekten QR-Codes soll sich mit dem darin enthaltenen Account Key eingeloggt werden.

#### Anzeigen des QR-Codes

Zwar gibt es Out-of-the-Box QR-Code Komponenten für React Native [[1], [2]], jedoch sind diese etwas einschränkend in ihren Möglichkeiten. Da es mit nur minimalem Mehraufwand möglich ist, mithilfe einer universellen QR-Code-Library wie [node-qrcode]
SVG-Quelltext zu erzeugen und diesen mithilfe von [react-native-svg] anzuzeigen, haben wir uns für diesen Weg entschieden.

Da sich um uns herum heutzutage viele QR-Codes befinden, die wir beim späteren scannen nicht versehentlich als Account Keys unserer App fehlinterpretieren wollen, formatieren wir ihn als URI der Form `kopfsachen:account/6aa5f1b7-fe69-4cb8-a8b0-f50089888a3f`.

```tsx
QRCode.toString(`kopfsachen:account/${accountKey}`)
  .then(setQrCodeXml);
// ...
return <SvgXml xml={qrCodeXml} />;
```

### Scannen des-QR Codes

Expo stellt die Library [expo-barcode-scanner] bereit, die neben Barcodes und einer Reihe anderer Formate auch QR-Codes lesen kann. Dabei kann die Kameravorschau einfach als React-Komponente eingebunden werden. Wird ein Code jedweder Art erkannt, wird ein Callback aufgerufen.

```tsx
export const ACCOUNT_QR_CODE_PREFIX = 'kopfsachen:account/';

const handleBarCodeScanned: BarCodeScannedCallback = ({ type, data }) => {
  if (type !== BarCodeScanner.Constants.BarCodeType.qr) {
    console.debug('Scanned something other than a qr code, ignoring');
    return;
  } else if (!data.startsWith(ACCOUNT_QR_CODE_PREFIX)) {
    console.debug(`QR code data does not start with '${ACCOUNT_QR_CODE_PREFIX}', ignoring`);
    return;
  }

  const accountKey = data.substring(ACCOUNT_QR_CODE_PREFIX.length);
  if (accountKey.length !== 36) {
    console.debug(`QR code does not contain a valid account key after '${ACCOUNT_QR_CODE_PREFIX}', ignoring`);
    return;
  }

  setAccountKey(accountKey);
};

return <BarCodeScanner
  barCodeTypes={[BarCodeScanner.Constants.BarCodeType.qr]}
  onBarCodeScanned={handleBarCodeScanned}
  style={StyleSheet.absoluteFill}
/>;
```

Leider erwies sich die BarCodeScanner-Komponente beim Styling des Screens als Problemquelle. Es muss eine Höhe und Breite angegeben werden, in die die Kameravorschau dann so groß wie möglich mit dem korrekten Seitenverhältnis eingesetzt wird. Da die Library aber die Größe der Kameravorschau nicht verfügbar macht, bleiben je nach Gerät immer oben und unten schwarze Ränder, die nicht entfernbar sind. Der Screen ist vermutlich auf allen Geräten nutzbar, sieht aber vermutlich auf einigen (möglicherweise Tablets) nicht gut aus.

### Weiterentwicklung

Um aus diesem Feature den besten Nutzen zu schlagen, sollten auch in die Webanwendungen eine Funktion zum Anzeigen eines QR-Codes mit dem gleichen Format erhalten. Möglicherweise könnten diese auch angeschlossene Kameras nutzen, um einen QR-Code-Scanner zu implementieren.

Außerdem sollte, für den Fall, dass die Kamera des genutzten Smartphones kaputt ist oder anderweitig nicht genutzt werden kann, die Option zur manuellen Eingabe des Account Keys gegeben werden. Dazu haben wir bereits einen Knopf hinzugefügt, der aber noch keine Funktion hat.

<img alt="First start screenshot" src="assets/user-setup.png" height="500">
<img alt="QR code scanner screenshot" src="assets/scanner.png" height="500">

<br/>

<img alt="Profile overview" src="assets/profile-overview.png" height="500">
<img alt="QR code" src="assets/profile-export.png" height="500">

[1]: https://www.npmjs.com/package/react-native-qrcode

[2]: https://www.npmjs.com/package/react-native-qrcode-svg

[node-qrcode]: https://www.npmjs.com/package/qrcode

[react-native-svg]: https://docs.expo.dev/versions/latest/sdk/svg/

[expo-barcode-scanner]: (https://docs.expo.dev/versions/latest/sdk/bar-code-scanner/)


## Sicherheitsnetz
Ein großer Teil der Kopfsachen-App besteht aus den von Kopfsachen vorgegebenen Starkmachern. Diese erlauben es den Nutzer:innen auf verschiedenen Wegen Probleme mit mentaler Gesundheit zu bewältigen bzw. vorzubeugen.

Ein besonders wichtiger Starkmacher ist das Sicherheitsnetz. Dort tragen die Nutzer:innen Themen, Personen, Aktivitäten etc. ein, die sich im tagtäglichen Leben positiv auf die mentale Gesundheit auswirken oder die im schlimmsten Fall ein "Auffangnetz" bilden.


### Sicherheitsnetzeinträge im Frontend
Zunächst wurde der Datentyp "SafetyDType" definiert, welcher die eingetragenen Daten der Nutzer:innen durch den Erstellungsprozess eines Sicherheitsnetzeintrags trägt.
SafetyNetDType besteht aus vier Teilen:

````tsx
export type SafetyNetDType = {
  id: number;
  type: string;
  name: string;
  strategies: [string, string, string];
};
````

1.  "id" ist eine im Backend generierte Nummer, die den Eintrag eindeutig identifiziert
2.  "type" ist der von Nutzer:innen eingetragene Typ des Eintrags
3.  "name" ist ist der von Nutzer:innen eingetragene Titel des Eintrags
4.  "strategies" ist ein Array aus drei Strings, von denen mindestens einer ein nicht-leerer string sein muss


### Verwendung des Sicherheitsnetzes
Die Erstellung eines Sicherheitsnetzeintrages in der App besteht aus zwei Schritten.

Im ersten Schritt tragen die Nutzer:innen einen Titel ein und wählt eine, von fünf möglichen, Kategorien für den Sicherheitsnetzeintrag.
Der eingetragene Titel wird unter "name" gespeichert und das ausgewählte Icon bestimmt den "type" (z.B. Pfotenicon -> "type": "pets"). Nach Bestätigung wird überprüft ob ein "name" und "type" angegeben wurden und an den zweiten Schritt weitergeleitet.

<img alt="Safety-Net step 1" src="assets/security-net1.jpg" height="500">

Im zweiten Schritt tragen die Nutzer:innen mindestens eine und maximal drei Strategien ein, wie dieser Sicherheitsnetzeintrag helfen kann. Diese werden unter "strategies" gespeichert. Hier wird auch überprüft ob am Ende genug Daten (mindestens eine eingetragene Strategie) vorliegt.

<img alt="Safety-Net step 2" src="assets/security-net2.jpg" height="500">

Nach Bestätigung des zweiten Schrittes werden die Daten aus dem SafetyNetDType per POST-Request an das Backend versendet.

### Kommunikation mit dem Backend
Um Schreibarbeit zu sparen und die Kommunikation mit dem Backend in einem zentralen Punkt zu halten, wurde die "AuthenticatedBaseClient" Klasse erstellt. Diese enthält Archetypen für verschiedene Arten von Anfragen (POST, GET, etc.), welche per fetch-API,mit der Session-ID im Header, an das Backend versendet werden.

````tsx
export default class BaseClient {
  constructor(protected baseUrl: URL | string) {}

  protected async request(method: string, path: string, options?: RequestInit): Promise<Response> {
    const url = path.includes('://')
      ? path // absolute
      : new URL(path, this.baseUrl);

    const response = await fetch(url, {
      method,
      ...options,
    });

    if (response.ok) {
      return response;
    } else {
      throw new Error(`${response.statusText}. Error code ${response.status}.`);
    }
  }

  protected async get<R>(path: string, options?: RequestInit): Promise<R> {
    const response = await this.request('GET', path, options);
    return await response.json();
  }

  //...
}
````

Zur Nutzung des AuthenticatedBaseClients in einem Feature, wird beispielsweise für das Sicherheitsnetz, eine Klasse "SecurityNetClient" erstellt, welche vom AuthenticatedBaseClient erbt.
Dort werden die Daten aus dem Frontend und die gewollten Endpunkte (z.B. "/safetyNet" oder "/safetyNet/3") and die Anfragen gebunden.
Außerdem können mögliche Netzwerkfehler oder unerfolgreiche Anfragen abgefangen werden und Fehlermeldungen ausgegeben werden.

````tsx
export default class SecurityNetClient extends AuthenticatedBaseClient {
  public async getItems(): Promise<SafetyNetDType[]> {
    const result = await this.get<SafetyNetDType[]>('/safetyNet').catch(() => {
      Alert.alert('Keine Verbindung.', 'Leider besteht zurzeit keine Verbindung zu unserem Server :(');
      return [];
    });
    return result;
  }

  //...
}
````

### Weitere Features im Sicherheitsnetz

Zusätzlich zum Erstellen von Einträgen haben die Nutzer:innen noch die Möglichkeit alle existierenden Einträge eines Types einzusehen und zu verändern bzw. löschen.

Dafür holt sich der SecurityNetClient per GET-Request alle Einträge eines/-r Nutzer:in. Diese werden dann nach Typ gefiltert und die resultierende Liste als Kachellayout angezeigt. Dort kann ein erstellter Eintrag auch wieder gelöscht werden. Dafür klicken die Nutzer:innen das "X" am Eintrag im Kachellayout an. Daraufhin wird eine DELETE-Request an "safetyNet/{id}" gesendet, um den Eintrag im Backend zu löschen. "id" ist hierbei die dem Eintrag zugewiesene id.

<img alt="Safety-Net list of entries" src="assets/security-net_itemview.PNG" height="500">

Zum Verändern eines Eintrags wird auf einen der Einträge im Kachellayout geklickt, woraufhin man durch dieselben Schritte wie bei der Erstellung eines Eintrages geleitet wird.
Hierbei sind die Daten aus dem bestehenden Eintrag schon in die zugehörigen Felder eingetragen.
Nach Beendigung wird überprüft, ob der Eintrag tatsächlich verändert wurde, und nur dann per PUT-Request an "/safetyNet/{id}" aktualisiert. Dadurch werden Verdopplungen von Einträgen vermieden.

<img alt="Safety-Net modify existing entry" src="assets/security-net_modify_component.PNG" height="500">

### Hürden bei der Implementierung
Bei der Erstellung der Kategoriewahl ist eine Diskrepanz zwischen der Vorgabe von Kopfsachen und den akzeptierten Kategorien der API aufgefallen.
Um dies zu lösen, wurden zunächst dazu in der Gruppe mögliche Alternativen besprochen.
Diese wurden darauf implementiert und in einer Pull-Request für die anderen Teams einsehbar gemacht.
Die Alternative in der Pull-Request wurde sowohl auf Github überprüft als auch im "ProgPrak"-Channel auf Mattermost besprochen. Nachdem die vorgeschlagenen Änderungen von mehreren Seiten akzeptiert wurden, konnte die Pull-Request gemerged werden.


## TODO

Entwickeln
* ~~E2E-Tests~~ :x:
* Authentifizierung :heavy_check_mark:
* QR-Code :heavy_check_mark:
* Polishen

Ideen generieren
* woran habe ich über das Semester gearbeitet? Was ist diskussionswürdig/ interessant?
* z.B. Zusammenarbeit untereinander/mit anderen Teams; Hürden/Dinge die gut gelaufen sind

Beispiele von Elias
* https://medium.com/swlh/how-to-write-a-technical-article-and-be-read-ccbecd30a66c
* https://medium.com/@weblab_tech/graphql-everything-you-need-to-know-58756ff253d8
* https://blogonyourown.com/tech-blogs

Slides:

* [Midterm slides]
* [Final slides]

[Midterm slides]: https://docs.google.com/presentation/d/1NGj9W046PPFRzViuPo1Oqs9OgjPQGymZTuSad5sK_3A/edit
[Final slides]: https://docs.google.com/presentation/d/1UYDbpvOzaAlohBbl47hufxIubeY6l2X04z7tfMnBxt0/edit

## Termine

> * Morgen (15.8.) wird kein Meeting stattfinden, obwohl es ein Montag ist
> * Am darauffolgenden Montag (22.8.) machen wir regulär ein Meeting, auch, wenn es vermutlich nicht super viel zu besprechen gibt
> * Ungefähr am 26.8. (Fr) - 29.8. (Mo) sollte die Entwicklung so ziemlich abgeschlossen sein. Dann setzen wir uns zusammen, um in der App nocheinmal alle Features durchzugehen, um etwaige Bugs und Ungereimtheiten zusammenzutragen
> * Am 5.9. (Montag vor der Abgabe) sollte sich der Report dem Ende nähern. Dann setzen wir uns nocheinmal zusammen, um alles durchzugehen.
> * Am 9.9. ist die Abgabe. Dazu schicken wir Elias einen Link zu einem (neuen) GitHub-Repo mit unserem Report als README.md.

###### tags: `uni`
