# Personal learning workspace

My working notes and notebooks for the **AI Engineering from Scratch** course.
This is my portfolio of understanding — not the course's answer key.

> Convention from AGENTS.md: **Build It, then Use It.** I write each concept
> from scratch first, then compare against the course's reference `code/` and
> against the real library (NumPy / PyTorch / etc.).

## How I work each lesson

1. **Read** `phases/NN-.../MM-.../docs/en.md` myself.
2. **Note** the key ideas in my own words → `notes/<phase>/<lesson>.md`.
3. **Code it from scratch** (no peeking at `code/`) → `notebooks/<phase>/<lesson>.ipynb`,
   using the **`AI Course (Python 3.12)`** Jupyter kernel (the project venv).
4. **Compare** against the course's reference `code/` and against NumPy/PyTorch.
5. **Do the quiz + 1–2 exercises** in the notebook.
6. **Self-test per phase**: ask the tutor to quiz me. <70% → revisit weak lessons.

Rules:
- Stuck for >15 min → ask for a hint, not the answer.
- "If I can't explain it in my own words, I didn't understand it."
- Commit notes + notebooks regularly — this is the portfolio.

## Layout

```
personal/
├── notes/         # markdown summaries, my own words
│   └── 01-math-foundations/
│       └── 05-chain-rule-and-autodiff.md   # example
└── notebooks/     # jupyter notebooks, my from-scratch code
    └── 01-math-foundations/
        └── 05-chain-rule-and-autodiff.ipynb
```

Notebook kernel: **AI Course (Python 3.12)** (the `.venv` with CPU-only torch).
