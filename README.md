# MKS-Mission-Title-Updater
Mission Title updater for Meldkamerspel

// ═══════════════════════════════════════════════════════════════════════════
// Missienamen-module (v7)
// Vervangt missie-captions door eigen namen (uit `missionNames`) in:
//   • de sidebar-missielijst
//   • de missiepagina (/missions/<id>)
//   • tooltips/labels op de Leaflet-kaart (incl. gedeelde meldingen)
//
// Versiegeschiedenis (samengevat):
//   v2  Tooltip-update bij 'tooltipopen' i.p.v. eenmalig 'layeradd';
//       operator-precedence bug en naamconflict (missionIds/missionNames)
//       gefixt; guards tegen ontbrekende elementen; sidebar-observer met
//       subtree zodat missie-uitbreidingen worden opgepikt.
//   v3  Naam-cache per missie-id (tooltips werken ook als de sidebar-entry
//       tijdelijk weg is); TreeWalker-fallback voor geneste captions.
//   v4  Sidebar-observer aan één stabiele root (overleeft herbouwen van
//       lijsten, o.a. bij delen); characterData-mutaties worden opgepikt;
//       replace-functies idempotent (geen mutatie-loops); updates
//       gedebounced via requestAnimationFrame.
//   v5  Pane-observer: zichtbare kaartlabels worden direct in de DOM
//       herschreven, onafhankelijk van tooltipopen/layeradd-events.
//   v6  Gedeelde meldingen: hun tooltips bevatten alleen de kale caption
//       (géén mission_address_-element). Reverse-map (originele caption →
//       nieuwe naam) als fallback-lookup.
//   v7  Opschoning: changelog samengevat, null-guard in missionUpdate,
//       consistente stijl, comments bij bewuste redundantie.
// ═══════════════════════════════════════════════════════════════════════════
