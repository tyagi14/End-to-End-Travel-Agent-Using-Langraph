# ✈️ End-to-End Travel Agent using LangGraph

An intelligent AI travel agent built with **LangGraph** that searches for flights and hotels in real-time, then emails the results directly to you — all through a clean **Gradio** web interface.

## 🧠 Overview

This project implements a **stateful agentic AI system** using LangGraph's `StateGraph`. The agent:
1. Accepts a natural language travel query (e.g., *"Flights from NYC to London June 10–15, 4-star hotels"*)
2. Autonomously decides which tools to call and in what order
3. Searches for real flights using **Google Flights via SerpAPI**
4. Searches for real hotels using **Google Hotels via SerpAPI**
5. Returns a rich response with prices, links, and logos
6. Sends the results as a formatted **HTML email via SendGrid**

## 🏗️ Architecture

```
User Query (Gradio UI)
        │
        ▼
┌─────────────────────┐
│   call_tools_llm    │  ← GPT-4o decides which tools to call
└────────┬────────────┘
         │ tool_calls exist?
    ┌────┴────┐
   Yes        No
    │          │
    ▼          ▼
┌──────────┐  ┌─────────────┐
│invoke_   │  │email_sender │ → SendGrid HTML Email
│tools     │  └─────────────┘
└────┬─────┘
     │
     ▼
  (loops back to call_tools_llm)
```

### LangGraph Nodes

| Node | Description |
|------|-------------|
| `call_tools_llm` | GPT-4o decides which tool to call next |
| `invoke_tools` | Executes `flights_finder` or `hotels_finder` via SerpAPI |
| `email_sender` | Converts result to HTML and sends via SendGrid |

### Tools

| Tool | API Used | Description |
|------|----------|-------------|
| `flights_finder` | SerpAPI (Google Flights) | Search flights by route, date, passengers |
| `hotels_finder` | SerpAPI (Google Hotels) | Search hotels by location, dates, class |

## 🖥️ Gradio UI

The app has a two-section interface:

1. **Travel Query Section** — Enter your travel request in plain English → get flight & hotel results
2. **Email Section** — Fill in sender, receiver, subject → sends the results as a formatted HTML email

## 🚀 Getting Started

### Prerequisites

- Python >= 3.10
- OpenAI API Key
- SerpAPI Key — [serpapi.com](https://serpapi.com)
- SendGrid API Key — [sendgrid.com](https://sendgrid.com) (free tier available)
- A verified sender email in SendGrid

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/End-to-End-Travel-Agent-LangGraph.git
cd End-to-End-Travel-Agent-LangGraph

# Install dependencies
pip install -r requirements.txt
```

### Environment Variables

Create a `.env` file in the root directory:

```env
OPENAI_API_KEY=your_openai_api_key_here
SERPAPI_API_KEY=your_serpapi_key_here
SENDGRID_API_KEY=your_sendgrid_api_key_here
FROM_EMAIL=your_verified_sender@example.com
TO_EMAIL=recipient@example.com
EMAIL_SUBJECT=Travel Information
```

> ⚠️ **Never commit your `.env` file or hardcode keys in the notebook!**

### Run the Notebook

Open `End_to_End_Travel_Agent_using_Langgraph_Practice.ipynb` in Jupyter or Google Colab and run all cells.

### Launch the Gradio App

The last cell launches the Gradio interface:

```python
demo.launch(server_name="0.0.0.0", share=True)
```

A public shareable URL will be printed — open it in any browser.

## 💬 Example Queries

```
Flights from New York (JFK) to London (LHR) from June 10 to June 15 for 2 adults,
and 4-star hotels in London for the same dates.
```

```
Find me the cheapest flights from Toronto to Paris in July and hotels near the Eiffel Tower.
```

## 📦 Dependencies

| Package | Purpose |
|---------|---------|
| `langchain-core` | LangChain core abstractions |
| `langchain-openai` | GPT-4o LLM integration |
| `langgraph` | Stateful agent graph (StateGraph) |
| `serpapi` | Google Flights & Hotels search |
| `sendgrid` | HTML email delivery |
| `gradio` | Web UI for the travel agent |
| `python-dotenv` | Environment variable management |

Install all with:

```bash
pip install -r requirements.txt
```

## 📁 Project Structure

```
End-to-End-Travel-Agent-LangGraph/
│
├── End_to_End_Travel_Agent_using_Langgraph_Practice.ipynb  # Main notebook
├── requirements.txt                                          # Python dependencies
└── README.md                                                 # Project documentation
```

