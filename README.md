# Calculator + FX Agent

A simple AI assistant that performs mathematical calculations and currency conversions.

## Setup

1. Install dependencies:
```bash
uv sync
# or
pip install openai-agents python-dotenv
```

2. Create `.env` file:
```
OPENAI_API_KEY=your_key_here
```

3. Run:
```bash
uv run chat.py
```

or

```bash
python chat.py
```

## Usage

```
You: What's 15% of 380?
Agent: 57.0

You: Convert 100 USD to EUR
Agent: 100.00 USD = 92.45 EUR (rate 0.924500, updated 2025-09-13 00:01:02 UTC)

You: What's 234*234 dollars in euros?
Agent: 54,756.00 USD = 50,628.30 EUR (rate 0.924500, updated 2025-09-13 00:01:02 UTC)
```

Type `/bye` to exit.

## Features

- Mathematical calculations using Python expressions
- Real-time currency conversion for 170+ currencies
- Recognizes currency symbols ($, €, £) and names (dollars, euros, reais)
- Combines math with currency conversion

