# FitFindr

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, every
> command, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> python app.py listings --full -n 6      # read the data (Milestone 1)
> python app.py fields                    # what you can filter on
> python app.py ask 'vintage graphic tee under $30'
> ```
>
> All three tools are stubs, so that last command will do nothing useful yet.
> That's the starting position.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     This is your submission. Fill each section in as you finish the milestone
     it belongs to — don't leave it all to the end.

     Unit 3 asks for the first five sections. Unit 4 adds the five below them.
     Leave the unit 4 sections alone until then; they're here so you know
     what's coming.

     Everything is pasted as TEXT. No screenshots, no images, no video links.
     A typed block of output gets full credit; a picture of the same output
     gets none.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 3 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Three or four sentences: what a user asks for, and what they get back. -->



---

## Tool Inventory

<!-- Four lines per tool. This is worth 2 points and it's the single most
     common place students lose them.

     "Returns a list" earns NOTHING. The description has to say what is IN
     the list.

     The empty case isn't optional either — it's the thing your loop branches
     on, and if you don't decide it here you'll discover it as a crash in
     Milestone 5. -->

### `search_listings`

- **What it does:** Searches the listing data to find items matching the description and filters by size and max price if specified. 
- **Inputs:**  
  - `description` (str) - contains keywords describing the item user wants
  - `size` (str) - size of item to filter results by
  - `max_price` (float) - specifies max price of items that should be returned
    <!-- name and type each: `max_price` (float), not "a price" -->
- **Returns:** A list of dicts where each dict is a listing dict representing an item that contains:
  - [id (str), title (str), description (str), category (str), style_tags (list of str), size (str), condition (str), price (float), colors (list of str), brand (str or None), platform (str)]
- **When it has nothing:** It retuns an empty list `[]`when there are no items in the listing data that match the description, size, and max_price.

### `suggest_outfit`

- **What it does:** Suggest an 1-2 outfits based on new item and existing wardrobe.
- **Inputs:** 
  - `new_item`(dict)  - a listing dict representing the item. It contains [id (str), title (str), description (str), category (str), style_tags (list of str), size (str), condition (str), price (float), colors (list of str), brand (str or None), platform (str)]
  - `wardrobe`(dict) - The items key maps to a list of dicts, where each dict contains `id` (str), `name`(str), `category` (str), `colors` (list of str), `style_tags`(list of str), `notes` (str)
- **Returns:** Returns 1-2 outfit suggestions based on the new item and existing items in the wardrobe. 
- **When it has nothing:** Provides general styling advice if the wardrobe is empty. It does not return an empty string or raise an error in this case.

### `create_fit_card`

- **What it does:** Creates a 2-4 sentence caption about the item and the outfit suggestion that someone would post on social media. 
- **Inputs:** 
  - `outfit` (str) - the outfit suggestion from `suggest_outfit`
  - `new_item` (dict) - listing dict that contains [id (str), title (str), description (str), category (str), style_tags (list of str), size (str), condition (str), price (float), colors (list of str), brand (str or None), platform (str)
- **Returns:** A caption that’s 2-4 sentences long describing the item, it’s price, and which platform it’s from. It also mentions the outfit they were able to make with the item and the vibe of it.
- **When it has nothing:** It returns a message saying that no outfit was provided. It does not raise an error or return an empty string.

---

## Planning Loop

<!-- Your branch rule, stated as a rule — the condition AND both paths — plus
     the file and function that holds it.

     Like this:
       "If search_listings returns an empty list, put a message in the session
        and stop. Otherwise take the first result and go to suggest_outfit."
        — agent.py::run_agent

     The grader checks your code against what you claim here, so the file and
     function have to be real. -->

**Branch rule:** If `search_listings` returns an empty list, put a message describing that no items were found and what parameters user can change using `_nothing_found_message` in `session[error]` and return the session. Otherwise, if `search_listings` has results, take the first item and store it in `session[selected_item]` and use it to call `suggest_outfit`.

**Where it lives:** `agent.py::run_agent`

**How the query is parsed:** <!-- regex, string splitting, or asking the model — say which -->
It is parsed using regex. `parse_query` extracts the price using `_PRICE_RE` by matching pharses such as “up to $30” and extracts the price from the phrase. The matched text is replaced with a space. It extracts the size using `_SIZE_RE` by looking for pharses starting with size and ending with a size word such as ‘L’. It falls back on `_BARE_SIZE_RE` if only a size word is mentioned. Then the matched text is replaced with a space. Finally, the description is extracted from the remaining text after striping the whitespace and commas. 

**What moves through the session:** <!-- which fields, in what order -->
A new session is created using `query` (str) and `wardobe` (dict). `query` is parsed by `parse_query` and stored in the `session["parsed"]`. Then `search_listings` is called on the parsed query by passing `￼`parsed["description"`￼]`, `parsed[“size”]`, `parsed[“max_price”]`. The result is then saved to session[“search_results”]. If empty, an error message indicating nothing found is created using `_nothing_found_message`  and the session is stopped. If not empty, first item from result is passed to `suggest_outfit` along with the sesion’s wardrobe. Then `create_fit_card` is called based on the outfit suggestion from `suggest_outfit` and the listing dict for the selected item. 


---

## Sample Run

<!-- Two things go here.

     1. One FULL query and its output, pasted as text.
     2. Your three per-tool terminal tests — the command and what it printed. -->

**One full query**

```
$ python app.py ask '...'

```

**The three tools, tested one at a time**

```
$ python -c "from tools import search_listings; print(search_listings('graphic tee', max_price=30))"

```

```
$ python -c "from tools import suggest_outfit; ..."

```

```
$ python -c "from tools import create_fit_card; ..."

```

---

## How I Used AI

<!-- Two specific moments. What you asked, what came back, what you changed.

     "I used Claude to help me code" is not enough.

     "I gave Claude my search_listings spec. It returned None on no match
     instead of an empty list, so I changed it" is the level we want. -->

**Moment 1**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

<!-- ═══════════════════════ UNIT 4 — THE TEST ═══════════════════════

     Don't fill these in during unit 3.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Run Log — Before

<!-- Five criteria, five tries each, in this exact format.

     Five, because your criteria are written out of five. Mark each try PASS
     or FAIL, count the passes, and read that count against your target — a
     row targeting 4 of 5 with three PASS cells is MISSED (3/5).

     `python run_eval.py --label before` runs everything and writes the table
     into results/. Paste it here and fill in the verdicts. -->

| Criterion | Target | Try 1 | Try 2 | Try 3 | Try 4 | Try 5 | Verdict |
|---|---|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |  |  |
| 2.  |  |  |  |  |  |  |  |
| 3.  |  |  |  |  |  |  |  |
| 4.  |  |  |  |  |  |  |  |
| 5.  |  |  |  |  |  |  |  |

**Real output from one try**, pasted as text, naming the file and function
that produced it:

```

```

---

## Verdicts and Diagnoses

<!-- MET or MISSED per criterion against LAST UNIT's target, plus a sentence on
     how you decided.

     Then, for every miss: which of the four places it happened — a tool, the
     loop's branch, the session, or the model's output — AND the mechanism.

     Not a diagnosis:  "The fit card was bad."
     A diagnosis:      "The fit card criterion missed on 2 of 5 items. Both had
                        an empty brand field. My prompt puts the brand in the
                        first sentence, so the card opened with a blank and read
                        like a fragment. The tool worked; the prompt assumed a
                        field that isn't always there."

     Look for a pattern. Three misses on the same tool is one problem, not
     three. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**



---

## Loop Trace

<!-- One full run, printed step by step, with the MCP call visible in it.

     `python app.py ask '...' --trace` once you've added the trace.step()
     calls in Milestone 2.

     Worth pasting BOTH the happy path and the empty-search path. The empty
     one should be visibly shorter, because it stops. If your two traces are
     the same length, your branch isn't working — and this is the fastest way
     anyone will ever find that out. -->

**Happy path**

```

```

**Empty search**

```

```

**On the MCP move:** <!-- what changed in your code, and whether anything
behaved differently afterwards. If the rewire didn't work, say exactly where it
broke — the error text and the last thing that worked. That earns the point in
full. -->



---

## The Improvement

<!-- What you changed, why your diagnosis pointed at it, and the after-run in
     the same table format. One change, measured properly.

     `python run_eval.py --label after` -->

**What I changed:**

**Which failure it was meant to fix:**

### Run Log — After

| Criterion | Target | Try 1 | Try 2 | Try 3 | Try 4 | Try 5 | Verdict |
|---|---|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |  |  |
| 2.  |  |  |  |  |  |  |  |
| 3.  |  |  |  |  |  |  |  |
| 4.  |  |  |  |  |  |  |  |
| 5.  |  |  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it made things worse, say that. Honestly reported, that earns full
     credit and is more interesting than one that worked. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped where
     you did. "I ran out of time" is fine if it's true. Pretending nothing is
     left is not. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 3

       [ ] criteria.md has five numbered criteria, each with a target
       [ ] Each criterion has a reason underneath it
       [ ] All five unit 3 sections above have real content
       [ ] Tool Inventory: all three tools, inputs WITH TYPES, a specific
           return value, and the empty case
       [ ] Planning Loop names the branch rule and agent.py::run_agent
       [ ] Sample Run: one full query plus the three per-tool tests, as text
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN, you submit the same one
           next unit

     SUBMISSION CHECKLIST — unit 4

       [ ] mcp_server.py exists with one tool registered
           (or a written record of exactly where the rewire broke)
       [ ] Run Log — Before, five criteria, five tries each
       [ ] Real output pasted underneath, naming file and function
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, naming a place AND a mechanism
       [ ] Loop Trace, with the MCP call visible in it
       [ ] All three failure modes triggered and handled
       [ ] One improvement, with Run Log — After in the same format
       [ ] What's Still Broken
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository. Your commit history is what
     shows your criteria existed before your results did.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
