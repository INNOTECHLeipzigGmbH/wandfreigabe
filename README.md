# Wandfreigabe — AWO Psychiatriezentrum Halle, Haus 2

Statische Web-App zur Freigabe von Trockenbau-Wänden pro Firma, direkt im
Grundriss (EG/OG). Die Wandgeometrie stammt aus dem Original-CAD-Plan (DXF,
Layer `AK-LEICHTBAU`) und wurde automatisiert extrahiert. Der Freigabe-Status
wird live über Supabase synchronisiert — alle Beteiligten sehen denselben
Stand in Echtzeit.

## Nutzung

1. Repo als GitHub Pages veröffentlichen (Settings → Pages → Branch `main`,
   Ordner `/`), oder `index.html` lokal im Browser öffnen.
2. Firma oben auswählen. Über **„Firmen verwalten"** lassen sich weitere
   Firmen hinzufügen oder entfernen — live für alle sichtbar.
3. Wand im Plan anklicken → pro Firma einzeln freigeben.
   Farbe zeigt den Gesamtstand: rot = offen, gelb = teilweise,
   grün = komplett freigegeben.
4. Reiter **„Protokoll"** zeigt den kompletten Verlauf.
   - **„Protokoll kopieren"** legt eine Textfassung in die Zwischenablage.
   - **„Als PDF exportieren"** erzeugt ein Dokument mit eingefärbtem Plan,
     Statustabelle je Wand und Verlauf — zum Herunterladen/Versenden.

## Repo-Struktur

```
index.html               — die App (keine Build-Tools nötig)
plan-eg.jpg               — Grundriss Erdgeschoss (PZH-A-AP-GA-EG-20)
plan-og.jpg               — Grundriss Obergeschoss (PZH-A-AP-GA-OG-21)
supabase.js               — Supabase-Client, lokal eingebunden (kein CDN)
jspdf.umd.min.js          — PDF-Erzeugung, lokal eingebunden (kein CDN)
```

Die Bibliotheken liegen bewusst lokal im Repo statt über ein externes CDN
geladen zu werden — so funktioniert die Seite auch hinter restriktiven
Firmen-Firewalls/Proxys zuverlässig.

## Backend

Die App nutzt das bestehende Supabase-Projekt **„Protokoll"** der
Organisation *INNOTECH Leipzig GmbH* (Tabelle `wandfreigabe_status`,
Realtime aktiviert). Drei Zeilen darin:

- `eg` / `og` — Freigabe-Status je Wand für das jeweilige Geschoss
- `companies` — die aktuelle Firmenliste

Der im Code hinterlegte Schlüssel ist ein *publishable key* — er erlaubt
nur Lesen/Schreiben in dieser einen Tabelle, keine Admin-Rechte.

## Wandgeometrie aktualisieren oder neues Geschoss ergänzen

Die Wandkoordinaten stehen als `WALL_GEOMETRY`-Objekt fest im `<script>`
von `index.html` (aus dem DXF extrahiert, auf Bildkoordinaten umgerechnet).
Für ein zusätzliches Geschoss oder eine aktualisierte Planversion müssten
die Wandlinien aus einer neuen DXF-Datei erneut extrahiert und die
Koordinaten in `WALL_GEOMETRY` ersetzt werden — das ist kein Vorgang, der
sich direkt im Browser erledigen lässt.
