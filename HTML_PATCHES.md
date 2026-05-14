# HTML patches for visual hierarchy v2

These patches accompany the v2 changes to `input.css`. Apply them to `index.html`
to wire up the new typography (drop caps, pull quotes, section watermarks,
sprint-block differentiation, banner chip). Each patch is independent.

Drop caps use `::first-letter` (pseudo-element, no markup change required) — see
the note on patch 4. The only required HTML changes are the additions of
`section-number` watermarks, `has-dropcap` class hooks, pull-quote `<blockquote>`
elements, sprint-block modifier classes, and the banner-chip span split for the
rust-coloured numerics.

---

## Patch 1 — §01 The work: add section-number watermark, drop-cap hook, pull quote

Find:
```html
    <section class="section" id="work">
      <p class="section-label">§ 01 &nbsp;&middot;&nbsp; The work</p>
      <h2>The work</h2>
      <div class="prose">
        <p>Most robots in SMB manufacturing are programmed once and run one product. They handle pose variation badly, choke on part-family changes, and lose hours on changeover. The hardware is fine — what's missing is a software layer that lets a workcell adapt.</p>
        <p>Asperity builds that layer. We use existing robot arms (<span class="mono-inline">Universal Robots</span>, <span class="mono-inline">FANUC</span>, <span class="mono-inline">ABB</span>), pair them with cameras, force/torque, and tactile sensing where it matters, and run perception, anomaly detection, task adaptation, and recovery on top. The result is a cell that handles variation instead of breaking on it.</p>
        <p>Year-1 focus: East Texas fabrication shops within driving distance of Tyler, and specialty electronics contract manufacturers nationally. Two reasons. The work in these places is high-mix and existing automation handles it badly. And the operators will tell you the truth about what's breaking.</p>
      </div>
    </section>
```

Replace with:
```html
    <section class="section" id="work">
      <span class="section-number" aria-hidden="true">01</span>
      <p class="section-label">§ 01 &nbsp;&middot;&nbsp; The work</p>
      <h2>The work</h2>
      <div class="prose">
        <p class="has-dropcap">Most robots in SMB manufacturing are programmed once and run one product. They handle pose variation badly, choke on part-family changes, and lose hours on changeover. The hardware is fine — what's missing is a software layer that lets a workcell adapt.</p>
        <p>Asperity builds that layer. We use existing robot arms (<span class="mono-inline">Universal Robots</span>, <span class="mono-inline">FANUC</span>, <span class="mono-inline">ABB</span>), pair them with cameras, force/torque, and tactile sensing where it matters, and run perception, anomaly detection, task adaptation, and recovery on top. The result is a cell that handles variation instead of breaking on it.</p>
        <blockquote class="pull-quote">Operators will tell you the truth about what's breaking.</blockquote>
        <p>Year-1 focus: East Texas fabrication shops within driving distance of Tyler, and specialty electronics contract manufacturers nationally. Two reasons. The work in these places is high-mix and existing automation handles it badly.</p>
      </div>
    </section>
```

Why: adds the faint "01" watermark for visual rhythm, applies a drop cap to the
opening paragraph, lifts the operators line into a serif-italic pull quote with
rust left-rule, and trims the redundant repeat of that line from the third
paragraph so the pull quote stands as the punchline.

---

## Patch 2 — §02 The Sprint: watermark, banner chip, drop-cap, pull quote, sprint-block modifiers

Find:
```html
    <section class="section" id="sprint">
      <p class="section-label">§ 02 &nbsp;&middot;&nbsp; The Sprint</p>
      <h2>The AI Robotics Feasibility Sprint</h2>
      <p class="chip"><span class="mono">6 weeks &nbsp;&middot;&nbsp; $25K&ndash;$80K</span></p>

      <div class="prose">
        <p class="lede">Most automation feasibility studies are slide decks. The Sprint is a working cell.</p>
        <p>We bring or configure a leased robot at your facility (or at our lab with your parts), instrument it for data collection, build a baseline automation, then add the AI layer that handles the variation your operators see every day. At week 6, you have measured before/after numbers and a production-pilot proposal grounded in what we observed — not vendor optimism.</p>
      </div>

      <div class="sprint-grid">
        <div class="sprint-block">
          <p class="mono-label">What you get</p>
          <ul>
            <li>A working robotic cell on one of your real tasks</li>
            <li>Cycle time, quality, and failure-mode measurements (before vs. after)</li>
            <li>A production-pilot proposal with calibrated numbers</li>
            <li>The data we collect, in a format you own</li>
          </ul>
        </div>

        <div class="sprint-block">
          <p class="mono-label">What's out of scope</p>
          <p>Mechanical fixturing, end-of-arm tooling fabrication, PLC integration, safety engineering. Those are delegated to regional integration partners. We stay in the AI/software lane.</p>
        </div>

        <div class="sprint-block sprint-block-wide">
          <p class="mono-label">Design-partner pricing</p>
          <p>First five customers get a discount in exchange for data-sharing rights and a case-study agreement.</p>
        </div>
      </div>
    </section>
```

Replace with:
```html
    <section class="section" id="sprint">
      <span class="section-number" aria-hidden="true">02</span>
      <p class="section-label">§ 02 &nbsp;&middot;&nbsp; The Sprint</p>
      <h2>The AI Robotics Feasibility Sprint</h2>
      <p class="chip"><span class="mono"><span class="chip-number">6 weeks</span> &nbsp;&middot;&nbsp; <span class="chip-number">$25K&ndash;$80K</span></span></p>

      <div class="prose">
        <p class="lede">Most automation feasibility studies are slide decks. The Sprint is a working cell.</p>
        <p class="has-dropcap">We bring or configure a leased robot at your facility (or at our lab with your parts), instrument it for data collection, build a baseline automation, then add the AI layer that handles the variation your operators see every day.</p>
        <blockquote class="pull-quote">Measured before/after numbers and a production-pilot proposal grounded in what we observed — not vendor optimism.</blockquote>
      </div>

      <div class="sprint-grid">
        <div class="sprint-block sprint-block--value">
          <p class="mono-label">What you get</p>
          <ul>
            <li>A working robotic cell on one of your real tasks</li>
            <li>Cycle time, quality, and failure-mode measurements (before vs. after)</li>
            <li>A production-pilot proposal with calibrated numbers</li>
            <li>The data we collect, in a format you own</li>
          </ul>
        </div>

        <div class="sprint-block sprint-block--boundary">
          <p class="mono-label">What's out of scope</p>
          <p>Mechanical fixturing, end-of-arm tooling fabrication, PLC integration, safety engineering. Those are delegated to regional integration partners. We stay in the AI/software lane.</p>
        </div>

        <div class="sprint-block sprint-block-wide sprint-block--pricing">
          <p class="mono-label">Design-partner pricing</p>
          <p>First five customers get a discount in exchange for data-sharing rights and a case-study agreement.</p>
        </div>
      </div>
    </section>
```

Why:
- `section-number` watermark for §02.
- `<span class="chip-number">` wraps the numerics so the new banner chip (full-width, ~1.75rem mono, double-rule top/bottom) renders the time + price in rust while keeping the middot punctuation in ink.
- `has-dropcap` on the second paragraph (the first regular prose paragraph after the lede — the lede stays as a lede, which is what gives it its own emphasis treatment).
- The closing line of the original second paragraph becomes the pull quote; the slimmed paragraph stops at "every day."
- Sprint blocks get modifier classes for left-edge differentiation: rust rule for value, muted rule for boundary, rust rule + emphasised label for pricing.

---

## Patch 3 — §03 Founder: watermark + drop cap

Find:
```html
    <section class="section" id="founder">
      <p class="section-label">§ 03 &nbsp;&middot;&nbsp; Founder</p>
      <h2>Founder</h2>

      <div class="founder-grid">
        <div class="founder-photo">
          <img src="satwant.jpg" alt="Portrait of Satwant Kumar, founder of Asperity Industries" width="280" height="280" loading="lazy">
        </div>
        <div class="founder-bio prose">
          <p><strong>Satwant Kumar</strong> — founder.</p>
          <p>Physician-scientist (MBBS, PhD systems neuroscience), with prior research in machine perception and clinical AI published in top journals. Previously founded <strong>Neuroreef Labs</strong>, an Antler-backed healthcare AI startup, where he scaled the company from zero and shipped <strong>CareCortex</strong> — a state-of-the-art clinical operations platform for healthcare providers.</p>
```

Replace with:
```html
    <section class="section" id="founder">
      <span class="section-number" aria-hidden="true">03</span>
      <p class="section-label">§ 03 &nbsp;&middot;&nbsp; Founder</p>
      <h2>Founder</h2>

      <div class="founder-grid">
        <div class="founder-photo">
          <img src="satwant.jpg" alt="Portrait of Satwant Kumar, founder of Asperity Industries" width="280" height="280" loading="lazy">
        </div>
        <div class="founder-bio prose">
          <p><strong>Satwant Kumar</strong> — founder.</p>
          <p class="has-dropcap">Physician-scientist (MBBS, PhD systems neuroscience), with prior research in machine perception and clinical AI published in top journals. Previously founded <strong>Neuroreef Labs</strong>, an Antler-backed healthcare AI startup, where he scaled the company from zero and shipped <strong>CareCortex</strong> — a state-of-the-art clinical operations platform for healthcare providers.</p>
```

Why: watermark for §03. The first paragraph (`<strong>Satwant Kumar</strong> — founder.`) is a one-line tag, so the drop cap goes on the second paragraph where it has room to wrap two lines. Note: `::first-letter` won't apply to a leading `<strong>` element, but on this paragraph the first character is plain "P", which is what we want.

---

## Patch 4 — §04 Reach us: watermark + drop cap

Find:
```html
    <section class="section" id="reach">
      <p class="section-label">§ 04 &nbsp;&middot;&nbsp; Reach us</p>
      <h2>Reach us</h2>
      <p class="lede">If you run a shop or a line and you've got a task that's eating hours, write. We respond.</p>
```

Replace with:
```html
    <section class="section" id="reach">
      <span class="section-number" aria-hidden="true">04</span>
      <p class="section-label">§ 04 &nbsp;&middot;&nbsp; Reach us</p>
      <h2>Reach us</h2>
      <p class="lede has-dropcap">If you run a shop or a line and you've got a task that's eating hours, write. We respond.</p>
```

Why: watermark for §04. §04 is short — the lede *is* the first paragraph, so the drop cap goes on it directly. (`has-dropcap` works fine on a `.lede`; the `::first-letter` rule's larger size overrides the lede's font-size for that glyph only.)

---

## Notes on the drop-cap approach

I went with `::first-letter` (no `<span class="dropcap">` wrapper) because:

1. The instruction was to use the cleaner option; `::first-letter` is one rule and survives content edits.
2. Cross-browser support for `::first-letter` on `float: left; font-size; line-height; padding` is universal in modern engines (it's a 20-year-old spec; quirks are limited to legacy IE).
3. The only HTML change is adding `has-dropcap` to the paragraphs that should get the treatment — a class hook, not a wrapper span.

If for any reason a section needs the drop cap suppressed (e.g., an opening paragraph starts with a numeric or a quote that doesn't render well as a drop), simply remove `has-dropcap` from that `<p>`.

---

## Summary of class names introduced

| Class | Purpose |
|---|---|
| `.section-number` | Faint oversized "01"/"02"/"03"/"04" watermark, top-right of each numbered section. `aria-hidden="true"`. |
| `.has-dropcap` | Hook on the first prose paragraph of each numbered section; activates `::first-letter` drop cap. |
| `.pull-quote` | `<blockquote>` element with rust left-rule, serif italic, ~1.4–1.55rem. |
| `.chip-number` | `<span>` inside the chip banner, paints "6 weeks" and "$25K–$80K" in rust. |
| `.sprint-block--value` | Rust left-rule on "What you get". |
| `.sprint-block--boundary` | Muted left-rule on "What's out of scope". |
| `.sprint-block--pricing` | Rust left-rule + emphasised label on "Design-partner pricing". |

No existing classes were renamed or removed.
