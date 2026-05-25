---
name: excel-championship-methods
description: Winning techniques from the Microsoft Excel World Championship (MEWC) and Financial Modeling World Cup (FMWC) — dynamic arrays, LAMBDA helpers, GROUPBY/PIVOTBY, REGEX, and speed tactics — with parallel translations to LibreOffice Calc and Python pandas. Use when solving analytical puzzles, speed-modeling scenarios, or porting heavy spreadsheet logic to code.
type: reference
origin: research-synthesized
---

# Excel Championship Methods

Spreadsheet esports (MEWC, FMWC) are won by people who compress a page of helper columns into one or two formulas, solve puzzles in 30 minutes, and think in arrays rather than cells. This skill catalogs the specific techniques used by champions like Andrew Ngai, Diarmuid Early, Michael Jarman, and Laurence Lau, and gives parallel implementations in **Excel 365**, **LibreOffice Calc**, and **Python / pandas** so the same pattern can be applied in any environment.

## When to Activate

- Solving puzzle-style or timed analytical cases (game simulations, lookups, aggregation, text parsing)
- Porting legacy spreadsheet logic to code, or vice versa
- Choosing between a formula-first vs code-first approach for an ad-hoc task
- Learning or teaching modern Excel (post-2022 dynamic array era)
- Comparing function availability across Excel 365, LibreOffice Calc, and pandas

## Champions and their signature styles

| Champion | Home | Titles | Known for |
|---|---|---|---|
| Andrew Ngai ("The Annihilator") | Australia (actuary, Taylor Fry) | MEWC 2021, 2022, 2023 | Consistency under pressure; deep INDEX/MATCH + XLOOKUP + lookup-table mastery; actuarial intuition for cashflow/probability cases. FMWC Open 2019 champion. |
| Michael Jarman ("The Jarman Army") | UK/Canada | MEWC 2024 | Modeling games in Excel (beat Ngai on a World-of-Warcraft–themed case); simulation and iteration. |
| Diarmuid Early | Ireland | MEWC 2025, FMWC 2025 | Dynamic-array & LAMBDA helper mastery (MAP/REDUCE); "groundwork first, then bonuses" strategy; origami / geometric cases solved with array formulas. |
| Laurence Lau | Hong Kong | Multiple FMWC podiums | Elegant LET-heavy architectures; fast keyboarding. |

Common pattern: every champion says the same thing publicly — **there's always a better way than copy-paste**; **LAMBDA collapses helper columns into one line**; **keyboard-only workflow**; **read the whole case before starting**.

## The six core weapons (ranked by championship impact)

### 1. Dynamic arrays + spilled ranges (Excel 365)

Formulas return arrays that **spill** into neighboring cells. The single biggest shift in competitive Excel since 2020. Replaces `Ctrl+Shift+Enter` array formulas, helper columns, and most copy-drag operations.

```
=UNIQUE(A2:A1000)                  ' unique list, spills down
=SORT(FILTER(data, data[region]="EU"))  ' filter + sort in one formula
=SEQUENCE(10, 1, 1, 1)             ' 1..10
```

- **LibreOffice Calc:** **No native dynamic arrays.** Use legacy array formulas (`Ctrl+Shift+Enter`) or the `lox365` extension. For `UNIQUE`: older Calc versions need `INDEX/SMALL/IF` or a pivot table; recent versions have `UNIQUE`.
- **pandas:** `df['col'].unique()`, `df.sort_values()`, `df[df.region == 'EU']`.

### 2. LAMBDA + helper functions — MAP, REDUCE, SCAN, BYROW, BYCOL, MAKEARRAY

Named `LAMBDA`s turn Excel into a functional language. Helpers iterate without loops.

```
=LAMBDA(x, x^2)(5)                           ' inline lambda
=MAP(A1:A10, LAMBDA(v, v*2))                 ' double each
=REDUCE(0, A1:A10, LAMBDA(acc, v, acc+v))    ' sum (accumulator)
=SCAN(0, A1:A10, LAMBDA(acc, v, acc+v))      ' running total
=BYROW(A1:C10, LAMBDA(row, SUM(row)))        ' per-row aggregate
=MAKEARRAY(5, 5, LAMBDA(r, c, r*c))          ' multiplication table
```

Recursion: a `LAMBDA` defined in the Name Manager can call itself — enables tree traversal, combinatorics, Fibonacci, path-finding.

- **LibreOffice Calc:** **No LAMBDA, MAP, REDUCE, SCAN, BYROW, BYCOL.** Substitute with `SUMPRODUCT`, nested `IF`, manual array formulas, or drop into **Basic macros / Python macros**.
- **pandas:** native Python lambdas + `apply`, `map`, `agg`, `functools.reduce`, `itertools.accumulate`.

### 3. LET — name intermediate expressions

Eliminates redundant computation and makes formulas readable.

```
=LET(
   sales,  B2:B1000,
   target, 10000,
   over,   FILTER(sales, sales > target),
   ROUND(AVERAGE(over), 2)
)
```

- **LibreOffice Calc:** No `LET`. Repeat the expression or use a helper cell.
- **pandas:** assign intermediates as variables or use `df.assign(...)` chains.

### 4. GROUPBY / PIVOTBY — formula-based pivot tables (Excel 365, 2024+)

```
=GROUPBY(data[region], data[sales], SUM, 3, 0)
=PIVOTBY(data[region], data[quarter], data[sales], SUM)
```

- **LibreOffice Calc:** classic Pivot Table GUI, or `SUMIFS`/`SUMPRODUCT` per cell.
- **pandas:** `df.groupby('region')['sales'].sum()`, `df.pivot_table(index='region', columns='quarter', values='sales', aggfunc='sum')`.

### 5. XLOOKUP and INDEX/XMATCH — modern lookups

```
=XLOOKUP(key, keys, values, "not found", 0, 1)   ' exact, binary search
=INDEX(values, XMATCH(key, keys, 0, -1))         ' reverse search
```

Champion nuance: lookups only return the **first** match. For multi-match, use `FILTER`.

- **LibreOffice Calc:** `XLOOKUP` is available in recent versions (7.6+). Older: `INDEX(MATCH(...))` or the `lox365` extension.
- **pandas:** `df.merge(lookup_df, on='key', how='left')` is the general pattern; `df.set_index('key').loc[k]` for single lookups.

### 6. TEXTSPLIT / TEXTBEFORE / TEXTAFTER / REGEX

Modern text parsing. Replaces chains of `LEFT`/`RIGHT`/`MID`/`FIND`.

```
=TEXTSPLIT("a,b;c,d;e,f", ",", ";")      ' 2D split
=TEXTBEFORE(A1, "@")                      ' local part of email
=TEXTAFTER(A1, "@")                       ' domain
=REGEXEXTRACT(A1, "\d+")                  ' first number
=REGEXREPLACE(A1, "\s+", " ")             ' collapse whitespace
```

- **LibreOffice Calc:** `REGEX()` function is extremely powerful — works inside `COUNTIF`, `SUMIF`, `SEARCH`. Enable under Tools → Options → Calc → Calculate → "Enable regular expressions in formulas". No `TEXTSPLIT`; use `REGEX` with a capture group or `MID`/`FIND`.
- **pandas:** `df['col'].str.split(',', expand=True)`, `.str.extract(r'(\d+)')`, `.str.replace(r'\s+', ' ', regex=True)`.

## The strategic layer — how champions actually win

From published post-mortems (Diarmuid Early's 2025 MEWC walkthrough, Ngai's 2023 "How I won" writeup, FMWC case bundles):

1. **Read the whole case first.** 2–3 minutes of reading saves 10 minutes of rework. Identify which outputs depend on which inputs; find the cheapest bonus.
2. **Establish groundwork before speeding.** In the 2025 final Early *skipped* early bonuses to build a clean data structure, then swept every bonus by the halfway mark. Compare to the "race through easy points" approach — it leaves a brittle model that can't answer the hard bonus.
3. **One formula, one truth.** Build the answer as a single spilled formula wherever possible; avoid helper columns that you'll have to drag.
4. **Use named `LAMBDA`s from a personal library.** Champions keep reusable LAMBDAs (e.g., `SPLIT_AT`, `TO_BINARY`, `CHOOSEK`) in their muscle memory. Pre-thought templates beat improvisation under a 30-minute clock.
5. **Keyboard only.** Mouse = seconds lost per action × hundreds of actions per case. Required hotkeys: `Ctrl+T` (table), `Ctrl+Shift+L` (filter), `F4` (absolute refs), `Alt+=` (sum), `Ctrl+Shift+Enter` (legacy arrays), `Ctrl+` (nav/select).
6. **Stop polishing, submit.** Points are scored for correct answers, not elegance.
7. **Know the platform's corner cases.** `XLOOKUP` only returns first match. `SUMIFS` ignores wildcards unless criteria is a string. Array functions coerce booleans differently across versions.

## Case archetypes to expect

FMWC/MEWC cases rotate through a small set of archetypes. Know the pattern and you know which weapon to reach for:

| Archetype | Example cases | Primary weapons |
|---|---|---|
| Board/card game simulation | Battleship, Dominate the Dominoes, Poker, Cornhole, Pass the Arrows | `LAMBDA` recursion, `SCAN`, `MAKEARRAY`, `REDUCE` |
| Geometric / spatial puzzle | Origami (2025 final), grid / pathfinding | `MAKEARRAY`, `BYROW`, trigonometry, iterative layout |
| Financial / actuarial model | Cashflow, NPV, commissions, insurance reserves | `LET`, `REDUCE`, `SUMPRODUCT`, `IRR`/`NPV`, `XIRR` |
| Data aggregation & reporting | Sales rollups, dashboards, KPI | `GROUPBY`, `PIVOTBY`, `FILTER`, `SORT`, `UNIQUE` |
| Text / data cleaning | Parsing free-text, standardizing codes | `TEXTSPLIT`, `REGEXEXTRACT`, `MAP`, `TRIM` |
| Sim / Monte Carlo | Asteroid mining (Eve Online case), probability | `RANDARRAY`, `SCAN`, data tables, or escape to Python |

## When to abandon Excel and reach for Python

Even MEWC rules allow Python. Champions use it when:

- The solution requires a real algorithm (graph search, optimization, ML)
- The dataset won't fit in a reasonable workbook (>1M rows)
- A proven library exists (`scipy.optimize`, `networkx`, `numpy.linalg`)
- The same transformation is needed across many files — script > macro

For mixed workflows, prefer:
- **pandas** for analysis
- **openpyxl** for reading/writing `.xlsx` without Excel installed (fast, pure-Python, streamable via `read_only=True`)
- **xlwings** when a live workbook must stay open and Python is a helper
- **XlsxWriter** for writing only — fastest for large output files
- **Python in Excel** (Microsoft 365) for inline pandas/numpy/matplotlib in cells via `=PY()`

## Cross-platform translation cheatsheet

| Task | Excel 365 | LibreOffice Calc | pandas |
|---|---|---|---|
| Unique values | `=UNIQUE(A:A)` | `UNIQUE` (7.x+) or pivot | `s.unique()` |
| Filter rows | `=FILTER(tbl, tbl[x]>0)` | Ctrl+Shift+Enter: `INDEX+SMALL+IF` | `df[df.x > 0]` |
| Sort | `=SORT(A:A)` | Data→Sort or array formula | `df.sort_values('x')` |
| Sequence 1..n | `=SEQUENCE(n)` | `ROW(INDIRECT("1:"&n))` | `range(1, n+1)` |
| Lookup | `=XLOOKUP(k, ks, vs)` | `XLOOKUP` (7.6+) or `INDEX/MATCH` | `df.merge(...)` or `.loc[]` |
| Aggregate by group | `=GROUPBY(g, v, SUM)` | Pivot Table / `SUMIFS` | `df.groupby('g').sum()` |
| Pivot | `=PIVOTBY(r, c, v, SUM)` | Pivot Table | `df.pivot_table(...)` |
| Conditional sum | `=SUMIFS(v, g, "A")` | `SUMIFS` or `SUMPRODUCT` | `df.loc[df.g=='A','v'].sum()` |
| Map each element | `=MAP(a, LAMBDA(x, f(x)))` | Array formula / macro | `s.map(f)` or `s.apply(f)` |
| Reduce / accumulate | `=REDUCE(0, a, LAMBDA(acc, x, ...))` | Macro | `functools.reduce` / `s.cumsum()` |
| Running total | `=SCAN(0, a, LAMBDA(acc, x, acc+x))` | `SUM($A$1:A1)` drag | `s.cumsum()` |
| Split text | `=TEXTSPLIT(s, ",")` | `REGEX` or MID/FIND chain | `s.str.split(',', expand=True)` |
| Regex extract | `=REGEXEXTRACT(s, "\d+")` | `REGEX(s, "\d+")` | `s.str.extract(r'(\d+)')` |
| Name intermediates | `=LET(x, ..., y, ..., x+y)` | Helper cell | local var or `df.assign()` |

## References

See `references/` for deeper dives:

- `references/champion-playbook.md` — strategic patterns and published post-mortems
- `references/excel-365.md` — full function catalog with recipes
- `references/libreoffice-calc.md` — workarounds for missing modern functions
- `references/pandas-python.md` — idiomatic pandas patterns for every Excel pattern above
- `references/speed-shortcuts.md` — keyboard shortcuts, case-prep drill
- `references/case-archetypes.md` — recognizable problem patterns and which weapon to reach for

## Sources

Research drawn from Microsoft Community Hub MEWC announcements (2024, 2025), `mewc.wiki.gg`, Financial Modeling World Cup product pages, Diarmuid Early's LinkedIn writeups and YouTube walkthroughs ("Origami" 2025 final), Andrew Ngai's "How I won 2023" LinkedIn post and Actuaries Digital interview, Microsoft Excel blog (GROUPBY/PIVOTBY launch Feb 2024, REGEX July 2024), and official pandas 3.x docs.
