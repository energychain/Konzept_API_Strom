# Fork mit Vorschlägen zur prozessualen Erweiterung der API

**Hinweis:** Bei diesem Repository handelt es sich um einen Fork des offiziellen EDI@Energy-Projekts. Die hier vorgenommenen Änderungen sind Vorschläge zur Weiterentwicklung und sollen aufzeigen, wie die API-Webdienste über die reine 1:1-Abbildung von EDIFACT-Nachrichten hinaus zu einer echten prozessualen Plattform für die deutsche Energiewirtschaft ausgebaut werden könnten.

## Zusammenfassung der vorgeschlagenen Erweiterungen

Das Ziel dieser Erweiterungen ist es, die heute oft ineffizienten, bilateralen Klärprozesse durch proaktive, synchrone und nachverfolgbare API-Workflows zu ersetzen. Anstatt nur Daten zu übertragen, soll die API den Marktpartnern Werkzeuge an die Hand geben, um die Datenqualität proaktiv zu sichern und Prozesse zu automatisieren.

### 1. Proaktive Datenabfrage & Synchronisation

**Beweggrund:** Ein Großteil der heutigen Klärfälle entsteht, weil die Stammdaten zwischen den Marktpartnern (insb. zwischen Lieferant und Netzbetreiber) nicht synchron sind. Die folgenden "Lese"-Endpunkte ermöglichen es, die "Wahrheit" des Netzbetreibers jederzeit on-demand abzufragen und so Fehler zu vermeiden, *bevor* sie entstehen.

- **Abfrage des Lieferstatus:** Verhindert "Trial-and-Error"-Anmeldungen, indem ein Lieferant vorab prüfen kann, ob er an einer MaLo der aktuelle oder letzte Lieferant ist.
  - **Implementierung:** [`API/Wechselprozesse GPKE/LieferstatusAbfrage.yaml`](./API/Wechselprozesse%20GPKE/LieferstatusAbfrage.yaml)

- **Abfrage des MaLo-Portfolios:** Revolutioniert den MaBIS-Prozess, indem ein Lieferant sein gesamtes aktives Portfolio zu einem Stichtag abfragen kann, anstatt mühsam Listen austauschen zu müssen.
  - **Implementierung:** [`API/Wechselprozesse GPKE/PortfolioAbfrage.yaml`](./API/Wechselprozesse%20GPKE/PortfolioAbfrage.yaml)

- **On-Demand-Abruf von MaLo-Stammdaten:** Ermöglicht einem Lieferanten, die vollständigen Stammdaten (inkl. Bilanzierungsgrundlagen) einer MaLo abzurufen, um die Synchronität sicherzustellen.
  - **Implementierung:** [`API/Wechselprozesse GPKE/StammdatenAbfrage.yaml`](./API/Wechselprozesse%20GPKE/StammdatenAbfrage.yaml)

### 2. Prozessuale Endpunkte zur Fehlervermeidung

**Beweggrund:** Viele transaktionale Prozesse (wie die Rechnungsstellung) scheitern an inkonsistenten Stammdaten. Diese Endpunkte verlagern die Prüfung an den Anfang des Prozesses.

- **Validierung von Abrechnungsgrundlagen:** Ermöglicht einem Rechnungssteller, abrechnungsrelevante Daten (z.B. JVP) *vor* dem Versand einer INVOIC gegen die Daten des Empfängers zu validieren. Dies kann die Klärfallquote bei Rechnungen drastisch senken.
  - **Implementierung:** [`API/Wechselprozesse GPKE/AbrechnungsbasisValidierung.yaml`](./API/Wechselprozesse%20GPKE/AbrechnungsbasisValidierung.yaml)

- **Stammdaten-Änderungsvorschläge (PATCH):** Formalisiert den heute oft informellen Prozess der bilateralen Klärung. Ein Marktpartner kann einen strukturierten Vorschlag zur Korrektur von Stammdaten einreichen. Nach Prüfung und Annahme durch den Dateneigentümer (z.B. VNB) wird die offizielle Marktkommunikation (UTILMD) angestoßen. Dies stellt sicher, dass die Daten beim "Master" bereits korrekt sind, wenn die UTILMD versendet wird.
  - **Implementierung:** [`API/Wechselprozesse GPKE/StammdatenÄnderungsvorschlag.yaml`](./API/Wechselprozesse%20GPKE/StammdatenÄnderungsvorschlag.yaml)

### 3. Angleichung an bestehende EDIFACT-Prozesse

**Beweggrund:** Um die Akzeptanz und Prozesssicherheit zu erhöhen, wurden bestehende API-Definitionen analysiert und um wichtige, aus der EDIFACT-Welt bekannte Kontexte ergänzt.

- **Kontext für Messwertanfragen (ORDERS):** Anfragen wurden um einen `reasonCode` erweitert, um den Geschäftskontext (z.B. Einzug, Jahresablesung) zu übermitteln.
  - **Implementierung:** [`Schema/Werte/Werte_Anfrage/reasonCode.yaml`](./Schema/Werte/Werte_Anfrage/reasonCode.yaml)

- **Spezifikation für Ersatzwerte (MSCONS):** Die Übermittlung von Werten wurde um Details zur Ersatzwertbildung (`reasonOfSubstituteValueFormation`, `substituteValueFormationMethod`) und das vollständige "Fachlichkeit"-Tupel (`energyDirection`) ergänzt, um die Datenqualität und Transparenz zu erhöhen.
  - **Implementierung:** [`Schema/Werte/energyAmountMarketLocation2.yaml`](./Schema/Werte/energyAmountMarketLocation2.yaml)

---

### Hinweis zur Entstehung

Dieser Vorschlag entstand im Rahmen des Community Hubs der Nutzer von "Willi Mako" unter [https://stromhaltig.de/](https://stromhaltig.de/) und wurde durch die [STROMDAO GmbH](https://stromdao.de/) kuratiert.

---

# Hinweise zum Repository (Original)
Dieses Repository enthält alle API-Webdienste und Schemas zur Veranschaulichung des durch EDI@Energy bereitgestellten **Konzepts zur Nutzung und Veröffentlichung von API-Webdiensten**. Das Konzept wird dem Markt vom 01.09.2025 bis zum 15.10.2025 zur Konsultation gestellt.

Die in diesem Repository beschriebenen API-Webdienste werden nach der Konsultationsphase wieder gelöscht und gehen **nicht** zum nächsten Änderungsmanagementtermin produktiv.

Folgende API-Webdienste werden in diesem Repository beschrieben:
  * Anfragen von Messwerten
  * Reklamationen von Messwerten
  * Stornierung von Werten
  * Versand von Werten
  * Anfrage einer Kündigung der Netznutzung
  * Übermittlung eines Kommunikationsdatenblattes eines LF

**Visualisierung der Schnittstellen**  
Eine SwaggerUI-Dokumentation der Schnittstellen aus diesem Repository ist unter folgendem Link abrufbar: https://edi-energy.github.io/Konzept_API_Strom/