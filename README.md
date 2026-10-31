# ProofStack

Turn a portfolio into a coding assessment—and show the evidence behind the score.

**[Try the live demo](https://proofstack-zuyt.onrender.com/)** · **[Demo script](DEMO.md)** · **[Technical details](PROOFSTACK.md)**

Upload a JSON or text-based PDF portfolio, review your projects and skills, and receive six coding questions across three levels. Solve them in the browser, run tests, and use guarded hints at the higher levels. The final report connects claimed skills to actual results and lets you replay saved code changes.

## What makes it different

- **Your claim → your evidence:** shows what was tested and labels unsupported claims “Not tested.”
- **Solution replay:** inspect saved code and changes at test, hint and submission checkpoints.
- **Tests decide correctness:** AI helps personalize tasks and provide hints; executable checks decide marks.
- **Validated questions:** reference solutions must pass before a task is released. Starter code cannot earn free marks.

## Run locally

Requires Docker Desktop (running), Node.js 22+, Python 3.11+ and `uv`.

```sh
git clone https://github.com/adityakrishna183/proofstack.git
cd proofstack
# If this checkout contains source.tar.gz instead of apps/, extract it first:
# tar -xzf source.tar.gz
cp .env.example .env
# Optional: add your own FEATHERLESS_API_KEY to .env for AI generation and hints.
bash scripts/start-local.sh
```

Open http://127.0.0.1:3001. First startup installs dependencies, builds the isolated Python runner and creates the local database. Ctrl+C stops the app processes and preserves the database. Do not start another copy while these ports are occupied.

Without an API key, use the manual/sample/JSON flow and validated template questions. AI hints and AI PDF extraction require a working Featherless key. Never commit `.env` or share keys in a submission.

## Judge walkthrough

1. Use the sample portfolio or upload JSON/PDF; review extracted information.
2. Create an assessment and wait for its validation. The timer has not started.
3. Read the rules: 60 minutes, six questions, and a one-time 40-point deduction if the assessment page becomes hidden after accepting the rule.
4. Start, select a question, edit its files, and run public checks.
5. Try a hint in Medium/Hard; Easy has no assistant.
6. Finish and inspect the score, claim evidence, replay and downloadable report.

Free hosting can take time to wake up. New AI generation can take around a minute or longer; repeated portfolios can reuse validated questions. This is not a guaranteed 5–10 second service.

## Stack

Next.js / React / Monaco → FastAPI → PostgreSQL. Featherless supplies task personalization and guarded hints. Local execution uses isolated Docker; the hosted demo uses a bounded Wasmtime/WASI Python runtime. Grading uses deterministic tests.

## Scope and limits

Python tasks only; arrays, hash maps and strings. Web/ML/Security/Backend are project contexts, not separate full application runtimes. Difficulty levels are fixed. Text PDF only (5 MB, 10 pages); scanned PDFs are unsupported. Visibility events can be bypassed and do not prove cheating. Session links grant access to that session, so treat them as private. This is a hackathon prototype, not a production hiring service.

## Tests

```sh
cd apps/api
FEATHERLESS_API_KEY='' .venv/bin/python -m pytest tests/test_portfolio_hardening.py tests/test_portfolio.py tests/test_portfolio_pdf.py tests/test_featherless_provider.py -q
cd ../web
npm run build
```

## Credits and license

Built on [SignalLoop](https://github.com/signalloop-ai/signalloop), base commit `31b2507`, by **Sreenivas Makam and Ritesh Dhoot**, under Apache-2.0. Retained [LICENSE](LICENSE), [NOTICE](NOTICE), [authors](AUTHORS.md), and [third-party notices](THIRD_PARTY_NOTICES.md). Original project documentation is preserved in [the upstream README](UPSTREAM_README.md).

SignalLoop supplied the Next/Monaco/FastAPI/Postgres foundation, assessment infrastructure and assistant policy/provider foundation. Our fork adds the portfolio-first flow, Featherless integration, bounded DSA questions, validation gates, shared timer, starter-credit correction, simpler question navigation, PDF intake, claim evidence, replay, usage caps and WASI deployment.

Disclose this foundation and AI-assisted development in your submission. The organisers must allow pre-existing open-source foundations; this repository does not establish eligibility.
