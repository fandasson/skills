# Examples

## Idea fails the gate

Input:

```text
/linkedin idea: I just got promoted to Staff Engineer, feeling proud, want to share the news
```

Response:

```text
Verdict: Not worth posting yet: this is an announcement with no takeaway, "I got promoted" makes you the subject and gives the reader nothing to use.

What changed in how you work that got you here? A practice you picked up, a project that mattered, a mistake you had to unlearn? Write about that, and mention the promotion as one line of context, not the headline.
```

## Idea passes and is drafted

Input:

```text
/linkedin idea: we cut our CI build time from 40 min to 8 min by switching to remote caching, worth sharing what we learned
```

Response:

```text
Verdict: Worth posting: concrete numbers, a real technique, and a takeaway beyond "we did a thing."

40 minutes. That's how long our CI build used to take.

We just got it down to 8, by switching to remote caching.

Nothing fancy. No rewrite, no new tooling most people haven't heard of. Just caching layers we already had, wired up properly.

The bigger shift wasn't technical. Engineers stopped skipping tests to save time, because waiting stopped being expensive.

If your CI is slow enough that people work around it instead of trusting it, the fix might be smaller than you think.
```

Examples are author-specific. Replace or remove them when adapting the skill for another person.
