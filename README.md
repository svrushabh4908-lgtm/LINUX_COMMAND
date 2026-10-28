# ProofFind --- Evidence-based document question answering

> `proof-find` --- searches an uploaded PDF or TXT document for passages
> that support an answer, then shows the evidence so the user can verify
> it.

**Team:** Vrushabh Sonawane, Yash Aryaa.  
**Repository:** Add the project GitHub URL when available  
**Date:** 28-10-2026

------------------------------------------------------------------------

## 1. The problem

ProofFind focuses on a repeated task: searching long documents to find a
specific answer, date, rule, or requirement.

### What we keep doing by hand

For this project, the problem is the effort required to locate a specific fact in a long PDF or text document. The following workflow describes the common manual approach that ProofFind is designed to simplify; it is not a measured user-study result.

A typical manual workflow may look like this:

1.  Open a PDF or notice.
2.  Use the PDF reader's search feature or scroll through pages.
3.  Try different keywords if the first search does not find the answer.
4.  Read surrounding paragraphs to understand the context.
5.  Copy the relevant passage into notes or a message.
6.  Repeat for the next question or document.

``` text
# Example manual workflow — replace with your real workflow
Open college_notice.pdf
Search for "deadline"
Read the matching paragraph
Check the surrounding context
Copy the answer and page number
```

### Evidence and problem validation

The project addresses a plausible document-search problem, but the team has not provided measured timings, screenshots, or user-interview results in this README. These should be collected during testing rather than estimated.

A practical validation trial should record the document used, the exact question, the time taken manually, the number of searches or steps, and evidence such as notes or screenshots. Until those observations are collected, the frequency of the problem and the time cost remain unmeasured.

### Who else has this problem

Potential users include students searching assignment instructions, applicants checking notices, and staff looking up rules in long documents. These are intended user groups, not confirmed interview findings. Direct feedback from classmates, faculty members, or other document users is still pending.

------------------------------------------------------------------------

## 2. Prior art --- what already exists

> Do not claim that no similar tools exist. Compare ProofFind with
> existing approaches and record what you actually tested.

### Tools we found and actually tried

**Complete this table after trying the tools.** A link alone is not
proof that a tool was tested.

  ------------------------------------------------------------------------
  Tool / approach    Where we found it What it does      What was missing
                                                         in our test case
  ------------------ ----------------- ----------------- -----------------
  PDF reader search  \[Name/link\]     Searches for      \[Record the
                                       matching words in actual limitation
                                       a PDF             you observed\]

  Operating-system   \[Name/link\]     Finds files or    \[Record the
  or desktop                           text, depending   actual limitation
  document search                      on the tool       you observed\]

  Existing PDF chat  \[Name/link\]     Lets users ask    \[Record the
  / document Q&A                       questions about   actual limitation
  tool                                 documents         you observed\]
  ------------------------------------------------------------------------

### Evidence of trying them

For at least one tool, record the exact question/document used and what
happened.

``` text
Tool tested: [Name and version, if available]
Document: [Document name or safe sample]
Question: [Question asked]
Observed result: [Paste actual result or summarize it]
Limitation observed: [Specific issue, if any]
```

> Do not invent terminal output or claim a tool failed unless you
> reproduced the failure.

### Where we searched

Mark each item only after you have actually searched it.

-   [ ] Search engine queries for PDF question-answering and document
    search
-   [ ] GitHub search for document Q&A / PDF search projects
-   [ ] PyPI for Python document-search packages
-   [ ] Streamlit community apps or examples
-   [ ] Asked a human: \[who, and what they said\]
-   [ ] Tested an existing PDF reader's built-in search
-   [ ] Tested at least one existing document question-answering tool

### The gap, in one sentence

Complete this after your comparison:

> "Existing tools can do \_\_\_\_\_\_, but in our tested use case they
> do not \_\_\_\_\_\_, which is the part ProofFind is intended to
> improve."

Possible project focus to investigate---not a verified claim about all
existing tools: **a simple student-focused workflow that displays the
exact supporting passage and clearly says when the uploaded document
does not contain enough evidence.**

------------------------------------------------------------------------

## 3. What we are building

### In scope

-   Upload one readable PDF or TXT document.
-   Extract text and split long content into smaller passages.
-   Retrieve passages relevant to a user's question.
-   Display the answer together with the supporting passage and page
    information when available.
-   Show an "insufficient evidence" response when the source does not
    support an answer.

### Out of scope

-   Guaranteeing that every answer is correct.
-   Giving legal, medical, financial, or other professional advice.
-   Reading every scanned PDF image in the first version; OCR can be
    added later.
-   Searching the entire internet or private drives.
-   Claiming that an answer is verified merely because an AI model
    generated it.

### How we will know it worked

> "ProofFind is successful if, on a prepared test set of \_\_\_
> questions across \_\_\_ sample documents, it retrieves useful
> supporting passages for \_\_\_ questions, displays evidence for every
> answer it gives, and abstains on questions whose answers are absent
> from the source."

Fill the blanks only after running and recording the tests.

------------------------------------------------------------------------

## 4. Tech stack

  -------------------------------------------------------------------------
  Choice                  What we picked            Why, and what we
                                                    rejected
  ----------------------- ------------------------- -----------------------
  Language                Python 3                  Suitable for a
                                                    beginner-friendly
                                                    prototype and has
                                                    document-processing and
                                                    retrieval libraries.

  User interface          Streamlit                 Quickly creates a local
                                                    interactive interface
                                                    without requiring a
                                                    separate frontend
                                                    framework.

  PDF extraction          `pypdf`                   A lightweight starting
                                                    point for text-based
                                                    PDFs; scanned PDFs may
                                                    need OCR.

  TXT extraction          Python file handling      No extra library is
                                                    needed for basic text
                                                    files.

  Initial retrieval       TF-IDF with               Easier to inspect and
                          `scikit-learn` or simple  debug than a vector
                          keyword search            database for the first
                                                    version.

  Optional semantic       `sentence-transformers`   Can retrieve passages
  retrieval                                         with related meaning
                                                    even when the wording
                                                    differs.

  Optional vector index   FAISS or Chroma           Useful if the project
                                                    grows to a larger
                                                    collection of embedded
                                                    passages.

  Optional answer         LLM API                   Can format retrieved
  generation                                        evidence into natural
                                                    language, but adds API,
                                                    privacy, and cost
                                                    considerations.

  Testing                 `unittest` or `pytest`    Makes extraction,
                                                    retrieval, and
                                                    missing-evidence
                                                    behavior repeatable.

  Version control         Git and GitHub            Tracks changes and
                                                    makes the project
                                                    easier to share and
                                                    review.
  -------------------------------------------------------------------------

### Why this language

Python is the main choice because the project is a document-processing
and retrieval application, and Python has accessible libraries for PDF
extraction, text processing, testing, and a simple user interface. It is
also a practical choice for a student who is already learning Python.
This is a project-fit decision, not a claim that Python is best for
every application.

### What we are deliberately not using

The first version will not require a complex frontend/backend split, a
paid LLM, or a vector database. Start with extraction, chunking,
retrieval, and evidence display; add semantic search or an LLM only when
the baseline works and testing shows a reason to improve it.

------------------------------------------------------------------------

## 5. Architecture

The first version is a small pipeline: load a document, extract its
text, prepare searchable passages, retrieve evidence for a question, and
display a grounded result.

``` mermaid
flowchart TD
    A["User uploads PDF / TXT"] --> B["Validate file"]
    B --> C["Extract text"]
    C --> D["Split into passages"]
    D --> E["Build searchable index"]
    Q["User asks a question"] --> R["Retrieve relevant passages"]
    E --> R
    R --> S{"Enough relevant evidence?"}
    S -->|Yes| T["Prepare answer from evidence"]
    S -->|No| U["Return insufficient-evidence message"]
    T --> V["Display answer and source passage"]
    U --> V
```

### Components

  -----------------------------------------------------------------------
  Part                    Responsibility          Suggested file
  ----------------------- ----------------------- -----------------------
  User interface          Upload documents,       `app.py`
                          accept questions, show  
                          results                 

  Document loader         Validate files and      `document_loader.py`
                          extract PDF/TXT text    

  Chunker                 Split extracted text    `chunker.py`
                          into passages and       
                          retain source metadata  

  Retriever               Rank passages using     `retriever.py`
                          keyword or TF-IDF       
                          search                  

  Answer formatter        Present a short         `answerer.py`
                          response based on       
                          selected evidence       

  Tests                   Check extraction,       `tests/`
                          retrieval, and fallback 
                          behavior                
  -----------------------------------------------------------------------

### Data flow

The input is a user-uploaded PDF or TXT file and a natural-language
question. ProofFind extracts the document text, divides it into
passages, ranks those passages against the question, and checks whether
relevant evidence was found. The output is a concise response with the
supporting passage and page information when available---or a clear
insufficient-evidence message.

### What happens when things go wrong

  -----------------------------------------------------------------------
  If this happens                     The tool should do this
  ----------------------------------- -----------------------------------
  Unsupported file type               Reject the upload and explain which
                                      formats are supported.

  Empty or unreadable document        Show a clear error and ask the user
                                      to try another file.

  Scanned PDF with no extractable     Explain that OCR may be needed; do
  text                                not pretend the document was read.

  No relevant passage found           Say the document does not provide
                                      enough evidence to answer.

  Retrieval finds a weak or unrelated Avoid presenting it as a certain
  passage                             answer; show a fallback where
                                      appropriate.

  Optional LLM API is unavailable     Show a useful error or fall back to
                                      displaying retrieved evidence if
                                      implemented.
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 6. Usage

> This section defines the intended user interface. The commands below
> are setup/run commands; application features should be demonstrated
> only after they are implemented.

### Synopsis

``` text
streamlit run app.py
```

### Main user actions

| Action | What it does | Expected behavior |
|---|---|---|
| Upload a file | Select a PDF or TXT document | The app validates the file and attempts text extraction |
| Ask a question | Enter a question about the uploaded document | The app searches the extracted passages |
| View evidence | Inspect the passage associated with the answer | The supporting source text is shown, with page information when available |
| Ask an unsupported question | Ask for information absent from the document | The app reports that there is insufficient evidence to answer |

### Examples

**Question with supporting evidence**

``` text
Question: What is the assignment deadline?
```

Expected behavior for a sample document containing "Assignments must be
submitted by 15 November":

``` text
Answer: The assignment deadline is 15 November.

Evidence: “Assignments must be submitted by 15 November.”
Source: [Page number, if available]
```

**Question not answered by the document**

``` text
Question: What is the penalty for late submission?
```

If the document contains no information about a penalty, expected
behavior is:

``` text
I couldn't find enough evidence to answer that from this document.
```

**Unreadable file**

``` text
Expected behavior:
The app explains that it could not extract readable text and suggests trying a text-based PDF or TXT file.
```

> The outputs above are illustrative examples, not captured output from
> an implemented application.

### Exit codes

For the Streamlit MVP, normal application behavior is handled through
the UI rather than a custom command-line interface. If a separate CLI is
added later, document its exit codes and test them.

### Installation

Create and activate an environment on macOS:

``` bash
mkdir proof-find
cd proof-find
python3 -m venv .venv
source .venv/bin/activate
python -m pip install streamlit pypdf scikit-learn
```

After creating `app.py` and the supporting modules, run:

``` bash
streamlit run app.py
```

To record the installed dependencies:

``` bash
python -m pip freeze > requirements.txt
```

These commands install the initial dependencies; they do not create the
application code automatically.

------------------------------------------------------------------------

## 7. References

> Add the sources you actually read or tested. For each one, write what
> you used it for. Do not list a source as tested if you only opened its
> webpage.

-   [Streamlit documentation](https://docs.streamlit.io/) --- reference
    for building the application interface.
-   [pypdf documentation](https://pypdf.readthedocs.io/) --- reference
    for extracting text from PDFs.
-   [scikit-learn feature extraction
    documentation](https://scikit-learn.org/stable/modules/feature_extraction.html)
    --- reference for TF-IDF and text features.
-   [Sentence Transformers documentation](https://www.sbert.net/) ---
    optional reference for semantic embeddings.
-   [FAISS documentation](https://faiss.ai/) --- optional reference for
    vector similarity search.

------------------------------------------------------------------------

## Project status

**Status:** Project specification and implementation plan.

This README documents ProofFind's problem, proposed scope, architecture, setup approach, expected behavior, and evaluation plan. The provided source document does not establish that the application has been implemented or tested, so example outputs and expected behaviors are clearly identified as illustrative. Update this section with verified implementation details, screenshots, and test results once available.

## Team

- **Vrushabh Sonawane**
- **Yash Aryaa**

## Future improvements

- Add OCR support for scanned PDFs.
- Evaluate semantic retrieval if keyword/TF-IDF retrieval is not sufficient.
- Add optional LLM-based answer phrasing while keeping source evidence visible.
- Expand automated tests for empty files, unsupported formats, missing answers, and weak matches.
- Record measured results from a small, repeatable evaluation set.
