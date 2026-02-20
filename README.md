# Natural Language to SQL with LangChain

Query a relational database using plain English. This project connects a SQLite database to a large language model (LLM) via LangChain, allowing users to ask questions in natural language and receive answers backed by real SQL queries.

---

## What This Project Does

Instead of writing SQL manually, you type a question like:

> *"Which country's customers spent the most?"*

The LLM converts it to SQL, runs it against the database, and returns the answer:

> *USA — $523.06*

---

## Tech Stack

| Component | Tool |
|---|---|
| Language | Python |
| LLM | Llama 3.3 70B via Groq API |
| LLM Framework | LangChain |
| Database | SQLite (Chinook) |
| Interface | Jupyter Notebook |

---

## Database

Uses the **Chinook database** — a sample dataset modeled after a digital music store with 11 tables including Artist, Album, Track, Customer, Invoice, and Employee.

---

## Project Structure

```
LangChain_SQLReasoning/
│
├── C1_SQL_Chain_OpenAPI.ipynb   # Main notebook: DB connection, chain setup, query execution
├── chinook.db                   # SQLite database (included, no download needed)
├── prompts.jsonl                # Prompt templates used for LLM chain configuration
├── .env                         # API keys (not committed)
└── requirements.txt             # Dependencies
```

---

## How It Works

**Step 1 — Connect to Database**
LangChain's `SQLDatabase` wrapper connects to the Chinook SQLite database and inspects the schema automatically.

**Step 2 — Initialize LLM**
Groq's Llama 3.3 70B is used as the reasoning engine. OpenAI GPT-3.5 is also supported by swapping one line.

**Step 3 — Build the Chain**
`SQLDatabaseChain` links the LLM to the database. When a question is asked, the LLM generates a SQL query, executes it, and interprets the result.

**Step 4 — Ask Questions**
```python
result = sql_db_chain.invoke("Which country's customers spent the most?")
# Returns: USA — $523.06
```

---

## Known Limitation

`SQLDatabaseChain` can fail when the LLM wraps its SQL output in markdown code fences. This is a known upstream issue. The fix is migrating to `create_sql_agent`, which handles output parsing more robustly. This upgrade is planned in the next version of this project along with a Streamlit UI.

---

## Setup

```bash
git clone https://github.com/prasad11s/text-to-sql-langchain.git
cd LangChain_SQLReasoning
pip install -r requirements.txt
```

Create a `.env` file:
```
GROQ_API_KEY=your_groq_api_key
OPENAI_API_KEY=your_openai_api_key  # optional
```

The `chinook.db` file is already included in this repository — no separate download needed.

---

## Roadmap

- [ ] Migrate from `SQLDatabaseChain` to `create_sql_agent` for better reliability
- [ ] Build Streamlit UI for interactive natural language querying
- [ ] Deploy on Streamlit Cloud with live demo link
- [ ] Support PostgreSQL and MySQL connections

---

## Author

**Prasad Shimpi**  
MS Applied Data Science, Syracuse University  
[LinkedIn](https://linkedin.com/in/prasadshimpi) | [GitHub](https://github.com/prasad11s)
