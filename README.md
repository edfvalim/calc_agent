# Calculator + FX Agent

A simple CLI assistant (OpenAI Agents SDK) that:

* Evaluates **single-line Python math expressions**
* Performs **live currency conversion** (170+ currencies)
* Understands currency **symbols and names** (“\$”, “euros”, “reais”, “R\$”, etc.)
* Can **combine math + FX** in one prompt (e.g., “what’s 234\*234 dollars in euros?”)

## Quickstart

### 1) Install

Using **uv** (recommended):

```bash
uv sync
```

or plain pip:

```bash
pip install openai-agents python-dotenv
```

### 2) Configure

Create a `.env` file:

```
OPENAI_API_KEY=your_key_here
```

### 3) Run

```bash
uv run chat.py
```
# or
```bash
python chat.py
```

Type `/bye` to exit.

## Using it

Ask questions in plain language:

```text
You: What's 15% of 380?
Agent: 57.0

You: Convert 100 USD to EUR
Agent: 100.00 USD = 92.45 EUR (rate 0.924500, updated 2025-09-13 00:01:02 UTC)

You: How much is 234×234 dollars in euros?
Agent: 54,756.00 USD = 50,628.30 EUR (rate 0.924500, updated 2025-09-13 00:01:02 UTC)

You: convert 2500 reais to dollars
Agent: 2,500.00 BRL = 435.22 USD (rate 0.174088, updated Fri, 13 Sep 2025 00:01:02 UTC)
```

It understands common symbols and names (e.g., \$, R\$, €, “dollars”, “reais”, “euros”) and normalizes them to standard ISO codes behind the scenes.

## What actually happens

The agent uses the OpenAI Agents SDK to route your request:

* If it’s a regular question with no math or money, it answers directly.
* If it’s a math question, it composes a safe single-expression calculation internally and evaluates it.
* If it’s a currency question, it requests live FX data from `open.er-api.com` (free tier API, no need for key) and returns a one-line result with the rate and timestamp.
* If it’s mixed (“… dollars in euros?” with arithmetic), it first computes the number, then converts the result.


## What’s inside

* **OpenAI Agents SDK** decides when to answer directly vs. call a tool.
* **Tools**

  * `calc(expr: str) -> str`
    Safe, single-expression `eval` with a **restricted sandbox** (no `__`, no `import`).
    Allowed: `math`, `cmath`, `abs`, `round`, `min`, `max`, `sum`, `pow`, `divmod`, `int`, `float`, `complex`, `range`, `Decimal`, `Fraction`.
  * `convert_currency(amount, from_code, to_code) -> str`
    Fetches live rates from **open.er-api.com** (free tier api) and prints a one-line summary with rate + last update time.
* **Currency normalization** handles common symbols/names → ISO codes (`USD`, `EUR`, `BRL`, `GBP`, `JPY`, `CNY`, `AUD`, `CAD`, `CHF`, `ARS`, `MXN`…).


