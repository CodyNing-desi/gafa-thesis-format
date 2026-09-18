---
name: gafa-thesis-format
description: Apply and verify Guangzhou Academy of Fine Arts (GAFA / 广州美术学院) undergraduate graduation thesis, creation report, or design report formatting. Trigger when the user says “按广美毕设论文格式改”, “广美论文格式”, “GAFA thesis format”, or asks to normalize a DOCX to the official GAFA undergraduate thesis template.
---

# 广州美术学院本科毕业论文 / 创作（设计）报告书格式 Skill

## Purpose

Use this skill to **reformat, normalize, or audit** an undergraduate graduation thesis / creation report / design report so that it follows the supplied Guangzhou Academy of Fine Arts official template.

The user may simply say:

- “按广美毕设论文格式改”
- “把这个 Word 改成广美本科毕业论文格式”
- “检查一下是不是符合广美毕设格式”

When triggered, do not ask the user to restate the formatting rules. Load `references/format-spec.md` and apply them.

## Source of truth

1. `references/format-spec.md` is the distilled operational specification.
2. `assets/广州美术学院本科毕业论文模板.docx` is the retained original reference template (byte-identical to the official 附件6《广州美术学院毕业论文参考范文模板》.docx, verified by MD5 in 2026-09).
3. If the distilled spec and the retained template appear to conflict, **the retained template wins**. Report the conflict instead of inventing a rule.
4. Do not silently substitute generic GB/T thesis formatting for a GAFA-specific rule.

## Required workflow

### A. Reformatting an existing DOCX

1. Inspect the document structure before editing: cover, declaration, TOC, Chinese abstract, English abstract, body chapters, conclusion, references, acknowledgements, and optional design/creation appendix.
2. Preserve substantive text, figures, tables, citations, equations, captions, section order, and user-authored wording unless the user explicitly asks for content editing.
3. Apply the page setup and typography from `references/format-spec.md`.
4. Map existing paragraphs to semantic roles rather than formatting by visual guess alone:
   - chapter title
   - second-level heading
   - third-level heading
   - body text
   - Chinese abstract title/body/keywords
   - English abstract title/body/keywords
   - figure caption
   - table caption/body
   - references
   - acknowledgements
5. Use automatic Word heading styles / outline levels for heading hierarchy where practical.
6. Generate or refresh the table of contents automatically; do not manually type dot leaders and page numbers.
7. Preserve citation numbering and cross-references. Do not renumber references unless required by actual document order.
8. Use chapter-based numbering for figures and tables (`图 1-1`, `表 2-1`, etc.).
9. Do not turn a table into a fully boxed Excel-style grid if the template calls for open left/right borders.
10. After editing, render the DOCX and visually inspect every page. Fix clipping, broken line wraps, orphan headings, table overflow, image overlap, TOC corruption, and page-numbering errors before delivery.

### B. Audit-only request

Return a concise compliance report grouped into:

- Page setup
- Cover
- TOC
- Abstracts
- Heading hierarchy
- Body text
- Figures and tables
- Citations and references
- Conclusion / acknowledgements
- Page numbering / section breaks

Mark each item as `符合`, `不符合`, or `无法从当前文件确认`.

## Non-negotiable rules

- A4 paper; all four margins 25 mm (the final section holding 致谢 + the creation/design statement follows the template's wider margins, ≈31.7 mm left/right — see `references/format-spec.md` §14).
- Main Chinese body: 小四号宋体, 1.5 line spacing, first-line indent 2 Chinese characters.
- Latin letters and Arabic numerals in body text: Times New Roman at the matching point size.
- Chapter title: 小二号黑体加粗, centered, 1.5 line spacing, 1 line before and after.
- Level-2 heading: 小三号黑体加粗, left aligned, 1.5 line spacing, 0.5 line before and after.
- Level-3 heading: 四号黑体加粗, left aligned, 1.5 line spacing, 0.5 line before and after.
- Figure title below figure; table title above table.
- TOC must be automatically generated.
- Chinese abstract length: 300–600 Chinese characters; keywords: 3–7.
- English abstract and keywords should be translations of the Chinese abstract and keywords.

## Important handling notes

- Chinese Word font sizes map conventionally as follows: 小初 36 pt, 二号 22 pt, 小二 18 pt, 小三 15 pt, 四号 14 pt, 小四 12 pt, 五号 10.5 pt. Use Word’s Chinese size labels where available.
- “段前/段后 1 行” and “0.5 行” should be implemented using Word paragraph spacing semantics as closely as the editor allows; do not casually replace them with arbitrary point values without checking the visual result.
- Use actual section breaks for changes in page-numbering scheme or front-matter/body transitions.
- Page numbering is fully verified from the template's XML: 封面, 学术诚信声明, and 目录 show **no page number**; 中文摘要 / Abstract use upper Roman numerals I–II restarting at the Chinese abstract; Arabic numbering starts at 1 on 第一章 and runs continuously through the end. Do not put page numbers on 声明/授权书/目录 pages, and do not start Arabic numbering at the abstract. Full 12-section map: `references/format-spec.md` §15.
- For any rule not explicitly present in `references/format-spec.md`, inspect the retained template before deciding.

## Output behavior

When asked to modify a document, return the edited `.docx` after render-and-verify QA. Do not return only a prose description of what should be changed.
