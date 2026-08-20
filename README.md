# CONCEIVE Prompts

<!-- #TODO after first release: add the DOI badge here
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX) -->

**CON**cept and **C**laim **E**xtraction with **I**nterdisciplinary **V**alidation of **E**vidence:
versioned, citable LLM prompts for auditable claim and concept extraction from
social-science and interdisciplinary literature. Case study: social (in)fertility.

Developed under British Academy Talent Development Award TDA23\230021
(*Connecting Disciplines using Large Language Models*).

## The prompts 

| File | Task | Status | Notes |
|---|---|---|---|
| `prompts/claim_extraction_plugin_pilot.txt` | claims | validated | causal claims + model decomposition; used for the pilot output |
| `prompts/concepts_extraction_plugin_pilot.txt` | concepts | validated | decomposes an extracted claim into model-role concepts |
| `prompts/entities_extraction_plugin_pilot.txt` | entities | validated | structured methods entities against a controlled vocabulary |
| `prompts/legacy/` | various | deprecated | earlier prompt versions retained for provenance; lighter review |

### Status
**validated**: tested against the corpus, outputs audited (e.g., verbatim-matched, human spot-checked)
**candidate**: designed but not yet used for a run.
**deprecated**: superseded; retained only for provenance.

## How to add a new prompt
1. Create `prompts/<task>_<version>.txt` (e.g. `prompts/claims_v8.txt`):
   the filename is the prompt's identity: it's the `prompt_version` in run
   metadata and the poster-slide badge.
2. Add a row to the table above (usually status `candidate`, with notes on
   what's untested).
3. Commit. Release + poster slide for human annotation only when results from it are headed into
   a published output (see below).

Note: Move a file to `prompts/legacy` only once `deprecated`. Else, just change the status from `candidate` to `validated` once
audited.

## Versioning Workflow

Two naming systems coexist:

- **Prompt names** (`claims_v7.txt`) are the human-given identity of a prompt
  *lineage*. v7 may pick up small edits over many commits
  and still be called v7. A new file only starts when this is judged as a new design. The filename is what the extraction pipeline records as `prompt_version`.

- **Release tags** pin exact bytes of small tweaks. This is because Zenodo can only archive and DOI
  whole-repo snapshots, not individual files, so a release freezes the
  entire prompt set and mints one DOI. A citation to one prompt is then
  "release DOI + filename". Release tags are dates: `2026-09-01` because semantic naming doesn't make sense for a set of prompts.

The workflow:

1. **Day to day: edit and commit only.** No release, no DOI.
2. **Release** when results produced with some prompt version are headed
   into a published output:

   ```bash
   gh release create 2026-09-01 --notes "what output is this for, what changed and why"
   ```
   Zenodo then archives the release and mints a DOI automatically.
   *(One-time setup before the first release: enable this repo at
   <https://zenodo.org/account/settings/github/>.)*

3. **Verify which prompt bytes a run used**: Since v7 may have changed after the run, the name alone isn't enough. Take the release cited next to
   the results, then: `git show 2026-09-01:prompts/claims_v7.txt`


## BONUS: Make a poster slide for manual annotations

Deck is made to print at A2. Each new prompt version gets its own slide, inserted before the template; existing slides (including manual annotation cards, dot anchors and leader lines) are never machine touched and should be updated as needed manually. The deck's **last slide is the TEMPLATE** -- do not delete it!

```bash
pip install python-pptx
python poster/make_slide.py prompts/claims_v7.txt poster/poster_deck.pptx
```

The badge ("PROMPT v7 · claims · 2026-08-20") is derived from the filename and can be overridden with `--badge "…"` if need be. Body text is 10 pt auto-dropping to 8 pt for long prompts (override with `--font-size`). Use `--out test.pptx` to try without touching the deck. 

## Licence

All original content is CC BY 4.0 (see `LICENSE`). All prompt text was
written for this project and has been reviewed for third-party material.
The maintained prompts contain no quoted text; worked examples are invented
illustrations, and references to real scholarship are attributed in place.
`prompts/legacy/` preserves earlier prompt versions for provenance under a
lighter standard of review. Any attributed third-party references remain
the copyright of their authors and are not covered by the CC BY licence.
Attribution is taken seriously: if you believe any passage requires
citation, correction, or removal, open a GitHub issue and it will be
addressed promptly.

## Cite

See `CITATION.cff` (GitHub renders it under the "Cite this repository"
button). Two cases:

- **Referring to the prompt set in general**: cite the concept DOI (the
  badge at the top, once released); it always resolves to the latest release.
- **Your results depend on one specific prompt**: cite the DOI of the
  release you used, plus the prompt's filename, e.g.:

  > Buyuklieva, B. (2026) et al. *CONCEIVE prompt set* (release 2026-09-01,
  > prompt `claims_v7.txt`). Zenodo. https://doi.org/10.5281/zenodo.XXXXXXX (tbc)

  No git knowledge or commit ID is needed: a release DOI permanently
  resolves to a Zenodo archive of the whole repository as it stood on that
  date, so "DOI + filename" already pins the prompt's exact text. To read
  the cited file, download the archive from the DOI page or browse it on
  GitHub by putting the release date in the URL, e.g.:
  `https://github.com/Bonnie-Buyuklieva/conceive-prompts/blob/2026-09-01/prompts/claims_v7.txt`
