# Client review: Larkspur disruption-care agent

**To:** Room 4  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-12 09:41

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. PITCH.md Number line: 80,347 tokens in, 5,844 out across five tickets, but readout-trace.json holds one run at 17,974 in and 1,212 out.**

The pitch says '$0.07 per contact' and '14 seconds' on five test tickets. The committed trace is a single wire run: 4 API turns, 5 tool calls, 12.7s wall clock. The pitch also carries its own placeholder, '[TEAM: replace this with a measured figure from python3 bench.py --label before]'. No before or after figure appears anywhere in the material.

Run python3 bench.py --label before --stage 1 --runs 3 and paste the per-contact cost and latency it prints.

**2. agent.py run_agent() has MAX_TOOL_CALLS = 8 and the diff shows no fallback after the cap, yet PITCH.md Still broken says handoff on tangled cases is.**

The while loop exits when turns reaches 8, and run_agent() then returns text_of(response) with no call to escalate_to_human and no marker that the cap was hit. The committed trace used 4 turns, so the cap path has not run. No eval cases exist (there is no evals/cases.json), so a conversation that hits 8 turns has no recorded outcome.

Run python3 run.py --all --trace and paste the totals footer showing the highest turn count across the booking shapes.

**3. hold_seat (73 characters) and send_confirmation (71 characters) are the two thinnest tool descriptions in build_tools().**

hold_seat reads 'Place a 15-minute hold on one alternative. Reversible. It simply expires.' and send_confirmation reads 'Send the customer a written confirmation of what was just done. Benign.' Neither says when to call it or what the argument comes from. The diff rewrote search_alternatives from 'search' to 369 characters and left both of these alone. The committed trace never called either one, so nothing shows how Claude chooses between them.

Run python3 run.py --show-tools and paste the hold_seat and send_confirmation entries beside a trace of a rebooking that reaches both.

**4. agent.py reopen_stats and its EXTRA_TOOLS schema: the 484-character description, but the committed trace never called it.**

reopen_stats reads 8 rows from data/americas/transcripts_sample.jsonl and counts reopened_within_72h per intent_label. Its description tells the model to quote counts and say 'it is only 8 conversations'. The tool order in readout-trace.json is lookup_booking, get_flight_status, next_available_day, check_policy, search_alternatives. reopen_stats is in tool_list() on every turn, and the trace shows no call to it and no cost or accuracy effect from offering it.

Run python3 run.py K7PQ2M --trace and paste the tool-call list showing whether reopen_stats is called, with the input tokens that tool_list() adds per turn from python3 run.py --tool-tax.

**5. readout-trace.json shows prompt caching at 0 read, 0 written, and agent.py has no cache_control, while every turn resends 10 tool schemas plus system.**

The trace logs 17,974 input tokens over 4 API turns with a hit ratio of None. Each run_agent() call passes runtime_preamble() + SYSTEM_PROMPT + TONE_ADDENDUM and tools=tool_list() on every create(). TONE_ADDENDUM is still 0 characters, so the system text is unchanged turn to turn. The input side of the $0.07 figure is mostly this repeated prefix, and no model choice changes that repetition.

Run python3 bench.py --label before --stage 1 --runs 3, then paste the input-token share of the per-contact total from python3 bench.py --compare before after.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (270 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
