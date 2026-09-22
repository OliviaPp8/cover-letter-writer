---
name: cover-letter-writer
agent_created: true
description: This skill should be used when a user asks to write, draft, or rewrite a job application cover letter (求职信 / 应聘信 / 求职动机信). Trigger phrases include "帮我写 cover letter", "写一封求职信", "cover letter", "求职信", "应聘信", "求职动机信", "cover letter for [company]". The skill guides information collection (language choice + JD + company + CV), industry research, template-based drafting following a structured Chinese-origin template in Chinese or English, and producing both Markdown and Word output files.
---

# Cover Letter Writer

## Overview

A reusable skill for writing tailored, story-driven job application cover letters. Unlike generic "letter for company X" outputs, this skill enforces a structured workflow: collect inputs (language + JD + company + CV) → research company/industry recent moves → draft using a fixed Chinese-origin template in the user-chosen language → polish → output Markdown and Word files.

## When to Use

Trigger this skill whenever the user asks to write, draft, or polish a cover letter / 求职信 / 应聘信 / 求职动机信 for a specific job application. The skill is designed for **Chinese users applying to multinational / cross-border jobs** (where bilingual EN/CN cover letters are common) but works for any single-language application.

## Workflow

Follow these steps in order. Do NOT skip the input-collection phase — drafting without a real JD + real CV produces generic fluff.

### Step 1 — Collect Required Inputs

If the user has not provided the following, use `AskUserQuestion` (or a clear text request) to gather them. Without these, do not start drafting.

**Language — ask this FIRST:** Use `AskUserQuestion` at the very start to ask which language to write in. Offer three options: 中文 / 英文 / 中英双语 (side-by-side). Do NOT silently default to the JD's language — always confirm the user's choice before drafting.

**Required inputs (must have):**
1. **Language** — the user's choice from the question above (中文 / 英文 / 双语).
2. **Target Job Description (JD)** — full text or URL. If URL, fetch with WebFetch.
3. **User's CV / resume** — file path or pasted text.
4. **Target Company** — name (used to research recent news and decide whether to name them in the letter).

**Optional but recommended:**
- Industry / sector the company is in (helps narrow the "recent move" research).
- Specific language style preferences (formal / warm / data-heavy).
- Length preference (default ~280-320 words in English; user can ask to shorten 1/3, 1/2, etc.).

If user only provides partial info (e.g., JD but no CV), ask for the missing pieces before drafting. Never invent experience or numbers.

### Step 2 — Analyze the JD

Extract and write down (do NOT skip — these drive the letter):

- **Core problem 1** (1-line summary of what the role really solves)
- **3-5 hard requirements** (years of experience, language, skills, education)
- **3-5 keywords** that must appear in the letter verbatim (e.g., "Go-to-Market", "Partner Development", "cross-border growth")
- **Preferred qualifications** that the user can highlight if matched
- **Language requirement** (English only / English + Mandarin / English + Spanish / Chinese only)

### Step 3 — Extract User Highlights from CV

Pull from the CV (not the JD) the elements that map to JD requirements:

- Total years of experience + dominant domain (e.g., "9 years BD + Growth Marketing in Web3 / Crypto")
- 1-2 **quantified core achievements** that mirror the JD's "core problem" (e.g., "$13M project incubation, 1700% ROI channel growth")
- **Career arc** (previous companies → current company). **Naming rule**: name well-known brands (Binance, Bitget, Google, Tencent) directly; for less-known companies, omit the brand name and describe the field instead (e.g., "a Web3 wallet company", "a logistics SaaS startup")
- **3-5 work keywords** (e.g., brand marketing, paid acquisition, event planning, community growth, channel ops)

### Step 4 — Research the Company's Recent Moves

Use `WebSearch` (and `WebFetch` for the top result) to find a specific recent action, dynamic, challenge, or industry news related to:
- The target company itself, OR
- The company's brand/product line, OR
- The broader industry the role sits in (pick whichever has the freshest / most relevant material)

Pick **one** concrete item with a date or event. Avoid vague phrases like "Google is a leading tech company". The item must be:
- Recent (within ~6 months preferred, up to 12 months acceptable)
- Specific (a product launch, lawsuit, partnership, earnings call, regulation, hiring spree, etc.)
- Connected to the role (e.g., for a "Google Play Partner Development" role, research Google Play / Android ecosystem news, not generic Google news)

If research fails to surface anything specific, **say so honestly** and pick the role's industry instead. Never fabricate news.

### Step 5 — Draft Using the Fixed Template

Use the following template **structure** verbatim. This is the user's preferred format — preserve the section ordering and the conversational tone. Fill in the bracketed `[…]` slots with the material from Steps 2-4.

```
[Salutation — match the user's preferred language and formality; default "Dear Hiring Manager," in English or 尊敬的招聘负责人： in Chinese]

我有 X 年的经验，核心解决过【JD 里面的核心问题 1 句话简述你做的核心成就+数据结果。或者 】。

我主要在 xxx 行业/领域，曾经任职于 xxx 公司，现在任职于 xxx 公司（假如是知名大厂可以说，假如不是，直接不用提公司名字，换成提及你在什么领域的公司，如：游戏公司）
核心的工作是 xxx，xxx，xxx（工作关键词，如：品牌营销，线上广告推广，活动策划）

我有关注贵公司/贵品牌/行业（选其中 1 个）近期【提及一个具体的动作，动态，挑战，或者行业信息】，我个人判断 【说出你的 1-2 句话的观点或者建议】。

我现在在看 xxx 类型的工作机会，觉得你的岗位是和我的经历比较匹配的，期待和您进一步沟通。

[Sign-off]
```

**English version** of the same structure (use when the user chose 英文):

```
Dear Hiring Manager,

I bring X years of experience in [domain]. Most notably, I [1 sentence: core achievement with data, mapped to the JD's core problem].

I have worked across [industry/sector], previously at [previous company — name only if well-known] and currently at [current company — name only if well-known, otherwise describe field, e.g., "a Web3 wallet company"]. My core work spans [3 keywords, e.g., brand marketing, paid acquisition, and community growth].

I have been following [your company / your brand / the industry — pick one], specifically [one recent action/launch/news with date]. My view: [1-2 sentences: your independent perspective or suggested action].

I am currently exploring [role type] opportunities and see strong alignment between your role and my background. I look forward to connecting.

Best regards,
[Name]
```

### Step 6 — Polish and Tighten

Apply these rules:

- **Length target**: 250-400 words in English (or 400-600 Chinese characters) unless the user specifies otherwise. The template's four short paragraphs naturally produce ~280-320 English words — do not pad just to hit a higher count. If the user asks to shorten 1/3, ~180-220 English words.
- **No filler openings** — skip "I am writing to express my interest" / "It is with great enthusiasm". Jump straight into value.
- **JD keyword mirroring** — every keyword identified in Step 2 must appear verbatim at least once.
- **Numbers always** — every achievement cited must include a quantified outcome ($X, X%, X users, X years).
- **No resume regurgitation** — the CV already lists experience. The letter adds narrative, perspective, and intent.
- **No company-name dropping for non-famous firms** — enforce the naming rule strictly.
- **Tone** — confident but not arrogant, specific but not robotic, warm but not gushing.

### Step 7 — Output Files

Generate **two** output files in a sensible location (default: a `cover_letters/` subfolder next to the user's CV, or wherever the user prefers):

1. **`Cover_Letter_<Company>_<Role>.md`** — Markdown source (editable)
2. **`Cover_Letter_<Company>_<Role>.docx`** — Word version (formatted, ready to submit)

Use `python-docx` for the .docx conversion. A reusable script lives at `scripts/build_cover_letter_docx.py`.

After writing both, call `present_files` so the user can preview/download.

### Step 8 — Offer Follow-ups

End the response by offering 2-3 natural follow-ups the user might want:

- Tone adjustment (more confident / more humble / add specific company name)
- Length adjustment (shorter / longer)
- Bilingual version (English + Chinese side-by-side)
- Cover letter for a backup role
- Embed the "recent move" paragraph into the CV

## Resources

### scripts/
- `build_cover_letter_docx.py` — converts a Markdown cover letter into a professionally formatted .docx (Calibri 11pt, 1.15 line spacing, 1-inch margins). Use this to regenerate the .docx after editing the .md.

### assets/
- `cover_letter_template.md` — the blank template (Chinese + English versions) for reuse.

## Writing Style for the Letter Itself

- Use **first person**, active voice.
- Prefer **specific verbs** (spearheaded, scaled, drove, built, advised) over weak ones (worked on, helped with, was responsible for).
- **Numbers before adjectives** ("3x revenue lift" beats "significant revenue lift").
- Avoid corporate buzzwords in clusters (synergy, leverage, holistic, robust).
- Use the **language the user chose in Step 1** (中文 / 英文 / 双语). Do NOT auto-match the JD's language. For a 双语 (side-by-side) choice, produce both the Chinese and English versions of the same letter.