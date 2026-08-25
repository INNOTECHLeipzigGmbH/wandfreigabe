# Wandfreigabe — AWO Psychiatriezentrum Halle, Haus 2

Statische Web-App zur Freigabe von Trockenbau-Wänden pro Firma, direkt im
Grundriss (EG/OG). Der Status wird live über Supabase synchronisiert — alle
Beteiligten sehen denselben Stand in Echtzeit, ganz ohne eigenes Backend.

## Nutzung

1. Repo als GitHub Pages veröffentlichen (Settings → Pages → Branch `main`,
   Ordner `/`), oder `index.html` einfach lokal im Browser öffnen.
2. Firma oben auswählen ("LKO ELECTRO GmbH", "Riek & Keßler GmbH" oder
   "SG Weber").
3. Grundriss anklicken → mit „+ Position setzen" eine Wand/Öffnung anlegen.
4. Jede Firma gibt nur ihre eigene Position frei; Status-Farbe zeigt den
   Gesamtstand (rot = offen, gelb = teilweise, grün = komplett freigegeben).
5. Reiter „Protokoll" zeigt den kompletten Verlauf, „Protokoll kopieren"
   legt ihn in die Zwischenablage.

## Struktur

```
index.html          — die App (keine Build-Tools nötig)
assets/plan-eg.jpg   — Grundriss Erdgeschoss (546.01-5-G-EG-SD-0100-e-F)
assets/plan-og.jpg   — Grundriss Obergeschoss (546.01-5-G-OG-SD-0100-g-F)
```

## Backend

Die App nutzt das bestehende Supabase-Projekt **„Protokoll"** der
Organisation *INNOTECH Leipzig GmbH* (Tabelle `wandfreigabe_status`,
Realtime aktiviert). Der im Code hinterlegte Schlüssel ist ein
*publishable key* — er erlaubt nur Lesen/Schreiben in dieser einen Tabelle,
keine Admin-Rechte. Für Details siehe Supabase-Dashboard des Projekts.

## Neuen Grundriss ergänzen (z. B. weiteres Geschoss)

1. Plan als JPG/PNG unter `assets/` ablegen.
2. In `index.html` im Objekt `FLOORS` einen neuen Eintrag hinzufügen
   (Bildpfad + Originalgröße in Pixel).
3. Passenden Tab-Button in der `<div class="tabs">` ergänzen.
