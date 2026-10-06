# Acceptance criteria — FitFindr

Five criteria that say what "working" means for this agent, written in unit 3
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. *"The agent handles errors"* is an opinion.
*"When search returns nothing, the agent stops before calling the second tool,
in 5 of 5 tries"* is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter one. A reason that says something about your tools, your loop, or the
data earns credit; *"80% seemed reasonable"* does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

**Two are written for you. You write three.**

---

## 1. A matching query completes all three tools

Given a query that matches at least one listing, the agent completes all three
tool calls and returns a fit card — in at least 4 of 5 tries.

**Why this target:**
<!-- Why 4 of 5 and not 5 of 5? Something about your search, probably —
     "my search is a plain keyword match and some phrasings will miss" is a
     real answer. -->

`search_listings` will score items based on matching keywords so there could be a miss for 
an item that fits the description, but has keywords that are synonyms. For example, a user could put "early 2000s" but the keyword the item has is "Y2K" and result in a miss.

---

## 2. An impossible query stops before the second tool

Given a query that matches no listings, the agent stops before calling
`suggest_outfit` and returns a message naming what to change — 5 of 5 tries.

**Why this target:**
<!-- Why is 5 of 5 reasonable here when criterion 1 isn't? What's different
     about this path? -->

This should be 5 out of 5 since it is important for the branch path for no results to always be taken by the agent if `search_listings` returns an empty list. We also want to tell the user that no listings were found and how they can change their query. Otherwise, this would pass input to the other tools that they do not expect. 
---

## 3. Something about state

<!-- YOU WRITE THIS ONE.

     How would you know that the item your search found is the same item the
     next tool received? Name something countable or observable.

     This is the criterion people find hardest, because state failure doesn't
     look like state failure — it looks like a tool problem. Something that
     compares session["selected_item"] against what actually reached
     suggest_outfit is the shape you're after. -->

The item id of session["selected_item"] should match the id of the item in `suggest_outfit` for 5 out of 5 tries.

**Why this target:**
It's important for the same item returned from `search_listings` and saved to session["selected_item"] to be the same one passed to `suggest_outfit`. Otherwise, this would cause inconsistencies such as suggest_outfit creating an outfit based on an item that was unrelated to the search results. It should be 5 out of 5 because the session state should be consistent at all times and a lower threshold would be allowing the session state to be corrupted.


---

## 4. Something about the fit card

<!-- YOU WRITE THIS ONE.

     The fit card calls a model, so the same input can produce different words
     each time. That's not a bug — it's the nature of the tool. So what would
     make it acceptable?

     Think about what you'd actually be unhappy to see. A caption that never
     mentions the price? Two different items producing the same opening
     sentence? A card longer than a caption anyone would post? Any of those can
     be turned into a number. -->

The fit card captions should be no longer than 400 characters in 5 out of 5 tries.


**Why this target:**
Since an average sentence is around 100 characters and the output should be 2-4 sentences, I chose 400 characters at the limit. I set it at 5 out of 5 since the fit card caption needs to always meet the limit to be usable as a social media post and the length limit should be addressed by `create_fit_card`'s prompt.


---

## 5. Your choice

<!-- YOU WRITE THIS ONE TOO.

     Pick something you actually care about getting right. Speed, the empty
     wardrobe path, what happens when the model can't be reached, whether the
     search respects a price ceiling — anything, as long as it names a number
     or an observable outcome. -->

If the user has an empty wardrobe and found an item, `create_fit_card` should sucessfully create a caption from the generalized styling advice from `suggest_outfit` 5 tries out of 5.

**Why this target:**
The `create_fit_card` should not fail based on generalized styling advice from `suggest_oufit`. I chose 5 out of 5 since the listing dict for the item is always passed into `create_fit_card` so there is enough information to generate a caption about what the user found.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 4 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 4. Something about the fit card

         The fit card is different every time.

         **Why this target:** ...

         > **Revised in unit 4:** For 5 different items, the 5 fit cards share
         > no opening sentence.
         >
         > **Why revised:** "different" wasn't checkable — two cards that
         > differed by one word still counted. The new version is something I
         > can actually score.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "I said the empty search stops it 5 of 5 times, but I got 3 of 5,
            so 3 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
