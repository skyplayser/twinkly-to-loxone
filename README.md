# Twinkly Integration mit Node-RED und Loxone


## Voraussetzungen

- Ein laufender **Node-RED-Server**
- Ein **Twinkly**-Gerät im lokalen Netzwerk
- Ein **Loxone Miniserver**
- Die Node-RED-Palette [`node-red-xled`](https://flows.nodered.org/node/node-red-xled)
- Zugriff auf den **Loxone Config** Editor

---

## 1. Node-RED Flow Importieren

1. Öffne **Node-RED**.
2. Gehe zu **Import** und füge den JSON-Code des Flows ein.
3. Stelle sicher, dass die **IP-Adresse deines Twinkly-Geräts** korrekt ist.
4. Deploye den Flow.

---

## 2. Funktionsweise des Flows

- Die HTTP-GET-Anfrage von Loxone (`/loxone/twinkly/baum/:rgbval`) nimmt einen RGB-Wert entgegen.
- Die Funktion **"Loxone to RGB"** wandelt den Wert in einzelne Rot-, Grün- und Blau-Anteile um.
- Die Funktion **"RGB to HEX"** konvertiert den RGB-Wert in eine Hexadezimalfarbe.
- Der Wert wird an das Twinkly-Gerät gesendet.

---

## 3. Loxone Virtual Output anlegen

1. Öffne den **Loxone Config Editor**.
2. Navigiere zu **Virtuelle Ausgänge**.
3. Füge einen **Neuen Virtuellen Ausgang** hinzu mit der Adresse:  
   ```
   http://<nodered_IP>:1880
   ```
4. Erstelle einen **Virtuellen Ausgangsbefehl** mit:
   - **Einschalten (CmdOn):**  
     ```
     /loxone/twinkly/baum/<v>
     ```
   - **Methode:** GET
   - **Analog:** Ja

> Beispiel: Der Wert `255000000` setzt das Licht auf **Rot**.

---

## 4. Werte für Farben berechnen

Die Farbcodierung in Loxone erfolgt als `RRRGGGBBB`.  
Beispiele:

| Farbe  | RGB-Wert     | Loxone Wert  |
|--------|------------|-------------|
| Rot    | (255, 0, 0) | 255000000   |
| Grün   | (0, 255, 0) | 000255000   |
| Blau   | (0, 0, 255) | 000000255   |
| Gelb   | (255, 255, 0) | 255255000   |

---

## 5. Fehlerbehebung

- Falls Twinkly nicht reagiert:
  - Prüfe, ob die Twinkly-IP korrekt in Node-RED eingetragen ist.
  - Teste den HTTP-Request mit Postman oder im Browser.
- Falls Node-RED keine Daten empfängt:
  - Überprüfe die Debug-Node in Node-RED.
  - Kontrolliere, ob der Loxone-Server die Anfrage sendet.

---

## 6. Sicherheitsbelehrung

Die Kommunikation zwischen Node-RED und dem Loxone Miniserver erfolgt über **Basic-Authentifizierung**, die **nicht verschlüsselt** ist.

### Sicherheitsmaßnahmen:
- **Einen separaten Loxone-Benutzer "API" anlegen.**
- **Dem Benutzer nur minimale Rechte geben** (nur virtuelle Eingänge steuern).
- **Zugriff auf den Miniserver einschränken** (z. B. über VPN oder internes Netzwerk).

---

## 7. Erweiterungen & Anpassungen

- Mehrere Twinkly-Geräte steuern
- Automatisierte Lichtszenen basierend auf Uhrzeit oder Ereignissen
- Integration in Loxone Lichtsteuerung

---



