# Upstream notices

This file records sources and license notices for bundled third-party material. Source revisions describe provenance, not dependencies to install. Review upstream changes before incorporating them so local adaptations and project-specific decisions remain deliberate.

## Pstack

[unslop.md](unslop.md) is reproduced from Lauren Tan's [Unslop skill at revision 7366ac128bdf95f45e6734f412b49a4031800169](https://github.com/cursor/plugins/blob/7366ac128bdf95f45e6734f412b49a4031800169/pstack/skills/unslop/SKILL.md), with its skill frontmatter removed and attribution added.

[blast-radius.md](blast-radius.md) is adapted for standalone use from Lauren Tan's [Blast radius skill at revision c5db7fef1f1b1ebb2d4b7ae0308bf4beb10cb4c1](https://github.com/cursor/plugins/blob/c5db7fef1f1b1ebb2d4b7ae0308bf4beb10cb4c1/pstack/skills/blast-radius/SKILL.md), fetched September 10, 2026. The workflow, evidence ladder, and reporting format are retained. The exact adaptations are:

* Remove skill frontmatter and invocation examples because the main skill owns loading.
* Remove the companion-skill introduction and the dependency on `why`'s unspecified rules.
* Replace `why`'s PR and commit lookup with a direct instruction to read them using available source-control tools.
* Replace `arena` with independent reviewers, several models when available, and an explicit fallback when reviewers are unavailable.
* Point `unslop` to the bundled reference and add attribution and this license link.

[Pstack license](https://github.com/cursor/plugins/blob/7366ac128bdf95f45e6734f412b49a4031800169/pstack/LICENSE):

```text
MIT License

Copyright (c) 2026 Lauren Tan

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
