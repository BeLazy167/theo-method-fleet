# Testing skill descriptions

A description is the only part of a skill loaded into context unconditionally. Its one job is to fire on the right prompts and stay quiet otherwise. Over-triggering is the expensive failure: a wrongly-pulled skill burns context on requests it has no business touching, and it does that on every request, forever.

So the descriptions in this repo are tested. If you fork this and rewrite them — which you should — rerun the test.

## Running it

```sh
./bin/test-descriptions            # print a judge prompt
./bin/test-descriptions --answers  # expected results
./bin/test-descriptions --score results.txt
```

Paste the judge prompt into a fresh agent with no other context, save its reply, and score it. The judge must not see the expected answers, must judge each prompt independently, and must be told the mix is unbalanced — otherwise it infers that roughly half should fire and starts matching to hit that ratio.

`tests/prompts.tsv` holds the cases. Every negative in it is a near-miss that shares vocabulary with some description. Add a row whenever you find a prompt that fires wrongly; that is the regression test.

## What the current set found

23 prompts, 11 positives and 12 traps, run across six independent judges. All 23 land correctly, but the useful output was never the score — it was the near-miss reasoning and the confidence ratings. A correct answer at low confidence means the description decided narrowly and will break on the next phrasing.

Three defects it caught, each a description flaw rather than a judgment error:

- `file-upload` said "build artifact", which matched "upload the build to TestFlight" on two literal words with entirely the wrong intent.
- `apply-fleet` said "sync" with nothing scoping it, so it reached for "sync my dotfiles to the server".
- `postplan-read` said "any hosted HTML write-up link", which covered ordinary repo files.

## Two lessons worth keeping

**Negative clauses over-suppress.** Every "Not for X" added in the first fix round silently broke a case the skill *should* handle. `apply-fleet` excluded "dotfiles" — but `~/.claude/CLAUDE.md` is a dotfile. `html-communication` excluded product HTML while listing UI mocks, which are mocks of the product. Write the exclusion against the skill's purpose, not against the trap you just saw.

**Test the act, not the noun.** "Not for build artifacts" is wrong because a crash log attached to an issue is a build artifact that should absolutely be uploaded. "Not when the upload is itself the release step" is right, because it names what the user is doing rather than what they are holding.

A corollary: a description can be too *narrow* in a way that is invisible without a test. `audit-agent-history` briefly gained "wonders why an agent run took so long" as a trigger, which turned out to match every slowness complaint. It moved to the skill body, where a use case belongs. Triggers and use cases are not the same thing.
