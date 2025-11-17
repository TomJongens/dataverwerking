# Data Detective 🕵️‍♀️

Interactieve leerervaring voor MBO niveau 4-studenten rond het vak `Dataverwerking` van Da Vinci College. Deze v2.0 is een browser-based spel waarmee studenten spelenderwijs kennismaken met de volledige leerlijn *Dataverwerking* (`source/Leerlijn dataverwerking.pdf`) en voorbereiden op de praktijkopdracht/examenmoment in periode 2 (cohort 2025-2026).

## Inhoud en leerdoelen
- **Kerntaak / werkproces**: `B1-K1 W3` (gegevens verwerken binnen administratieve processen).
- **Competenties**: analyseren, informatie verwerken, communiceren, verantwoord handelen en kwaliteitsgericht werken.
- **Leerdoelen** *(samenvatting uit de leerlijn)*:
  - Begrijpen wat data/dataverwerking is en hoe stam- & transactiedata samenhangen.
  - Uitleggen waarom datakwaliteit en AVG belangrijk zijn en hoe CRM-systemen werken.
  - Fouten herkennen en verbeteren, data naar rapportages vertalen en eenvoudige rapportages maken.
  - Integer handelen en privacybewustzijn toepassen in praktijksituaties.

Het spel vertaalt deze doelen naar **drie weken met elk vier levels (60 vragen totaal)**:

### Week 1: Basisprincipes Dataverwerking
- **Level 1**: Wat is data? (DIKW-piramide: Data, Informatie, Kennis)
- **Level 2**: Gestructureerde en ongestructureerde data, stamgegevens en transactiedata
- **Level 3**: AVG en privacy principes (dataminimalisatie, doelbinding, beveiliging)
- **Level 4**: Integer en ethisch handelen met data

### Week 2: Administratieve Documenten & Financiën
- **Level 1**: Administratieve documenten (order, orderbevestiging, pakbon, factuur)
- **Level 2**: Datastroom van order tot factuur en documentrelaties
- **Level 3**: Financiële dataverwerking (BTW-berekeningen, debiteuren, crediteuren)
- **Level 4**: Financiële rapportages en relatie stamdata/transactiedata

### Week 3: Rapportages & Data-analyse
- **Level 1**: Soorten rapportages (verkoop, voorraad, personeel, debiteuren)
- **Level 2**: Rapportages maken in Excel (SOM-functie, filters, groeperen)
- **Level 3**: Draaitabellen en grafieken (pivot tables, grafiektypen kiezen)
- **Level 4**: Rapportages analyseren en interpreteren (trends identificeren, beslissingen nemen)

## Projectstructuur

```
.
├── index.html            # Complete React-app (via CDN) + styling + gameData
├── da-vinci-college.webp # Logo in de UI
└── source/               # Lesmateriaal & leerlijn (PDF's)
    └── Leerlijn dataverwerking.pdf
```

Er is géén build pipeline; alle logica, data en styles staan in `index.html` en draaien direct in de browser.

## Gebruik
- **Docent/demo**: open `index.html` in een moderne browser (Chrome/Edge). Internet is nodig voor de React-CDN's en confetti library.
- **Student**: kan lokaal spelen of via (bijvoorbeeld) VS Code Live Server.
- **Ontwikkelaar**: werk in een editor, gebruik devtools voor console/logs. Omdat Babel stand‑alone wordt gebruikt (`type="text/babel"`), kun je ES6/JSX schrijven zonder bundler.

## Gameflow
1. **Startscherm**: weekselectie (Week 1, 2, en 3 beschikbaar) + dynamische leerdoelen per week.
2. **Levels**: vier levels per week, elk met 5 vragen met diverse vraagtypen:
   - `multiple-choice` - Standaard meerkeuze vragen
   - `matching` en `drag-drop-classification` - Sleep items naar categorieën
   - `scenario` - Praktijksituaties met ethische afwegingen
   - `scenarios-multiple` - Selecteer meerdere juiste antwoorden
3. **Score**: +10 punten per volledig goed beantwoorde vraag, voortgangsbalk en eindscherm met percentage en feedback.
4. **Gamification**:
   - Streaks (3 en 5 op rij goed)
   - Achievements (Eerste Goed!, Perfect Level, Op Dreef, Onverslaanbaar, Voltooier)
   - Confetti effects bij correcte antwoorden
   - Optionele geluidseffecten (Web Audio API)
   - Dark mode toggle
5. **Accessibility**:
   - Toetsenbordnavigatie (1-4 voor antwoorden, Enter voor volgende)
   - ARIA labels en roles
   - Screen reader ondersteuning
   - Focus indicatoren

Alle content komt uit het `gameData`-object in `index.html`.

## Features v2.0

### UI/UX Verbeteringen
- **Enhanced animations**: Confetti, shake effects, smooth transitions, gradient shifts
- **Visual feedback**: Ripple effects, hover transformations, pulse animations, dynamic shadows
- **Accessibility**: Keyboard navigation, ARIA labels, screen reader support, focus indicators
- **Mobile responsive**: Optimized touch targets, better spacing, single-column layouts on small screens
- **Dark mode**: Complete theming system with CSS variables, smooth transitions
- **Gamification**: 5 achievements, streak tracking, level completion badges
- **Sound effects**: Procedural audio generation voor correct/incorrect/clicks/achievements
- **Better typography**: Increased font sizes, improved line height, strategic font weights
- **Progress indicators**: Enhanced progress bar with shimmer animation, detailed fraction display
- **Polish**: Version badge, animated emojis, card hover effects, loading animations

### Content Uitbreiding
- Week 1: 20 vragen (basis dataverwerking, AVG, integer handelen)
- Week 2: 20 vragen (documenten, datastroom, financiële administratie)
- Week 3: 20 vragen (rapportages, Excel, draaitabellen, analyse)
- Totaal: **60 vragen** die de volledige leerlijn dekken

## Aanpassen & uitbreiden

| Stap | Wat te doen | Tips i.v.m. leerlijn |
| --- | --- | --- |
| Nieuwe week toevoegen | Voeg `week4` toe aan `gameData` met 4 levels. Update week selector buttons. | Houd planning uit `Planning BSP W3 25-26.pdf` aan zodat toetsen/voorbereiding synchroon lopen. |
| Nieuw level / thema | Voeg een entry toe aan `gameData.weekX.levelY` met `title` + `questions`. | Gebruik de leerdoelenlijst hierboven als checklist voor dekking. |
| Nieuwe vraagtypen | Breid `question.type`-switch uit in `DataDetectiveGame` en beschrijf inputstructuur in README. | Laat vraagvorm aansluiten op competentie (bv. rapportage → analyse & communicatie). |
| Evaluatie/logging | Voeg state of localStorage toe voor voortgang, of exporteer resultaten voor portfolio-bewijs. | Link resultaten aan examencriteria (praktijkopdracht) en bespreek tijdens begeleiding. |
| Nieuwe achievements | Voeg achievement toe aan `achievements` array en unlock logic in component. | Link achievements aan leerdoelen voor extra motivatie. |

**Best practices**
- Zorg dat iedere levelbeschrijving expliciet verwijst naar een leerdoel/competentie uit de leerlijn.
- Houd `explanations` kort, in begrijpelijke taal, en verbind ze aan praktijkcases (CRM, HR, finance) zoals in de leerlijn beschreven.
- Introduceer AVG/ethiek-situaties met "Wat doe je?" vragen; gebruik de principes uit de leerlijn (doelbinding, dataminimalisatie, integriteit, opslagbeperking).
- Test nieuwe vragen op niveau (niet te makkelijk, niet te moeilijk voor MBO4).
- Zorg dat scenario-vragen realistisch zijn en aansluiten bij echte werkpleksituaties.

## Technische details

### Structuur gameData
```javascript
const gameData = {
    week1: {
        level1: { title: "...", questions: [...] },
        level2: { title: "...", questions: [...] },
        level3: { title: "...", questions: [...] },
        level4: { title: "...", questions: [...] }
    },
    week2: { ... },
    week3: { ... }
};
```

### Vraagtypen
- **multiple-choice**: `{ type, question, options: [], correct: index, explanation }`
- **matching**: `{ type, question, items: [{text, category}], categories: [], explanation }`
- **drag-drop-classification**: Zelfde als matching
- **scenario**: Zelfde als multiple-choice maar met praktijksituatie
- **scenarios-multiple**: `{ type, question, scenarios: [{text, isWrong: bool}], explanation }`

### Achievements
- `first_correct`: Eerste vraag goed
- `perfect_level`: Alle vragen in een level goed
- `streak_3`: 3 antwoorden op rij goed
- `streak_5`: 5 antwoorden op rij goed
- `completionist`: Alle levels voltooid

### Sound System
De Web Audio API wordt gebruikt voor procedurele geluidsgeneratie:
- **Correct**: Stijgende melodie (C5 → E5 → G5)
- **Incorrect**: Dalende tonen (F4 → D4)
- **Click**: Korte blip (800Hz)
- **Level up**: Fanfare (C5 → E5 → G5 → C6)
- **Achievement**: Arpeggio (5 noten)

## Backlog-ideeën
- Timer per vraag of per level (bevordert kwaliteitsgericht werken onder tijdsdruk)
- Adaptieve feedback op basis van fouten (extra hints bij herhaalde fouten)
- Vertaling van scores naar portfolio- of mentorfeedback
- Exportfunctie voor resultaten (PDF/Excel voor portfolio-bewijs)
- Multiplayer modus (klascompetitie)
- Persoonlijke voortgang opslaan (localStorage of database)
- Moeilijkheidsgraden (basis, normaal, expert)
- Certificaat bij 100% score
- Ondersteuning voor toetsen/herkansingen richting examenmomenten (29‑01‑2026 en 18‑03‑2026)

## Versiegeschiedenis
- **v1.0** (Week 1 only): Basis game met 20 vragen, 4 levels, eenvoudige UI
- **v2.0** (Week 1-3 compleet):
  - 60 vragen over alle 3 weken
  - Complete UI/UX overhaul met animations, dark mode, accessibility
  - Gamification: achievements, streaks, confetti, sounds
  - Mobile responsive
  - Keyboard navigation

## Referenties
- `source/Leerlijn dataverwerking.pdf` – kern van deze README en leerdoelen.
- Lesmateriaal Week 1-3 gebruikt voor vraagontwikkeling.

Heb je aanvullingen vanuit docenten of uit de praktijk? Voeg ze toe aan `source/` en verwijs ernaar in de README zodat toekomstige bijdragers weten waar de inhoud vandaan komt.

---

**Gemaakt door Tom Jongens, 17-11-2025**
Voor het vak Dataverwerking - Da Vinci College
