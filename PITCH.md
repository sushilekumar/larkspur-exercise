# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A chat assistant for passengers whose flights are disrupted. The passenger gives their booking reference and surname. It finds the booking, checks what happened to the flight, works out what Larkspur owes them under its own policy, and offers the next steps, such as rebooking, meals or a hotel. Left out on purpose: refunds, fare differences, hotel approval, other-airline rebooking, groups of 10 or more, unaccompanied minors, partner-operated flights, pets, medical requests and baggage tracing. Larkspur's own handbook (section 7, "What chat automation will not do") gives these to people, because they commit money or need judgement. The assistant explains the situation and hands over with a summary.
Does: Handled all five kinds of disruption we tested to a clear answer, with no failures: a cancellation, a short delay, a missed connection, a group booking and an abusive message. Each took about 14 seconds, where today a person would pick this up.
Number: About $0.07 per contact against the $6.90 Larkspur pays today for a person to process a case, roughly 1%. That is the cost of the AI model alone, at list price, measured on our five test tickets (80,347 tokens in, 5,844 out). At the workshop's estimate of 13,700 disruption chats a week, that is about $900 a week against about $94,500. It leaves out hosting, monitoring and testing costs, so the real figure will be higher. [TEAM: replace this with a measured figure from `python3 bench.py --label before`.]
Safety check: When the assistant is wrong, the customer is protected at the point of commitment. Nothing is rebooked until the passenger presses confirm, a held seat releases itself after 15 minutes, and anything out of scope goes to a person with the assistant's reasoning attached. We find out about a mistake by reading the step-by-step record each conversation leaves. Today that is manual: we have not built an alert or a review routine. [TEAM: say which of these protections you actually tried, and what you saw.]
Next: Name who looks after it once we leave. Today that is undecided, and it needs a Larkspur owner who reads a sample of conversations each week, fixes what is wrong and updates it when policy changes. [TEAM: name the role.] To make that safe we hand them our own test conversations (not yet written), so that any change can be checked before it reaches a passenger. Then make one improvement and measure it before and after.
Still broken: An angry or abusive passenger gets the same calm, routine reply as everyone else. Nothing changes its tone, and nothing flags the conversation for a person. We also have not yet confirmed that a conversation too tangled for the assistant is handed to a human, rather than simply ending.
Lever: <cost | speed | intelligence>

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
 