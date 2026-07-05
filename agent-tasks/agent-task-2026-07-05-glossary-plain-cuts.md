---
created: 2026-07-05
type: agent-task
status: ready
model: opus
---

**Goal**

Give every entry in `framework/glossary.md` (59 entries) a plain cut: one sentence,
containing no other glossary term, appended as a `_Plain:_` line after the entry's
definition. This is the framework-side work that gates hosystem v1's
definition-on-touch.

**Context**

hosystem — the Ho System's public site — marks every term of art in its prose;
touching a marked term shows its plain cut inline, with the full glossary entry one
link away. The frozen rule (hosystem basis of design §7): the plain cut is **one
sentence with no terms of art inside it**. The site ingests `framework/glossary.md`
at build time; today the glossary has no plain cuts, so the site cannot ship them.

Storage convention (decided at authoring; the practitioner has seen it): one italic
line immediately after each entry's final definition paragraph, before the next
entry:

```
_Plain: One sentence, ending with a period._
```

It renders quietly in the document and parses unambiguously (a paragraph starting
`_Plain:`). A worked reference exists: the hosystem spike drafted ~15 stand-in plain
cuts condensed from real entries — read them to calibrate register before writing.

Operating guardrails (binding): the spec is the authorization — do exactly what it
names, nothing adjacent; verify by command, not by impression; on surprise, halt
and surface rather than adapt.

**Files**

- Modify: `framework/glossary.md` (all 59 entries + one preamble line)
- Read-only: `/Users/atmarcus/Vaults/sageframe-dharma/hosystem/design/claude-design/spike/lib/plain-cuts.mjs` (register reference — spike stand-ins condensed from real definitions)

**Required Changes**

1. **Add a `_Plain:_` line to every entry.** Entries are the paragraphs beginning
   `**term** —`. After each entry's final paragraph (some entries run more than one
   paragraph; the `_Plain:_` line comes after the last, before the next `**term**`),
   add exactly one line: `_Plain: <one sentence.>_`

   Writing rules for the sentence:
   - One sentence, ending in a period. Simple, clear, terse — the practitioner's
     register, matching the glossary's existing voice.
   - No other glossary term inside it — not the term's surface form, not obvious
     inflections (e.g., a plain cut may not use "ho", "closure", "superseded",
     "tuner", "arc"…). The entry's own term may appear.
   - Derived from the entry's existing definition. The plain cut compresses; it
     never introduces doctrine the entry doesn't state.
   - For entries defining two senses, cut to the shared core; if no one sentence
     can hold both senses honestly, that entry goes to the Stop Condition list.

2. **Document the convention in the preamble.** After the existing preamble
   paragraph (before the first `---` separator's following entry block), add one
   sentence: `Each entry ends with a _Plain:_ line — the one-sentence cut shown on
   touch wherever the term is marked; plain cuts contain no terms of art.`

**Acceptance**

- [ ] Every `**term** —` entry (59) is followed by exactly one `_Plain:` line
- [ ] Every plain cut is one sentence ending in a period
- [ ] No plain cut contains any other glossary term (word-boundary,
      case-insensitive; own term exempt)
- [ ] The preamble documents the convention
- [ ] `framework/glossary.md` is the only modified file; its frontmatter is unchanged

**Verification**

```bash
# Entry count vs plain-cut count (must both be 59)
grep -c '^\*\*' framework/glossary.md
grep -c '^_Plain:' framework/glossary.md

# Structural + cross-term check
python3 - <<'EOF'
import re, sys
text = open('framework/glossary.md').read()
entries = re.findall(r'^\*\*(.+?)\*\*\s+—', text, re.M)
plains  = re.findall(r'^_Plain: (.+?)_\s*$', text, re.M)
assert len(entries) == len(plains) == 59, (len(entries), len(plains))
# one sentence: exactly one terminal period, no internal sentence break
bad = [p for p in plains if not p.endswith('.') or re.search(r'[.!?] [A-Z]', p)]
if bad: print('NOT ONE SENTENCE:', *bad, sep='\n  ')
# no other glossary term inside any plain cut
terms = set()
for e in entries:
    for t in re.split(r'\s*/\s*|\s*\(', e)[:2]:
        t = t.strip().strip(')')
        if t: terms.add(t.lower())
viol = []
for (e, p) in zip(entries, plains):
    own = e.lower()
    for t in terms:
        if t in own or len(t) < 3: continue
        if re.search(r'\b' + re.escape(t) + r'\b', p, re.I):
            viol.append((e, t))
if viol: print('TERM-OF-ART VIOLATIONS:', *viol, sep='\n  ')
sys.exit(1 if (bad or viol) else 0)
EOF

# Only glossary.md changed; frontmatter untouched
git diff --stat
git diff framework/glossary.md | head -20
```

The cross-term scan is a screen, not the standard: review its hits — a flagged
substring inside an unrelated word is fine to waive with a note; a real term of art
is not.

**Do Not**

- Do not rewrite, trim, or "improve" any existing definition — this task adds
  `_Plain:_` lines and one preamble sentence, nothing else.
- Do not touch the hosystem repo; its ingestion of this field is separate work.
- Do not add, rename, merge, or reorder entries.

**Stop Condition**

If any term cannot be cut to one honest sentence without leaning on another term of
art, do not force it: finish the rest, list the resistant terms with a note on what
each needs, and surface the list. Naming decisions belong to the practitioner. Halt
likewise if the entry pattern misses part of the file (entries not matching
`**term** —`).

**Commit**

Branch `glossary-plain-cuts`, single commit, no push. Message:

```
Glossary: add plain cuts — one sentence, no terms of art, per entry

All 59 entries gain _Plain:_ lines for hosystem definition-on-touch
ingestion; convention documented in the preamble.
```
