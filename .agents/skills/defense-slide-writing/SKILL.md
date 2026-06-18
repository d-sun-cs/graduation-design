---
name: defense-slide-writing
description: Use when Codex needs to write, rewrite, or review Chinese academic defense slides, especially thesis proposal, midterm, or final defense Beamer/PPT decks. Apply concrete-first, result-first, low-text slide writing; reduce abstract wording; improve page-level logic, image usage, layout, and compile/render verification.
---

# Defense Slide Writing

Use this skill to make defense slides easy for non-specialist committee members to follow. Prefer concrete facts, visible results, and large figures over report-style paragraphs.

## Core Rule

Write each slide as one clear fact chain:

1. What real object or result is being discussed.
2. What concrete problem or cost appears.
3. What method or implementation change addresses it.
4. What number, figure, or direct consequence proves it.

Do not start with abstract framing such as "research route", "focus", "objective", or "先看一个真实问题". Say the object and tradeoff directly.

## Default Workflow

1. Read the current slide source and the nearest source report or paper section.
2. Identify pages with too much text, no visual anchor, vague titles, or overlapping blocks.
3. Rewrite dense pages before touching well-supported figure/result pages.
4. Keep each page to one question or conclusion.
5. Compile or export the deck, then inspect representative rendered pages for overlap, tiny figures, and text overflow.

For LaTeX Beamer decks in this repository, verify with `latexmk -interaction=nonstopmode -halt-on-error main.tex`; use `make clean` after verification to remove auxiliary files while preserving the generated PDF.

## Writing Rules

- Put the answer in the title or subtitle: "慢在反复搬数据" is better than "优化目标与方法".
- Use 2-3 blocks on pure-text slides. Each block should contain 1-3 short bullets.
- Prefer noun phrases and numbers over full explanatory sentences.
- Move explanatory detail to speaker narration; slides only show what the audience must see.
- Avoid emphasizing "三副本" unless the specific replica count matters. Usually say "多副本".
- Avoid unexplained acronyms. On first use, write the Chinese term plus acronym, such as "键值缓存（KV Cache）".
- Introduce non-obvious system, library, or dataset names on first use with a short appositive, such as "Ceph：知名开源分布式存储系统"; do not spend a whole slide explaining a tool unless the tool itself is the topic.
- Show results before experimental details: first "+59.1%", then table/test setup.
- Keep metric labels consistent within the same slide. If two blocks compare the same kind of result, use the same label template, such as "编码平均提升", "解码平均提升", and "解码最高提升"; do not mix "单块恢复最高", "解码最高", and other partial variants unless the scope difference is the point of the slide.
- Do not compress report paragraphs into bullets. Rewrite them as object -> cost -> action -> result.

## Common Defense Story

For this graduation-design topic, introduce the motivation in this order:

1. Storage systems need reliability and redundancy.
2. Multi-replica redundancy is simple but uses more storage space.
3. Erasure coding saves space but adds encoding/decoding computation.
4. The optimization target is therefore to make encoding and decoding faster.

For the shift-XOR / Two-tone motivation page:

1. Reed-Solomon codes are mature but rely on finite-field matrix operations.
2. Shift-XOR erasure codes use shifts and XORs, so theoretical computational complexity is better than RS-style finite-field coding.
3. Two-tone is a theoretical SOTA code among shift-XOR erasure codes.
4. Existing shift-XOR codes lack high-performance storage-system implementations.
5. RS optimization techniques cannot be directly reused because the computation and memory-access patterns differ.
6. The slide conclusion should be: optimize Two-tone Shift-XOR code at the system implementation level.

## Layout Rules

- If a figure explains the idea, give it enough space. Prefer 55%-65% of slide width for the figure.
- For left-text/right-figure pages, keep the left side to the minimum explanation and vertically center the figure when the left side is shorter.
- Leave visible gaps between adjacent Beamer columns; avoid `0.50 + 0.50` block layouts when shadows or rounded blocks touch.
- Use tables for three or more comparable numbers. Use large text blocks for one or two key numbers.
- Do not put tiny figures on result pages. If a figure cannot be read, either enlarge it, crop whitespace, or remove it.
- Render at least the changed pages before finishing; visual inspection matters more than LaTeX compiling cleanly.

## Final Check

Before handing the deck back:

- Confirm the main source compiles or exports successfully.
- Confirm changed pages have no block overlap, off-page text, or unreadably small figures.
- Mention any unrelated dirty worktree changes separately; do not revert them unless explicitly asked.
