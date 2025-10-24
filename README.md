# Opgaveskabelon til Frontend Design tema på Frontend-valgfaget

Se opgavebeskrivelsen på Fronter.

## Refleksion over løsningen

### Introduktion

Dette projekt demonstrerer en moderne webløsning bygget med Astro, hvor fokus har været på semantisk HTML, vedligeholdelsesvenlig CSS og responsive layouts. Opgaven har givet mulighed for at arbejde med avancerede CSS-teknikker og komponentbaseret udvikling.

### Udfordringer og succeser

#### Succeser

**CSS Custom Properties til temaer:** Brugen af custom properties til at styre farver og størrelser på tværs af komponenter har været særdeles effektiv:

```css
[data-surface="primary"] {
  --core-text: var(--text-light);
  --core-bg: var(--primary-color03);
  --core-btn-bg: var(--secondary-color02);
  --core-btn-text: var(--text-dark);
}
```

Dette tillader samme komponent at have forskellige udtryk afhængigt af kontekst, som set i `CoreValues.astro`.

**Avanceret Grid med Subgrid:** Implementeringen af full-bleed layouts med subgrid i `FinancialProjections.astro`:

```css
.financial-projections {
  grid-column: full;
  display: grid;
  grid-template-columns: subgrid;
}

ul {
  grid-column: full;
  padding-inline: max(1rem, 50% - 1200px / 2) !important;
}
```

Dette giver mulighed for at bryde ud af container-bredden, mens indholdet stadig er alignet korrekt.

#### Udfordringer

**Scroll-drevne animationer:** Implementeringen af CSS scroll-timeline animationer i `Experience.astro` og `OurVision.astro` var kompleks, men effektiv:

```css
@keyframes progress {
  from {
    --p: 0%;
    --num: 0;
  }
  to {
    --p: var(--target-p);
    --num: var(--target-num);
  }
}

.stat {
  animation: progress auto ease-out both;
  animation-timeline: view();
  animation-range: entry 25% cover 45%;
}
```

Udfordringen var at få `@property` til at virke sammen med `counter()` for at animere tal progressivt.

**Responsivt layout med overlappende elementer:** Hero-sektionen med baggrundsmønstre og overlappende elementer krævede meget grid-planlægning:

```css
.hero-inner {
  display: grid;
  grid-template-columns:
    1fr minmax(0, calc(600px - 1rem)) minmax(0, calc(600px - 1rem))
    1fr;
}
```

At sikre det fungerede på både desktop og mobil med media queries tog sin tid.

### CSS-organisering

#### Global CSS

I `global.css` findes:

- Custom properties til farver, typografi og genbrugelige værdier
- Globale utility-klasser som `.container`, `.button01`, `.button02`
- Genbrugelige patterns som `.bg-pattern`, `.steps`, `.title`

```css
.container {
  background: var(--container-background, --secondary-color02);
  padding-block: 4rem;
  display: grid;
  grid-template-columns:
    [full-start] 1fr [content] minmax(0, 1200px)
    1fr [full-end];
  column-gap: 2rem;
}
```

Denne container-klasse bruges konsekvent gennem hele siden og definerer named grid lines, hvilket gør det let at arbejde med full-bleed layouts.

#### Komponent-specifik CSS

Hver Astro-komponent har sin egen `<style>`-sektion med:

- Scoped styles der kun gælder for komponenten
- Specifik layout-logik til komponentens struktur
- Media queries relevant for komponenten

Eksempel fra `CoreValues.astro`:

```css
[data-surface="primary"] {
  --core-text: var(--text-light);
  --core-bg: var(--primary-color03);
}

ul {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
}
```

#### Layer-strategi

Projektet bruger CSS layers til at kontrollere specificity:

```css
@layer reset, global, components, overrides;
```

Dette sikrer at:

- Reset-styles ligger nederst
- Globale styles kan override reset
- Komponent-styles har højere prioritet
- Overrides kan tvinge ændringer når nødvendigt

### Fremhævede teknikker

#### 1. Semantisk HTML

Gennem hele projektet bruges korrekte semantiske tags:

```html
<article class="container">
  <header>
    <figure>...</figure>
    <dl>
      <div>
        <dt>Case Name</dt>
        <dd>{caseName}</dd>
      </div>
    </dl>
  </header>
</article>
```

Brug af `<dl>`, `<dt>` og `<dd>` til key-value pairs forbedrer tilgængeligheden markant.

#### 2. Container Queries (implicit gennem Grid)

Selvom ikke eksplicit brugt, opnås responsive behaviour gennem bl.a.:

```css
grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
```

Dette pattern tillader kolonnerne at tilpasse sig automatisk baseret på tilgængelig plads.

#### 3. Moderne CSS funktioner

- **:has()**: Bruges implicit gennem moderne selectors
- **Logical properties**: `padding-inline`, `padding-block`
- **@property**: Til animerbare custom properties
- **conic-gradient**: Til cirkulære progress indicators

#### 4. View Transitions

```css
@view-transition {
  navigation: auto;
}
```

Dette giver smoothe transitions mellem sider i Astro.

### Konklusion

Projektet demonstrerer en moderne tilgang til webdevelopment med fokus på:

- Komponentbaseret arkitektur gennem Astro
- Skalerbar CSS med custom properties og layers
- Tilgængelighed gennem semantisk HTML
- Performance gennem optimerede layouts og animationer
- Vedligeholdelse gennem konsistent struktur og navngivning

De største læringspunkter har været arbejdet med avancerede Grid-layouts, scroll-drevne animationer og opbygningen af et skalérbart designsystem med CSS custom properties.
