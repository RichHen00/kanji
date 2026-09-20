# Lesson Extraction Prompt

Read the source file provided. Extract the lesson content for ONE kanji and produce a minimal HTML page.

## What to extract

For the primary kanji (first one in the source), extract:
- The kanji character
- Its English meaning (the primary one)
- Its hiragana reading (kun'yomi if available, else on'yomi)
- Its romaji pronunciation

## What to drop

Drop ALL of the following:
- Vocabulary lists
- Stroke counts
- Notes, asterisks, secondary readings
- Additional kanji in the same lesson
- "About This Lesson" blocks
- Word lists

## HTML output

Produce a single `<article class="lesson">` matching this exact structure (see `k/out20.html` for the working reference):

```
<header>
  <p class="date">YYYY-MM-DD · [English meaning]</p>
</header>
<section class="lesson-content" aria-label="Lesson content">
  <div class="lesson-heading">
    <h2>[Kanji] <span class="ja" lang="ja">[hiragana]</span></h2>
  </div>
  <p class="lesson-lead"><strong><span class="script-note">[Kanji]</span> [English meaning].</strong> <br> Hiragana [char] <span class="romaji">[romaji]</span> [char] <span class="romaji">[romaji]</span></p>

  <div class="characters">
    <div class="characters-heading"><h2>Stroke order</h2></div>
    <div class="kana-grid">
      <div class="kana">
        <div class="kana-top"><strong>[romaji]</strong></div>
        <div class="kana-body">[animCJK SVG here]</div>
      </div>
    </div>
  </div>
</section>
```

Nothing else. No vocabulary, no lesson-block, no word-list, no notes.

### lesson-lead format

- Line 1: `<strong><span class="script-note">[Kanji]</span> [English meaning].</strong>` — kanji in yellow highlight (script-note), meaning in bold, no quotes around meaning.
- Line 2: `<br> Hiragana [char] <span class="romaji">[romaji]</span> ...` — each hiragana character followed by its romaji in a `<span class="romaji">`.
- CSS for `.romaji`: `color: var(--muted); font-size: 13px;`

## Rules

- One kanji per page (pick the first/primary kanji if the source contains more than one).
- Include only the primary reading (first listed in the source).
- The animCJK SVG must be fetched from `https://raw.githubusercontent.com/parsimonhi/animCJK/master/svgsJa/[decimal-unicode].svg` — adapt its CSS to use `var(--ink)` for stroke color and `#e9e9e9` for ghost fill.
- Use the clone-to-replay click handler (clone SVG on click), not the `playing` class approach.
- Footer: stamp image + date stamp (use current time: `DD-MM-YYYY HH:MM`) + G Fonts link.
