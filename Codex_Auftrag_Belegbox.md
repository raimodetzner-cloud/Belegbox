# CODEX: Belegbox Android fertigstellen – verbindlicher Arbeitsauftrag

**Projekt:** Belegbox Beta – Belegscanner mit KI-Erkennung und Cloud-Ablage
**Auftraggeber/Rechteangabe in der App:** Hallesche Gesellschaft für Informationssicherheit KG (HGI KG)
**Stand dieser Übergabe:** 03.11.2026
**Zuerst lesen:** diese Datei, dann `01_PFLICHTENHEFT.md`, `02_TESTPLAN.md`, `03_QUELLCODE_UND_RISIKEN.md`, `04_FRAGEN_UND_ENTSCHEIDUNGEN.md`.
**Quellcode:** `Quellcode/belegbox/` (Flutter-Projekt, Ausgangsbasis Version `0.5.2+13`).

## Dein Auftrag an Codex

Übernimm das vorhandene Flutter-Projekt; **schreibe nicht grundlos eine neue App**. Repariere zuerst Build-, Migrations- und Datensicherheitsprobleme. Implementiere die ausstehende gruppierte Belegübersicht nach Jahr und Monat. Prüfe die in 0.5.2 vorbereiteten Funktionen für Text-Belegliste, verschlüsselte Vollsicherung, Wiederherstellen und gezieltes Löschen. Behalte die auf dem Android-Handy bereits funktionierenden Scanner-, ChatGPT- und Synology-WebDAV-Funktionen bei. Erstelle eine installierbare **Android-APK als Update** zur bestehenden Beta, sofern der originale Update-Signaturschlüssel sicher bereitgestellt wird. **Windows und iOS erst später**, wenn die Android-App auf dem Testgerät zuverlässig funktioniert.

Verarbeite die Anforderungen im Pflichtenheft vollständig. Plane eigenständig und frage den Auftraggeber nur nach wirklich unverzichtbaren Zugängen (z. B. originaler Signaturschlüssel, falls noch nicht bereitgestellt). Keine sensiblen Geheimnisse in Quelltext, Logs, ZIP, Chat oder Screenshots ausgeben.

## Unverrückbare Sicherheitsregeln

1. **Niemals die aktuell installierte Belegbox löschen lassen oder ein neues Paket darüber erzwingen**, solange keine nachweislich vollständige Sicherung der lokalen Belege vorliegt. Die funktionierende Android-Beta v0.5.1 wurde vom Nutzer bereits erfolgreich mit ChatGPT und Synology betrieben. Die 0.5.2 hat in dieser Übergabe ausschließlich Quellcode-Status; sie wurde nicht als APK auf dem Nutzergerät getestet.
2. Für **Update statt Neuinstallation** muss die Paketkennung `de.dnh24.belegbox.beta`, die signierende Zertifikatsidentität und ein höherer `versionCode` erhalten bleiben. Die aktuelle Quelle hat `0.5.2+13`, nächste Version beispielsweise `0.5.3+14`. Aktueller Gradle-Code verwendet Debug-Signing; **nicht ungeprüft** damit signieren. Vergleiche die Zertifikat-Fingerprints mit der installierten Beta bzw. v0.5.1. **Der originale Keystore ist absichtlich NICHT in diesem ZIP enthalten.** Ohne passenden Schlüssel nur eine isolierte Entwickler-Buildvariante erstellen und keine Aktualisierbarkeit behaupten.
3. Kein Test darf produktive Belege auf der NAS oder lokal löschen. Für Löschtests nur explizite Testbelege und Mock-WebDAV benutzen. Tests einer realen NAS nur mit ausdrücklicher Testfreigabe und einem Wegwerf-Testbeleg.
4. Jedes PDF **zuerst lokal sicher speichern**. Vor automatischer lokaler Löschung NAS-Upload abgeschlossen, per HTTPS GET komplett zurückgelesen und SHA-256 verglichen; bei Fehler lokal behalten. Standardmäßig die Löschautomatik ausgeschaltet lassen, bis der Auftraggeber sie aktiviert. Keine Löschung nur aufgrund einer HTTP-PUT-Bestätigung.
5. Keine TLS-Zertifikatsprüfung deaktivieren, keine NAS-Passwörter/ChatGPT-Tokens unverschlüsselt speichern. Kein VPN-Zwang: Synology und QNAP per korrekt erreichbarer **HTTPS/WebDAV-Adresse** und konfigurierbarem Unterordner.
6. Google Drive/OneDrive verwenden bisher Androids Storage Access Framework. Das Zurücklesen aus dem Provider beweist **nicht** die abgeschlossene Synchronisierung zum Cloud-Server. Hier deshalb keine automatische lokale Löschung oder Fernlöschung behaupten, bevor eine echte Ende-zu-Ende-Bestätigung implementiert und getestet wurde.
7. Der ChatGPT-Login erfolgt über den offiziellen, vom Nutzer bereits erfolgreich getesteten OpenAI-Anmeldeweg; NICHT durch Passwortabfrage in der App. Abo-/Modellberechtigung und Freigabe müssen wirklich geprüft werden. ChatGPT Plus und allgemeine API-Abrechnung sind zwei unterschiedliche Modelle; keinen unzutreffenden Zahlungsanspruch versprechen.
8. Exportierte JSON-Textlisten enthalten **nur Beleg-Metadaten und NAS-Verknüpfungen**, nicht die PDFs. Nur die verschlüsselte `.bbx`-Vollsicherung erhält auch noch lokal vorhandene PDFs. Keine Behauptung, JSON allein könne verlorene lokale PDFs wiederherstellen.
9. **Keine Schriftdateien im Übergabepaket.** Das ursprüngliche Projekt referenziert `assets/fonts/DejaVuSans.ttf`; diese Asset-Datei ist in dieser Übergabe bewusst ausgespart. Vor dem Build die PDF-Schrifteinbindung rechtskonform reparieren oder eine frei lizenzierte Schrift aus einer eigenen zulässigen Quelle in der Buildumgebung bereitstellen. Font nicht in die Antwort oder in den Übergabe-Download kopieren.
10. Alle rechtlichen Texte sind Beta-Entwürfe. Keine ungeprüfte DSGVO-Konformität und keinen pauschal wirksamen vollständigen Haftungsausschluss behaupten.

## Gewünschtes Ergebnis von Codex

- Lauffähige, geprüfte Android-App mit allen im Pflichtenheft genannten Funktionen.
- Fertige **APK** mit eindeutiger Version und SHA-256; wenn die korrekte Update-Signatur fehlt, offen als **nicht updatefähig** deklarieren und zunächst nur eine sichere Testvariante ausgeben.
- Aktualisierter vollständiger Quellcode und Änderungsprotokoll; keine Geheimnisse, Fonts oder privaten Signaturschlüssel mitliefern.
- `flutter analyze`, `flutter test`, Android-Release-Build und relevante Integrationstests nachweisbar ausführen; echte NAS/OpenAI-End-to-End-Tests klar als manuell/ausstehend markieren.
- Einfache deutschsprachige Installations- und Testanleitung, **ohne Deinstallation** der vorhandenen App, sowie Wiederherstellungsanleitung für den Notfall.
- Bei jedem Arbeitsschritt kurz dokumentieren: was geändert, was getestet, was noch offen ist. **Keine funktionierende Cloud-Anbindung verschlechtern.**

## Reihenfolge

1. Bestand und Paketidentität prüfen (0.5.2-Quellen gegen vorhandene 0.5.1-Funktionen), Sicherungs-/Delete-Abläufe auditieren.
2. PDF-Schriftreferenz und sonstige Build-Hindernisse beheben; Quellcode kompilieren.
3. Speicherindex/Backup/Restore/Delete auf Datenverlust, fehlerhafte Pfade und Transaktionsabbruch prüfen und korrigieren.
4. **Jahr-Monat-Dropdown** in `Belege` programmieren, mit Zählung, Sortierung nach Rechnungsdatum und schnellen Listen bei 10 Jahren/10.000+ Belegen.
5. Regression der Scanner-, OpenAI-, WebDAV-, Google/OneDrive- und Sicherungsfunktion testen.
6. Release-APK mit **passendem ursprünglichem Signaturschlüssel** erzeugen, Zertifikat vergleichen, Update mit Testdaten auf Android testen.
7. Ergebnisse und manuelle Testschritte dem Auftraggeber übergeben.

## Original-Zustand laut Nutzer

- Android **0.5.1**: Dokumentenscanner und WebDAV auf Synology funktionieren laut Nutzer gut.
- **0.5.2**: Wiederherstellung einer JSON-Belegliste und Löschen von lokal/NAS sind im Quellcode vorbereitet, aber nicht kompiliert/echt getestet.
- **Offen:** Aufklappbare Jahres-/Monatsbelegübersicht – wurde als Wunsch angenommen, ist im 0.5.2-Code noch nicht umgesetzt.
- **Zurückgestellt:** Windows-EXE, direkte Windows-Scanner-Ansteuerung, iPhone-Veröffentlichung. Flutter-iOS-Quellen sind vorhanden, aber keine fertig getestete iPhone-App.

**Lies das komplette Pflichtenheft, bevor du anfängst.**
