# MKS Mission Title Updater

> Mission title updater voor [Meldkamerspel](https://www.meldkamerspel.com) — vervang missie-captions door je eigen namen, overal in de interface.

Een Tampermonkey-userscript dat missie-captions vervangt door zelfgekozen namen (gedefinieerd in `missionNames`) op drie plekken:

- 📋 **Sidebar** — de missielijst in de zijbalk
- 📄 **Missiepagina** — `/missions/<id>`
- 🗺️ **Leaflet-kaart** — tooltips en labels, inclusief gedeelde meldingen

---

## 📦 Installatie

1. **Installeer Tampermonkey** via de extensiepagina van je browser.
   > ⚠️ **Chrome:** geef Tampermonkey extra permissies via
   > `Extensies → Mijn extensies → Tampermonkey → Details → Gebruikersscripts toestaan`.
   > Mogelijk moet je Chrome hierna herstarten.
2. **Maak een nieuw Tampermonkey-script** aan en plak de inhoud van `user.js` erin.
3. **Kopieer de inhoud van `missions.json` (of gebruik degene die er in staan)** naar het script, bovenin na `const missionNames =`, ter vervanging van de voorbeeldnamen.
4. **Pas de missienamen aan** naar eigen smaak. Klaar!

---

## ⚙️ Hoe het werkt

Het script houdt de DOM in de gaten met een gedeelde MutationObserver-aanpak:

- **Naam-cache per missie-id** — tooltips blijven werken, ook als de sidebar-entry tijdelijk verdwijnt.
- **Stabiele sidebar-observer** — overleeft het herbouwen van lijsten (bijv. bij het delen van meldingen) en pikt ook `characterData`-mutaties op.
- **Pane-observer** — zichtbare kaartlabels worden direct in de DOM herschreven, onafhankelijk van `tooltipopen`/`layeradd`-events.
- **Reverse-map fallback** — gedeelde meldingen bevatten alleen de kale caption; via een reverse-lookup (originele caption → nieuwe naam) worden ook deze correct hernoemd.
- **Idempotente updates** — replace-functies veroorzaken geen mutatie-loops; updates zijn gedebounced via `requestAnimationFrame`.

---

## 📜 Versiegeschiedenis

| Versie | Wijzigingen |
|:------:|-------------|
| **v7** | Opschoning: changelog samengevat, null-guard in `missionUpdate`, consistente stijl, comments bij bewuste redundantie. |
| **v6** | Ondersteuning voor gedeelde meldingen: reverse-map (originele caption → nieuwe naam) als fallback-lookup, omdat hun tooltips geen `mission_address_`-element bevatten. |
| **v5** | Pane-observer toegevoegd: kaartlabels worden direct in de DOM herschreven, los van tooltip-events. |
| **v4** | Sidebar-observer aan één stabiele root (overleeft herbouwen van lijsten); `characterData`-mutaties opgepikt; idempotente replace-functies; debouncing via `requestAnimationFrame`. |
| **v3** | Naam-cache per missie-id; TreeWalker-fallback voor geneste captions. |
| **v2** | Tooltip-update bij `tooltipopen` i.p.v. eenmalig `layeradd`; operator-precedence bug en naamconflict (`missionIds`/`missionNames`) gefixt; guards tegen ontbrekende elementen; sidebar-observer met `subtree`. |

---

## 📄 Bestanden

| Bestand | Beschrijving |
|---------|--------------|
| `user.js` | Het Tampermonkey-script zelf |
| `missions.json` | Mapping van missie-id's naar eigen namen |
