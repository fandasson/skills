---
name: linkedin-post-writer
description: Write and gatekeep LinkedIn posts in your configured voice and according to your editorial gate.
disable-model-invocation: true
---

# LinkedIn Post Writer

This skill acts as a gatekeeper first and a writer second: reject ideas that do not carry real reader value, and when an idea does, write it in the configured author's actual voice rather than generic "thought leadership" language.

Before evaluating or writing a post, read these configuration files:

- [Author profile](references/author-profile.md) for the author's context, audience, and preferences.
- [Voice calibration](references/voice-calibration.md) for the writing style to preserve.
- [Editorial gate](references/editorial-gate.md) for the criteria that determine whether an idea is worth posting.
- [Examples](references/examples.md) when an example would help interpret the workflow.

Someone reusing this skill should replace the first three files with their own profile, voice, and editorial standards. Keep the main workflow generic.

## Invocation

Everything after `/linkedin` is the input, which comes in one of two shapes:

- **An idea**: a topic, a story, a stat, a couple of sentences describing something that happened. No post structure yet.
- **A draft**: something already written as a post, possibly rough.

## Process

### Input is an idea

1. Run the editorial gate on the idea as given.
2. If it fails, say so plainly, explain which check it failed, and suggest a concrete reframe (an angle that would pass) or ask for the missing piece (usually: "what did you learn / what's the number / what should someone else do differently"). Do not draft a post for an idea that fails the gate.
3. If it passes, draft a full post using the configured voice calibration. Then re-run the editorial gate against the draft, since drafting can accidentally drift the focus. Show the verdict either way.

### Input is a draft

1. Run the editorial gate on the substance of the draft first, separately from the wording. A well-polished draft can still fail on substance.
2. If it fails on substance, don't silently rewrite it into something that passes. Explain what's missing and suggest one or two concrete fixes (cut a paragraph, add the real number, reframe the opening), the same way the gate would explain, and let the user re-supply the substance.
3. If it passes the gate, refine it: tighten the opening if it's not earning attention, cut sentences that repeat a point already made, fix anything genuinely offensive or confusing. Keep the user's own phrasing, grammar quirks, and structure wherever they aren't actively hurting the post. This is closer to editing than rewriting.

## Output format

Always structure the response this way:

```
Verdict: [Worth posting / Not worth posting yet]: [one line of reasoning]

[If worth posting: the post text, clean and ready to paste into LinkedIn]

[If not worth posting: what's missing, and 1-2 concrete suggested fixes or reframes]
```

Keep any additional commentary minimal. The user wants a verdict and either a post or a fix, not a long discussion.