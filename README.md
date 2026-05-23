# api-integration-evaluator

This is basically a toolbox for checking AI-generated API integration plans. The kind of stuff that comes up when you're trying to sync CRMs, marketing tools, and databases, and you need to make sure the AI isn't hallucinating steps or ignoring error handling.

I built this to practice for a job that involves training and evaluating AI systems on real API integration scenarios,REST APIs, webhooks, data mapping, the works.

## What's inside

The main thing is a Colab notebook (`api_integration_evaluator.ipynb`) that:

- Loads the `argilla/Synth-APIGen-v0.1` dataset (synthetic API call traces)
- Extracts ground-truth sequences from the data
- Generates a "weak AI plan", intentionally flawed, missing error handling, wrong parameters
- Evaluates those plans against the ground truth (step count, tool names, parameters, error handling, idempotency)
- Handles edge cases like empty sequences (sometimes the right plan is to do nothing)
- Includes payload improvement logic (cleans emails, normalizes phone numbers, validates against schemas)
- Designs a production-ready workflow with retries, dead-letter queue, idempotency keys, and data validation

There's also batch evaluation — I ran it on 50 samples and got a 0% average score, which is expected because the weak AI plan is intentionally broken. The point is that the evaluator works.

## How to run it

1. Open the Colab notebook: [https://colab.research.google.com/drive/1S3HRGTZ754kRhpb0BY3hIc4pifDhiefa](https://colab.research.google.com/drive/1S3HRGTZ754kRhpb0BY3hIc4pifDhiefa)
2. Run the cells in order.
3. You'll need a Hugging Face token for the API calls (if you want to test a real model). The notebook uses Colab secrets for that, so no hardcoded keys.

If the Hugging Face API is flaky (it was for me), the evaluation still works with the simulated weak AI plan. The logic is what matters.

## Results I got

After running batch evaluation on 50 samples:
Average score: 0.0%
Pass rate (>=75%): 0%

That's not a bug; it's because the weak AI plan I'm testing against is deliberately bad (no error handling, wrong tools, missing parameters). The evaluator catches all of it. A real model would score higher.

## Limitations

- The dataset is synthetic, but it's structured enough to test evaluation logic
- I didn't end up using a real LLM API due to connectivity issues, but the evaluation framework is model-agnostic — you can plug in any AI output and it'll score it
- The payload improvement logic is basic, but it shows the pattern

## Why I built this

There's a job out there for training and evaluating AI systems on API integrations — generating prompts, reviewing integration plans, fixing payloads, ensuring data sync. I wanted to prove I could do that work. This notebook is the proof. It's not a finished product, but it's a solid demonstration of the skills.

## Links

- GitHub repo: [https://github.com/AkpanDaniel/api-integration-evaluator](https://github.com/AkpanDaniel/api-integration-evaluator)
- Colab notebook: [https://colab.research.google.com/drive/1S3HRGTZ754kRhpb0BY3hIc4pifDhiefa](https://colab.research.google.com/drive/1S3HRGTZ754kRhpb0BY3hIc4pifDhiefa)

## License

MIT
