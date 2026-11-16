# Emulator-Datenbank

`CMS-Datenbank.sql` ist der unveränderte Ausgangsdump. Er enthält `DROP TABLE`
und darf **nicht** in eine Datenbank mit zu erhaltenden Daten importiert werden.
`EMU-Datenbank.sql` ergänzt eine bereits ausgewählte MariaDB-Datenbank:

```sh
mariadb --default-character-set=utf8mb4 -u BENUTZER -p DATENBANK < EMU-Datenbank.sql
```

Vor dem Import ein Backup erstellen und den Import zunächst an einer Kopie
prüfen. Anwendungen und andere Schreibzugriffe während des Imports stoppen.

- Fehlende Tabellen und Spalten werden mit den EMU-Definitionen angelegt.
  Bereits vorhandene Spalten, deren Definitionen und Werte bleiben unverändert;
  Standardwerte werden nur für neu angelegte Spalten übernommen.
- Datensätze mit übereinstimmenden Primär- oder eindeutigen Schlüsseln aus den
  Dumps werden übersprungen, nicht aktualisiert. Tabellen ohne eindeutigen
  Schlüssel werden anhand aller importierten Spalten auf vorhandene Zeilen
  geprüft. Der Import kann erneut ausgeführt werden.
- Vorhandene Tabellen erhalten keine fehlenden EMU-Indizes. Falls eine fehlende
  Auto-Increment-Spalte ergänzt wird, erhält diese jedoch den dafür nötigen
  eindeutigen Index.

Abweichende vorhandene Spaltentypen, zusätzliche Pflichtfelder oder andere
eindeutige Schlüssel und Constraints können den Import mit einem Fehler
abbrechen. Vorhandene Trigger können ebenfalls Auswirkungen haben. Fehler müssen
geprüft werden; der Import ist insbesondere wegen der Tabellenänderungen und
MyISAM-Tabellen nicht vollständig rückrollbar und kann teilweise ausgeführt sein.
Werte und Beziehungen bei kollidierenden IDs werden nicht zusammengeführt:
Insbesondere Benutzer, Berechtigungen und zugehörige Datensätze vor der Nutzung
auf fachliche Konsistenz prüfen.
