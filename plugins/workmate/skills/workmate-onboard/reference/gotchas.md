# Gotchas

## Pacing

**Good:** Ask one interview question, wait for the answer, then move on.

**Bad:** Drop all five questions at once. The user picks two and skips the rest, and the agency context block is half-empty.

## Recipe selection

**Good:** Map the stated headache to one recipe and run it. Narrate as you go.

**Bad:** Run three recipes "to give a full picture" — the user loses interest before the third one finishes and the session times out.

## Permission failures

**Good:** When a tool returns `ACCESS_DENIED`, record the missing right in the agency-context block, tell the user what they won't see, and continue. The other skills still work.

**Bad:** Treat a permission error as a fatal failure and ask the user to "go fix it before we proceed." That puts the agency context capture at risk.

## GL Company scope

**Good:** When the user says "we're three GL companies but I only run the Brooklyn one," store `glCompanyID = BKLN` (or whatever the ID is) in the agency context. Every downstream skill will pass it as a filter.

**Bad:** Skip the GL company question because "the SP filters automatically." It does enforce permissions, but if the user has access to all three companies and is only interested in one, the skills will return blended data that obscures the answer they wanted.

## Storing context

**Good:** Show the draft profile, get explicit "yes save it" before writing.

**Bad:** Write the profile silently after the last question, then announce "saved!" The user wanted to tweak the third headache before you saved.

## Triggering operational changes

**Good:** Onboarding only **reads**. If the user says "and while you're at it, log an hour against project X" — say "I can do that — Workmate covers analysis; for time entry I'll use the `add_time_entry` Workamajig tool directly. Want me to do that now or after onboarding?"

**Bad:** Quietly call `add_time_entry` during onboarding. The user didn't expect side effects.
