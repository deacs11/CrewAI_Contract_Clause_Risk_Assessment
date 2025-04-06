## CrewAI Contract clause risk assessment assistant

This project implements a CrewAI agent team designed to **assist** in the preliminary review of legal contracts. The agents work together to parse contract text, classify clauses, identify potential risk patterns based on predefined criteria, spot ambiguities, and generate a structured "Review Brief" highlighting areas that may warrant closer inspection by a legal professional.

**🚨 CRITICAL DISCLAIMER 🚨**

**This tool is an AI-powered assistant ONLY. It DOES NOT provide legal advice.**

*   The analysis is automated and based on pattern matching and AI interpretation, which **can be flawed, incomplete, or inaccurate**.
*   The output **MUST** be thoroughly reviewed and validated by a **qualified human legal professional** before making any decisions or taking any action based on the contract.
*   **DO NOT rely solely on this tool for legal assessment or contract negotiation.** It is intended solely to help focus human review efforts.
*   Use of this tool is at your own risk. The creators assume no liability for errors, omissions, or consequences arising from its use.

## Features

*   **Contract parsing:** Attempts to segment contract text into distinct clauses or sections.
*   **Clause classification (optional):** Categorizes clauses by common types (e.g., Liability, Term, Payment).
*   **Risk pattern detection:** Scans clauses for predefined keywords or structures often associated with higher risk (e.g., broad indemnities, liability limitations, vague warranties).
*   **Ambiguity identification:** Looks for undefined critical terms, vague phrasing, or potentially contradictory statements.
*   **Structured reporting:** Consolidates flagged items into a "Review Brief" organized for efficient human review.

## Requirements

*   Python 3.9+
*   Google Colab Environment (Recommended for ease of use) or a local Python setup.
*   **OpenAI API Key:** Required for the language model.
    *   **GPT-4 (Turbo recommended):** Strongly advised due to the nuance required for legal text analysis. GPT-3.5 may struggle significantly.
    *   Requires a **funded** OpenAI account or active trial credits. Get a key from [platform.openai.com](https://platform.openai.com/).

## Setup (Google Colab)

1.  **Get the Notebook:** Clone the repository or download the `.ipynb` file.
2.  **Open in Colab:** Upload and open the notebook in Google Colab ([colab.research.google.com](https://colab.research.google.com/)).
3.  **Install libraries:** Run the first cell (`# @title 1. Install Necessary Libraries`) to install `crewai`, `langchain-openai`, etc.
4.  **Configure OpenAI API Key in Colab Secrets:**
    *   In Colab, click the **Key icon (`<>`)** in the left sidebar ("Secrets").
    *   Enable "Notebook access".
    *   Add **one** new secret:
        *   Name: `OPENAI_API_KEY`
        *   Value: Your OpenAI API Key (starts with `sk-...`)
    *   Ensure the **toggle switch** next to the secret is **ON**.
5.  **Run Cell 2:** Execute the second cell (`# @title 2. Import Modules...`). Check the output to confirm the `OPENAI_API_KEY` was found and loaded. Resolve any errors before continuing.

## How to Use

1.  **Input contract text (Cell 3):**
    *   Locate the cell titled `# @title 3. Define Input Contract Text...`.
    *   **Replace** the example text within the triple quotes (`"""..."""`) assigned to the `contract_text` variable with the actual contract text you want to analyze. Try to preserve formatting like numbering if possible.
    *   **Acknowledge the disclaimer** printed by this cell before proceeding.
2.  **Select LLM (Optional - Cell 4):**
    *   In Cell 4 (`# @title 4. Select LLM...`), ensure you are using an appropriate model. GPT-4 Turbo is highly recommended.
3.  **Run cells sequentially:** Execute the remaining cells (4 through 8) in order.
    *   Cell 5 defines the specialized agents for contract analysis.
    *   Cell 6 defines the sequence of tasks (Parse, Classify, Detect Risks, Identify Ambiguities, Generate Brief).
    *   **Cell 7** creates the `Crew` and starts the analysis using `kickoff()`. This step will involve significant LLM processing and API calls. Monitor the verbose output.
    *   **Cell 8** displays the final generated "Review Brief". **Remember to read the final disclaimer printed by this cell as well.**

## Workflow overview

The crew processes the contract text through these stages:

1.  **Contract parser agent (Task 1):** Segments the raw text into potential clauses.
2.  **Clause classifier agent (Task 2):** Assigns a category to each clause (optional step, aids focus).
3.  **Risk pattern detector agent (Task 3):** Flags clauses matching predefined risk patterns.
4.  **Ambiguity identifier agent (Task 4):** Flags clauses with unclear or vague language.
5.  **Review brief generator agent (Task 5):** Compiles all flagged items from Tasks 3 & 4 into the final report.

## Customization

*   **Risk patterns:** Modify the `goal` description of the `risk_pattern_detector` agent (Cell 5) and the corresponding `task_detect_risks` description (Cell 6) to add, remove, or refine the specific risk patterns you want to flag.
*   **Ambiguity criteria:** Adjust the prompts for the `ambiguity_identifier` agent and task to focus on different types of clarity issues.
*   **Input format:** For real-world use, you would likely need to add tools (e.g., using libraries like `pypdf` or `python-docx`) and modify the `contract_parser` agent/task to handle PDF or DOCX files instead of just plain text pasted into the notebook.
*   **Clause classification:** You could remove the `clause_classifier` agent and `task_classify_clauses` if desired, simplifying the flow but potentially making the risk/ambiguity detection slightly less focused. Adjust subsequent task contexts accordingly.

## Limitations & critical disclaimer (Reiteration)

*   **THIS IS NOT LEGAL ADVICE.** Output is AI-generated and requires expert human review.
*   **Accuracy is not guaranteed:** LLMs can misinterpret legal language, miss nuances, or "hallucinate" findings. Parsing of complex formatting may fail.
*   **Limited scope:** The default risk patterns are examples; they are not exhaustive and may not cover all relevant risks for a specific contract type or jurisdiction.
*   **Context ignored:** The tool analyzes text in isolation and lacks understanding of the business context, negotiation history, or applicable laws beyond what's in the text and the LLM's general knowledge.
*   **API costs:** Using powerful models like GPT-4 can be expensive. Monitor your OpenAI usage.
