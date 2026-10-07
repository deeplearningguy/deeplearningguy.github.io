---
title: "Jev isn't a better judge than Claude"
description: "I spent $40 putting Jev head to head with three Claude models. The result isn't what either fan club wants to hear."
pubDate: 'Oct 06 2026'
---

_I spent \$40 putting Jev head to head with three Claude models. The result isn't what either fan club wants to hear._

When TypeSafe launched Jev in September, the pitch was irresistible: a model that doesn't write, it decides. Every answer comes with a calibrated probability. Set a threshold, automate the confident cases, and send the rest to a human.

So I asked two questions. Is Jev actually a better judge than Claude? And when a question is genuinely hard, does its confidence drop?

The answers: no, and no. Jev judges about as well as Claude. Neither model knows when it's out of its depth.

## A test where "right" isn't one answer

Most benchmarks for these models ask one thing: did it pick the right answer? That tells you nothing about confidence. A model that says 98% on everything scores exactly the same as one that hesitates on the hard cases.

So I built [DoubtBench](https://github.com/trust123500/DoubtBench) on NVIDIA's [HelpSteer2](https://huggingface.co/datasets/nvidia/HelpSteer2) (CC BY 4.0). Every AI response in it was rated by several humans, and their individual votes are public. When three people disagree about whether a response is helpful, even the experts don't have one right answer. A good judge should show that same doubt.

DoubtBench turns that into 7,455 questions: rate a response on helpfulness, correctness, coherence, complexity and verbosity, decide if it's acceptable, and pick the better of two responses. The score out of 100 averages accuracy, calibration, and how closely a model's probabilities match the human votes.

## The scoreboard

Four models, boxed in by two reference points: the human annotators replaying their own votes (the realistic ceiling) and blind guessing (the floor). Total bill for all four runs: \$39.90.

| Model                      | Score (of 100) | Accuracy | Tracks human disagreement | Cost per full run | Median latency |
| :------------------------- | -------------: | -------: | ------------------------: | ----------------: | -------------: |
| Human annotators (ceiling) |           90.4 |    82.6% |                      1.00 |                   |                |
| Claude Haiku 4.5           |           69.2 |    45.8% |                      0.15 |            \$5.00 |          2.2 s |
| Jev 1.13                   |           67.8 |    48.2% |                      0.14 |            \$0.10 |          0.1 s |
| Claude Opus 5.5            |           67.8 |    44.0% |                      0.06 |           \$23.30 |          3.7 s |
| Claude Sonnet 5.5          |           67.4 |    42.5% |                      0.12 |           \$11.50 |          2.4 s |
| Uniform guessing (floor)   |           54.8 |    18.4% |                           |                   |                |

"Tracks human disagreement" is the correlation between a model's uncertainty and how much the annotators disagreed. 1.0 means the model gets unsure exactly where people do; 0 means no relationship.

## Finding 1: Jev isn't a better judge

Strip away the hype and it's a dead heat. All four models finish within 2 points of each other, and the winner isn't Jev. It's Claude Haiku 4.5, the smallest and cheapest Claude.

Jev does win on raw accuracy, by a few points against every Claude model (paired McNemar test, p < 0.001). Its edge is biggest on the easy calls: when every annotator agreed, Jev got 62% right and Claude 53% to 55%.

Claude claws it back on calibration, and its trick is humility. Its average confidence is about 0.54, against Jev's 0.65. Claiming less certainty puts Claude's stated confidence closer to how often it's actually right.

And bigger was not better. Haiku beat both Sonnet 5.5 and Opus 5.5 on accuracy. Opus, the flagship, cost the most and tied for second.

## Finding 2: nobody knows when a question is hard

This is the number that should worry anyone building on these models. When humans disagreed about a response, did the models get less sure? Barely. On a scale where the humans score 1.0, every model landed between 0.06 and 0.15.

Put simply: Jev is just as confident about the responses people fought over as about the ones everyone agreed on. So is Claude. Opus, at 0.06, is the most oblivious of all.

That breaks the core promise. A confidence threshold is supposed to catch the cases a human would struggle with, and here it doesn't. If you use either model as a gatekeeper, test its probabilities on your own data first.

## Finding 3: the real gap is the price tag

If the judging is a tie, the bill is not. A full benchmark run cost 10 cents on Jev. The same run cost \$5 on Haiku, \$11.50 on Sonnet and \$23.30 on Opus. That's up to 230 times more.

Speed tells the same story. Jev answers in 98 ms; Claude takes 2.2 to 3.7 seconds. That's the difference between a check you can run on every single request and one you save for a nightly batch job.

So stop calling Jev a better judge. Call it what it is: a judge as good as Claude, for pennies, in a tenth of a second. For high volume grading, routing and guardrails, that's the whole ballgame.

## Before you quote me

- **Claude states its probabilities.** The Claude models wrote a probability for every option in structured JSON, while Jev returns them natively. Reading probabilities from token logprobs might give different results.
- **Sonnet and Opus ran at low effort.** Higher effort may improve their quality, at even higher cost.
- **One dataset, one task.** HelpSteer2 is English, and it's about grading AI responses. Jev may compare differently on classification or routing.
- **The ceiling isn't 100.** HelpSteer2's official label is a rounded average of the votes, so sometimes no annotator picked it. That's why even the humans score 82.6% accuracy.

## So which one should you use?

Jev is still the winner for decision making as it's much cheaper.

Just don't trust either one's confidence score to tell you when to call a human.

Everything is open and reproducible. The Jev run costs 10 cents, and adding a model is one adapter file plus a pull request. Laya and SemIf are up next. Got a model you want tested? [Open an issue](https://github.com/trust123500/DoubtBench) and I'll run it.
