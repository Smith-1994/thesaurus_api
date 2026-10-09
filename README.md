# Thesaurus API

A small Flask service that looks up word definitions from a local glossary and returns them as JSON. It is a portfolio project that shows how to serve a versioned HTTP API from a CSV dataset, with a landing page that documents how to call it.

The glossary contains about 66,000 environmental and technical terms (`word`, `definition`). The API loads that file once at startup and answers exact word lookups.

## Tech stack

- Python 3
- [Flask](https://flask.palletsprojects.com/) for routing and the landing page
- [pandas](https://pandas.pydata.org/) for loading and querying `dictionary.csv`

## Features

- `GET /` renders a short HTML page that describes the API URL format
- `GET /api/v1/<word>` returns the matched word and its definition as JSON
- Definitions are read from `dictionary.csv` in memory, so lookups do not need a database

## API

Base URL when running locally: `http://127.0.0.1:5000`

### Look up a word

```
GET /api/v1/<word>
```

The path segment is matched exactly against the `word` column, including case. Encode spaces in multi-word terms (for example, `acid%20rain`).

**Example**

```bash
curl http://127.0.0.1:5000/api/v1/acidity
```

```json
{
  "word": "acidity",
  "definition": "The state of being acid that is of being capable of transferring a hydrogen ion in solution."
}
```

## Getting started

From the project directory:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install flask pandas
python main.py
```

On Windows, activate the virtual environment with `.venv\Scripts\activate`.

Flask starts in debug mode at [http://127.0.0.1:5000](http://127.0.0.1:5000). Open that page for the URL format, or call `/api/v1/<word>` directly.

## Project structure

```
thesaurus_api/
├── main.py           # Flask app and /api/v1/<word> route
├── dictionary.csv    # Glossary of words and definitions
├── templates/
│   └── home.html     # Landing page
└── README.md
```

## Dataset

`dictionary.csv` has two columns:

| Column | Description |
| --- | --- |
| `word` | The lookup key |
| `definition` | The definition returned by the API |

Some definitions span more than one line inside a quoted CSV field. pandas reads those as a single string.
