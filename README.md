# Meal Recipe Search Tool

Course project artifact for ITEC 590, Assignment 5: wiring one real tool to an
agent. The agent can search the public TheMealDB catalog by recipe name. It
uses recipe search only for candidate discovery; it does **not** make nutrition,
allergen, or dietary-safety claims.

The main submission is the executed notebook:
[notebooks/assignment5_meal_recipe_tool.ipynb](notebooks/assignment5_meal_recipe_tool.ipynb).
The matching PDF submission is in [submission/](submission/).

## What the tool does

```text
User: “Find recipe candidates named Arrabiata.”
        ↓
LLM chooses search_recipes_by_name("Arrabiata")
        ↓
TheMealDB public REST API
        ↓
Reduced result: recipe ID, name, category, cuisine, ingredients
```

The implementation calls:

```text
GET https://www.themealdb.com/api/json/v1/1/search.php?s={recipe_name}
```

It accepts one non-empty recipe-name string. It imposes a timeout, handles
HTTP/network/JSON failures by returning an `{"error": ...}` object, and limits
the response to at most five small recipe records.

## Progress snapshot — audited 2026-10-08

### Assignment 5 requirements

| Requirement | Status | Evidence |
| --- | --- | --- |
| Part A: at least three capstone tools with purpose, input/output, service, access, read/write, authentication, and response details | Complete | Four tool entries appear in the first notebook section. |
| One real public tool | Complete | `search_recipes_by_name` calls TheMealDB's live recipe-search endpoint. |
| Clear typed wrapper, docstring, response shaping, timeout, and safe error return | Complete | Part B, Steps 2–4. |
| Mark the tool read or write; add a checkpoint only if it writes | Complete | The tool is read-only; `WRITE_TOOLS` remains empty. |
| Register the tool for the model | Complete | `RECIPE_SEARCH_SCHEMA` is added to `TOOLS`. |
| Direct normal-input test | Complete | Recorded `Arrabiata` result is visible. |
| Direct bad-input/no-result test | Complete | Recorded error result is visible. |
| LLM test that should call the tool | Complete | Recorded `CALL`, `RESULT`, and `FINAL` trace is visible. |
| LLM test that should not call the tool | Complete | Recorded final JSON explanation has no tool call. |
| Graduate MCP extension: `list_tools()` and `call_tool()` evidence | Complete | FastMCP server, tool schema output, and tool-call result are visible. |
| AI-use and verification note | Complete | Final notebook section. |
| Submission PDF and executed notebook | Prepared | Both are included in this repository; upload status is outside this repository. |

### Remaining work

There is no missing required implementation for Assignment 5 in the saved
notebook. Before or after submission, only these actions remain:

1. Confirm the PDF and notebook were uploaded to Blackboard; the repository
   cannot verify a course-site upload.
2. If rerunning the notebook, enter a fresh Groq key only in the hidden
   `getpass` prompt and review the outputs again before export.
3. Optional +1: run the FastMCP server as a separate process, attach it to a
   supported host such as VS Code or Claude Desktop, and capture the tool-list
   screenshot. This is not required for the base assignment.

## Proposal status

The Part A tool inventory is complete and can be reused as the **Tools and
knowledge** section of the later course project proposal. It describes four
tools: profile retrieval, recipe search, local meal listing, and deterministic
meal recommendation.

No standalone Week 4 project-proposal document was present in the original
`Assignment/` folder when this repository was made. Therefore, this repository
does not claim that the full proposal is complete. To add one later, create a
`proposal/` folder with a document covering:

- problem and intended user;
- agent task, input, output, and an example;
- why tools/agentic execution are needed;
- evaluation examples and success criteria;
- platform; and
- graduate research extension, if required.

## Run it

```bash
git clone https://github.com/TejPaladi/itec590-meal-recipe-mcp-tool.git
cd itec590-meal-recipe-mcp-tool
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Open the notebook and run from top to bottom. The initial setup cell asks for a
Groq key using `getpass` only if `GROQ_API_KEY` is not already available in the
environment. Do not paste a key into the notebook or commit a `.env` file.

The public TheMealDB call does not require a personal API key. Live LLM test
cells do require a Groq key and may be affected by provider availability or
rate limits.

## Safety and data boundary

- The public request contains only a recipe-name query, never a persona
  profile, account information, or real personal data.
- The tool does not determine whether a recipe is healthy, allergen-free, or
  appropriate for an individual's medical or dietary needs.
- A read-only tool makes no changes to TheMealDB or the local project.

## Repository layout

```text
notebooks/   executed Assignment 5 notebook, including test evidence
submission/  matching PDF export
```
