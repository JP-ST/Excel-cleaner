# 🧹 Excel Sonderzeichen-Bereinigungstool

Ein browserbasiertes Tool zum automatischen Entfernen nicht-deutscher Sonderzeichen aus Excel-Dateien – ohne Installation, ohne Server, vollständig offline nutzbar.

## Hintergrund

Entwickelt für Anwendungen, in denen Excel-Dateien (xls, xlsx) importiert werden – zur Vermeidung von Dubletten.
Dieses Tool bereinigt Excel-Dateien vor dem Import: Sonderzeichen werden in ihre lateinischen Entsprechungen umgewandelt, deutsche Umlaute (ä, ö, ü, ß) bleiben dabei **immer erhalten**.

## Features

- ✅ Entfernt türkische, polnische, kroatische, skandinavische, kyrillische und weitere Sonderzeichen
- ✅ Deutsche Umlaute und ß bleiben vollständig erhalten
- ✅ Alle Formatierungen (Farben, Rahmen, Schriften) der Originaldatei bleiben erhalten
- ✅ Geänderte Zellen werden in der Vorschau grün markiert
- ✅ 100% offline – keine Daten verlassen den Browser
- ✅ DSGVO-konform – keine externe Verbindung, keine Datenübertragung
- ✅ Keine Installation nötig – einfach HTML-Datei öffnen

## Verwendung

[https://vercel.com/jamie-steins-projects/excel-cleane](https://excel-cleaner-ten.vercel.app/) oder:

1. `index.html` herunterladen
2. Im Browser öffnen (Chrome, Firefox, Edge – alle modernen Browser)
3. Excel-Datei hochladen (`.xlsx` oder `.xls`)
4. Vorschau prüfen – grün markierte Felder zeigen alle Änderungen
5. Bereinigte Datei herunterladen – Dateiname bekommt den Zusatz `_bereinigt`

## Beispiele

| Vorher | Nachher |
|--------|---------|
| Çetin | Cetin |
| Šimić | Simic |
| Łukasz | Lukasz |
| Müller | Müller *(bleibt erhalten)* |
| Straße | Straße *(bleibt erhalten)* |

## Technischer Hintergrund

Das Tool arbeitet direkt auf Byte-Ebene im ZIP-Container der `.xlsx`-Datei:

- **Kein Neuschreiben der Datei über SheetJS** – das würde Formatierungen zerstören
- Stattdessen werden nur die relevanten XML-Dateien (`xl/sharedStrings.xml`, `xl/worksheets/sheet*.xml`) im ZIP direkt gepatcht
- Numerische XML-Entities (`&#199;` für `Ç` etc.) werden korrekt dekodiert und re-enkodiert
- Das ZIP wird danach neu zusammengebaut – alle anderen Dateien (Styles, Themes, etc.) bleiben byte-identisch zum Original

**Verwendete Bibliotheken (inline eingebettet, kein CDN):**
- [SheetJS](https://sheetjs.com/) – zum Lesen und Parsen der Excel-Datei für die Vorschau
- Eigene ZIP-Implementierung via `DecompressionStream` / `CompressionStream` Web API

## Browser-Kompatibilität

| Browser | Unterstützt |
|---------|-------------|
| Chrome 80+ | ✅ |
| Firefox 113+ | ✅ |
| Edge 80+ | ✅ |
| Safari 16.4+ | ✅ |

## Lizenz

MIT – frei verwendbar, auch kommerziell.
