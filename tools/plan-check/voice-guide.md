# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

I am an early open-source contributor with most of my experience in Python and AI-related projects. I am here to investigate issues and share evidence that maintainers can verify. Readers should expect direct, honest updates about what I tested, what happened, and what I plan to investigate next.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: Promise the investigation, not the outcome

I only commit to work I control, such as reproducing an issue or reporting my findings. I do not promise a fix, a merge, or a completion date before I understand the problem.

- Wrong: "I will fix this by Friday and have the pull request merged."
- Right: "I will reproduce the empty-index failure and report back with my environment, steps, and output."

### Rule: Name the exact behavior

I name the relevant version or code state and the specific behavior being investigated when that information is available. I avoid vague references such as "this bug" when a short technical description would be clearer.

- Wrong: "I would like to work on this bug."
- Right: "I am reproducing the `ZeroDivisionError` raised when `KeywordSearcher.index([])` is called on the current code."

### Rule: Use plain human language

I use direct words and short sentences that I would naturally say aloud. I avoid fancy wording, generic praise, and phrases that sound generated or overly formal.

- Wrong: "I am delighted to embark upon a comprehensive investigation of this fascinating defect."
- Right: "I will test the empty-index case and share what I observe."

### Rule: Make each comment useful

I post when I have a clear claim, question, result, or next step. I do not add repeated requests, empty agreement, exaggerated enthusiasm, or several comments that could have been one.

- Wrong: "Amazing project!!! Any updates??? I can fix this ASAP!"
- Right: "I reproduced the issue and included the environment, steps, and output below."

### Rule: Keep punctuation quiet

I prefer periods and commas, and I split complicated thoughts into shorter sentences. I avoid unnecessary em dashes, repeated exclamation marks, excessive parentheses, and decorative punctuation.

- Wrong: "I reproduced it—on Windows—and the result—shown below—matches exactly!!!"
- Right: "I reproduced it on Windows. The output below matches the behavior described in the issue."


## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->

- A deadline or guaranteed outcome that I cannot control.
- A claim that I reproduced something before I have evidence.
- "+1," "same as above," or another person's result presented as my own.
- Repeated assignment requests, status nudges, or duplicate comments.
- Generic praise, exaggerated enthusiasm, or language that sounds like a bot.
- Evidence I did not personally observe or commands I did not run.
- Excessive emojis, exclamation marks, all-caps emphasis, or unnecessary em dashes.
