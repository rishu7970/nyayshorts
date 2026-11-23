# Editorial Workflow & AI Prompt Guide
## How to Add 100-Word Rulings & Detailed Case Reports

This guide explains how to manage both parts of **NyayShorts**:
1. **The 100-Word Swipe Cards** (`rulings` table in Supabase)
2. **The In-App Detailed Case Reports** (Supreme Court Observer style, `case_reports` table in Supabase)

---

### Part 1: How the Two Tables Work Together

```
┌────────────────────────────────────────────────────────┐
│  TABLE 1: `rulings`                                    │
│  - Shows on the Mobile Swipe Feed                      │
│  - 40% News Photo + 60% White Card                     │
│  - Exactly 95–100 words holding                        │
└──────────────────────────┬─────────────────────────────┘
                           │
             User Taps Apple Glass Button
             "Tap to read detailed case report"
                           │
                           ▼
┌────────────────────────────────────────────────────────┐
│  TABLE 2: `case_reports`                               │
│  - Opens the Clean SC Observer Article Page            │
│  - Custom Headline & Subtitle                          │
│  - Full Names of Judges & Author                       │
│  - Long-form narrative paragraphs (as long as needed)  │
│  - Link to the certified signed PDF order at bottom    │
└────────────────────────────────────────────────────────┘
```

---

### Part 2: Master Prompt to Generate BOTH (Claude / Gemini)

Whenever you have a judgment PDF, case text, or legal news article, paste this single prompt into **Claude**:

```markdown
You are an expert legal editor for "NyayShorts", a legal intelligence mobile app.

I will provide a court judgment or legal news report. Please generate TWO outputs from it:

### OUTPUT 1: The 100-Word Inshorts Card (for `rulings` table)
- **title**: Punchy headline (10-14 words).
- **court_level**: Supreme Court / High Court / District Court.
- **specific_court**: e.g., Supreme Court of India / Delhi High Court.
- **category**: Practice area (e.g. Criminal Law / Constitutional Law).
- **bench**: Full formal names of all judges (e.g., Justice Aravind Kumar and Justice Vipul M. Pancholi).
- **citation**: Official neutral citation or date.
- **judgment_date**: YYYY-MM-DD.
- **summary**: STRICTLY 95 to 100 words. Plain English. Factual trigger + Ratio Decidendi. (Count words).
- **image_search_query**: 2-3 words for contextual news photo.

### OUTPUT 2: The Detailed Case Report (SC Observer Style for `case_reports` table)
- **report_title**: Full article title (e.g., "Lighting of Karthigai Deepam | Deepathoon to be lit as one-time measure").
- **report_subtitle**: Case matter / topic (e.g., "Lighting of Karthigai Deepam at Thiruparankundram Hill").
- **bench**: Formal list of judges.
- **author**: Reporter name (e.g., "Advay Vora / NyayShorts Desk").
- **report_date**: YYYY-MM-DD.
- **content**: 2 to 4 clean, well-spaced narrative paragraphs explaining:
  - Paragraph 1: What the bench directed today, key dates, interim vs final.
  - Paragraph 2: What petitioners sought, the previous High Court ruling, and arguments.
  - Paragraph 3: The constitutional or statutory clarification made by the Court.
- **pdf_url**: Link to official certified court order.

---
[ATTACH JUDGMENT PDF OR PASTE TEXT BELOW]
```

---

### Part 3: Adding Entries in Supabase (Step-by-Step)

#### Step 1: Add the 100-Word Card in `rulings`
1. Go to **[Supabase Table Editor](https://supabase.com/dashboard/project/zxbolqpmxlanslocqjsc/editor)**.
2. Click on the **`rulings`** table ➔ Click **"Insert row"**.
3. Paste: `title`, `court_level`, `specific_court`, `bench`, `summary` (100 words), etc.
4. Click **Save**. Note the `id` number assigned to this row (e.g., `50`).

#### Step 2: Add the Detailed Report in `case_reports`
1. In the same Supabase Table Editor on the left, click on **`case_reports`**.
2. Click **"Insert row"**.
3. Fill in:
   - **`ruling_id`**: Enter the `id` from Step 1 (e.g., `50`). *(This links the report directly to the card!)*
   - **`title`**: The custom headline from Output 2.
   - **`subtitle`**: The case matter subtitle.
   - **`bench`**: Full names of all judges.
   - **`author`**: Your name or `NyayShorts Legal Desk`.
   - **`report_date`**: Today's date.
   - **`content`**: Paste your detailed paragraphs (separate paragraphs with an empty line).
   - **`pdf_url`**: Official certified court PDF order link.
4. Click **Save**.

The moment you click Save, whenever any user taps the Apple Liquid Glass bar at the bottom of that card on their phone, it slides open your **clean, custom Supreme Court Observer-style briefing**!
