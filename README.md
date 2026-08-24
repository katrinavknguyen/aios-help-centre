# AIOS Help Centre prototype - handoff

Single file, `index.html`. No build step, no dependencies, no external requests. Open it directly or drop it on any static host.

This is a content and structure prototype, not production code. It exists to settle what the Help Centre contains and how someone moves through it, so the decisions are made before anyone builds it properly.

---

## What it contains

**Two tabs.** Documentation and Support.

**46 live articles.** All written from the AIOS capability guide (`AIOS_What_Your_AI_Workforce_Can_Do_v15_1.pdf`, v1 August 2026). Every one opens and reads properly.

**6 dead links**, deliberately. Listed under Open questions below.

**A simulated Assistant.** Slides in from the right. Replies are canned keyword matches, clearly labelled as a prototype inside the panel.

---

## How it is put together

```
index.html
├── <style>          all CSS, brand tokens as CSS variables at the top
├── <div id="view">  the only container that changes
├── #bubble/#panel   the Assistant, sits outside #view so it survives navigation
└── <script>
    ├── var V = {...}   every screen and article as an HTML string
    ├── show(key)       swaps innerHTML of #view
    └── one delegated click handler on document
```

Navigation is a single event listener reading `data-go` and `data-art` attributes. No router, no hash URLs, no framework. Deliberately dumb so it is easy to throw away.

### Adding or editing an article

Articles are keys in `V`. Each is an HTML string following the same shape:

```html
<a class="crumb" data-go="doc">All help articles</a>
<div class="article">
  <h1>Title</h1>
  <div class="lede">The answer, before any explanation</div>
  <p>Body...</p>
  <div class="callout"><b>Label.</b> Aside worth knowing.</div>
  <div class="related">...</div>
  <div class="artfoot">Did this answer your question?</div>
</div>
```

To make a card link live, give it `class="on" data-art="key"` and add `<span class="dotl"></span>` before the text.

---

## Design decisions worth preserving

**Command is removed from the Help page.** Two chat entry points on one screen was the main problem with the current version. The Assistant is the single conversational route here. Command still exists everywhere else in AIOS and has its own card in the article grid.

**The hero box and the bubble are one conversation.** Type in the hero, the panel opens with your message already sent. Close it, navigate, reopen, and the history is still there. This is the behaviour to specify to engineering, not a prototype convenience.

**The marketing block is deleted, not moved.** This is a signed-in surface. The reader is already a customer.

**Categories are ordered by need, not by the left-hand menu.** Menu order assumes the reader knows which part of the product their problem lives in, which is the thing a stuck person does not know.

**Article titles are symptoms, not topics.** "My credit usage is higher than last month", not "Understanding credit consumption". The first is what gets typed into a search box.

**The answer comes before the explanation.** Every article opens with a bolded lede in a purple panel. Someone can read one line and leave.

**Teal dots mark live articles.** Prototype scaffolding. Delete the dots, the `.dotl` class and the legend before anyone external sees it.

---

## Brand

Tokens are CSS variables at the top of the file, taken from the AIOS in-product spec rather than the marketing one. Purple `#6B30FF`, navy `#2A2092`, borders `#E8E6F2`, cards at 30px radius, pill buttons, Gotham with Montserrat fallback.

The gradient capsule is cover-only, at the very top of the page. It is not a full-section background.

Icons are inline Lucide paths. No emoji, no icon font, nothing fetched.

Copy follows the Implement AI guide. British English, hyphens only, no em or en dashes anywhere.

---

## Known rough edges

Search under the hero is decorative. It does nothing. Whether it should be real depends on whether the Assistant answers well enough to replace it.

Article counts on the "View all" links are invented and need correcting against the real set.

The Support tab shows an empty ticket state only. There is no populated version.

Support hours are a placeholder: Monday to Friday, 9am to 6pm UK.

Layout is desktop-first. It reflows at 1100px but has not been designed for mobile.

---

## Open questions

**For Onkar**

Can the Assistant be embedded as an inline input, or does it only open from the floating bubble? This decides whether the hero is a real input or a button.

Can it be opened programmatically with a message prefilled?

What does raising a ticket actually look like to the user? Silent creation with a confirmation, a summary to approve, or a form? Which Zendesk fields are mandatory?

Does the ticket reference come back into the chat, and do tickets appear on the Support tab with status and replies? Three articles are blocked on this.

**For Aalok**

The credit bands article reproduces the guide's figures in full. Acceptable in a controlled PDF, closer to a rate card on a linkable help page. Needs a decision.

Two lines are mine rather than the guide's and should be checked: the warning in the Support Team article that step one is the one people skip, and the note in the Task Team article about reading every step's output.

**Content gaps**

WhatsApp setup. The guide says talk to us and gives no steps.

What each role can see and do. The guide gives roles two sentences. The parent article is written and flags the gap in place.

Account settings. One line in the guide.

Where the language setting lives, referenced in "My worker only answers in English".

---

## Versioning

The articles are currently derived from the capability guide PDF. If both are maintained they will drift within a month. Worth deciding early that the Help Centre is the source and the PDF is generated from it, rather than the other way round.
