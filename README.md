# Resume Bot — Setup (PyCharm)

## 1. Project setup
1. Open PyCharm → **File > Open** → select this folder.
2. Create a virtual environment if PyCharm doesn't prompt you automatically:
   `View > Tool Windows > Python Packages`, or via terminal:
   ```
   python -m venv venv
   venv\Scripts\activate      # Windows
   source venv/bin/activate   # macOS/Linux
   ```
3. Install dependencies:
   ```
   pip install -r requirements.txt
   ```

## 2. API key    #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
Get an API key from https://console.anthropic.com/ and either:
- Set it as an environment variable `ANTHROPIC_API_KEY`, or
- Paste it directly into the sidebar field when the app runs (not saved anywhere).

## 3. Run it
PyCharm can't "Run" a Streamlit file directly with the green arrow — use the terminal:
```
streamlit run app.py
```
This opens the app in your browser at `http://localhost:8501`.

(Optional: create a PyCharm **Run Configuration** of type "Python" pointing at
the `streamlit` module, with parameters `run app.py`, so you get a Run button too.)

## 4. How it works
- **No file uploaded** → whatever you type in "Context / instructions" is treated
  as the description of the candidate, and Claude generates a resume from scratch.
- **File uploaded** (.pdf/.docx/.tex/.txt) → its text is extracted and sent to Claude
  along with your instructions, so it rewrites/tailors the existing resume.
- Claude always responds in a fixed lightweight Markdown structure internally
  (see `SYSTEM_PROMPT` in `app.py`), which is then parsed and rendered into
  whichever output format you picked: PDF (fpdf2), DOCX (python-docx), or
  TEX (raw LaTeX source you'd compile with `pdflatex`/Overleaf).
- The generated Markdown is shown in an editable text box before export, so
  you can tweak wording without regenerating.

## 5. Known limitations / things to improve next
- The Markdown→structure parser (`parse_markdown_resume`) assumes Claude follows
  the exact section format in the system prompt. If output drifts, tighten the
  prompt or add retries/validation.
- PDF export via fpdf2 is basic (no LaTeX-quality typesetting). If you want
  nicer PDFs, generate the `.tex` and compile with a local LaTeX install
  (`pdflatex resume.tex`) instead of using the fpdf2 path.
- No persistence yet — nothing is saved between runs. Add a `data/` folder +
  simple JSON/SQLite if you want history of past resumes.
- Single-user, no auth — fine for personal use, not deployment-ready as-is.
