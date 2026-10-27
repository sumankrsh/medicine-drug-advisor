# Medicine & Drug Interaction Advisor

A drug-information and interaction checker for common Indian and mental-health medicines. A FastAPI backend answers questions such as "What are the side effects of sertraline?" from a curated database of 34 medicines, and a browser front end lets you search, build a medication list and check it for interactions. The repo also includes an experimental RAG version built with LangChain, FAISS and OpenAI.

![Python](https://img.shields.io/badge/python-3.8--3.11-blue.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-0.104.1-green.svg)
![License](https://img.shields.io/badge/license-MIT-blue.svg)

> **Medical disclaimer:** this is a learning project, not medical advice. Always check with a doctor or pharmacist before starting, stopping or combining medicines.

## What it covers

**34 medicines** in `backend/comprehensive_drug_database.py`, each with Indian and international brand names, uses, side effects, dosage, warnings, known interactions, an approximate price range in INR, and whether it needs a prescription or is a controlled substance.

- **20 mental-health medicines:** antidepressants, anti-anxiety medicines, antipsychotics, mood stabilisers, and ADHD and wakefulness medicines
- **12 common medicines:** pain and fever, antibiotics, diabetes, blood pressure, cholesterol, acidity, allergy and asthma, steroids and vitamin D
- **2 Ayurvedic options:** ashwagandha and triphala

## Features

- **Ask a question:** side effects, uses or dosage for a medicine, by generic or brand name
- **Interaction checker:** check two medicines against the interactions listed in the database
- **My medications:** build a list in the browser and check all pairs at once
- **Browse:** medicines by category, by health condition, or the full list
- **Mental-health view:** all mental-health medicines grouped by type

Not built yet: the **Pill identifier**, **Compare** and **voice search** buttons appear in the front end but aren't wired up.

## Quick start

You need Python 3.8 to 3.11. Some versions pinned in `requirements.txt` have no builds for Python 3.12 or later.

**1. Get the code**

```bash
git clone https://github.com/sumankrsh/medicine-drug-advisor.git
cd medicine-drug-advisor
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

**2. Start the API**

The default API only needs three packages:

```bash
cd backend
pip install fastapi==0.104.1 uvicorn==0.24.0 pydantic==2.4.2
python enhanced_main.py
```

It runs at http://localhost:8000, with interactive API docs at http://localhost:8000/docs.

**3. Open the front end**

With the API running, open `frontend/enhanced-index.html` in your browser (double-click it, or run `open frontend/enhanced-index.html` on a Mac from the repo root). There's nothing to install or build.

## API endpoints

| Method | Path | What it does |
|---|---|---|
| `POST` | `/query` | Answer a plain-English question about a medicine |
| `POST` | `/check-interaction` | Check two medicines for listed interactions |
| `GET` | `/drug/{drug_name}` | Full details for one medicine |
| `GET` | `/mental-health-drugs` | Mental-health medicines grouped by type |
| `GET` | `/search/{category}` | Medicines in a category, such as `antidepressant` |
| `GET` | `/all-drugs` | Every medicine in the database |

Examples:

```bash
# Ask about a medicine
curl -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{"query": "What are the side effects of sertraline?"}'

# Check an interaction
curl -X POST http://localhost:8000/check-interaction \
  -H "Content-Type: application/json" \
  -d '{"drug1": "sertraline", "drug2": "ibuprofen"}'
```

## Sample medicines

**Mental health**

- Antidepressants: sertraline (Zoloft), fluoxetine (Prozac), escitalopram (Lexapro), bupropion, mirtazapine, venlafaxine, duloxetine
- Anti-anxiety: alprazolam (Xanax), clonazepam (Klonopin), lorazepam (Ativan)
- Antipsychotics: quetiapine (Seroquel), olanzapine (Zyprexa), risperidone (Risperdal), aripiprazole (Abilify)
- Mood stabilisers: lithium, valproate (Depakote), carbamazepine (Tegretol)
- ADHD and wakefulness: methylphenidate (Ritalin), atomoxetine (Strattera), modafinil (Modalert)

**Common Indian brands**

- Pain and fever: Crocin, Dolo 650 (paracetamol); Brufen, Combiflam (ibuprofen)
- Antibiotics: Azithral (azithromycin), Novamox (amoxicillin)
- Diabetes, blood pressure, cholesterol: Glycomet (metformin), Amlong (amlodipine), Atorva (atorvastatin)
- Acidity and allergy: Omez (omeprazole), Cetzine (cetirizine), Montair (montelukast)

## Project structure

```
medicine-drug-advisor/
├── backend/
│   ├── enhanced_main.py                # Default API (FastAPI, in-memory database)
│   ├── comprehensive_drug_database.py  # The 34-medicine database
│   ├── main.py                         # Experimental RAG API
│   ├── rag_pipeline.py                 # RAG pipeline (LangChain + OpenAI)
│   ├── vector_db.py                    # FAISS vector store with sentence-transformers
│   ├── data_fetcher.py                 # openFDA and RxNorm client
│   ├── indian_data_fetcher.py          # Helpers for Indian medicine datasets
│   ├── test_main.py                    # Demo-mode API with canned answers
│   ├── requirements.txt
│   └── .env.example
├── frontend/
│   ├── enhanced-index.html             # Main UI
│   └── index.html                      # Earlier, simpler UI
├── scripts/
│   └── initialize_db.py                # Builds the vector store for the RAG API
├── DEMO.md                             # Example queries to try
└── LICENSE
```

## Experimental RAG version

`backend/main.py` is a separate API that retrieves drug-label text with FAISS and generates answers with an OpenAI model through LangChain. To try it:

```bash
cd backend
pip install -r requirements.txt
cp .env.example .env               # then add your OpenAI API key to .env
python ../scripts/initialize_db.py # builds the vector store
python main.py
```

## Built with

- **Backend:** Python, FastAPI
- **RAG version:** LangChain, FAISS, sentence-transformers, OpenAI, openFDA and RxNorm
- **Front end:** HTML, JavaScript, Tailwind CSS

## Contributing

Issues and pull requests are welcome. Please open an issue first for larger changes.

## License

MIT. See [LICENSE](LICENSE).

## Contact

Built by Suman Mukherjee. Questions or ideas: [contactme@mukherjeesuman.com](mailto:contactme@mukherjeesuman.com) · [mukherjeesuman.com](https://www.mukherjeesuman.com)
