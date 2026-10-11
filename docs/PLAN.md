# Portfolio plan: a data science portfolio that teaches as you scroll

## Context
- **Roles you're targeting:** data science, ML, and agentic AI.
- **Goals:**
  - The site should feel like a 3Blue1Brown video you can interact with: visitors *learn real data science concepts* as they browse.
  - Each concept should tie back to your résumé experiences.
  - Include a 3D "galaxy" of your work and a tool-using AI agent.
  - Work on every device.
  - Build it in a learning-first way (VibeWise), so you can defend every piece in interviews.
- **Coursework:** only the topics that make sense on a portfolio site get featured. Mechanics like row reduction stay out.

---

## The big idea: the site is a data science pipeline, and the visitor walks through it
The home page is a scroll-driven story, an "explorable explanation" in the style of 3Blue1Brown, Distill.pub, R2D3's *A Visual Introduction to Machine Learning*, and Bartosz Ciechanowski. Each chapter does three things:
1. teaches one concept with a visual you can interact with;
2. **uses that concept on the site itself**, so it isn't a textbook example;
3. ends with a **"Where I used this"** card linking to a real experience or project.

The visitor's path follows the path a data scientist takes: **question → data → model → representation → agent → evaluation.**

### Chapters (each one is a good interview story)
| # | Chapter | Concept (and its course) | Interactive visual | How the site uses it | Why it matters for your target roles |
|---|---|---|---|---|---|
| 0 | **"Where data comes from"** | Web APIs and scraping (requests, BeautifulSoup) → Postgres, ER diagrams, joins, GROUP BY (Data Management & Analysis) | The site's own **ER diagram**, which you can click. A **join animator** shows inner, left, full outer and self joins matching row by row (more accurate than the usual Venn diagram) | The pipeline really does pull your GitHub stats through the API into Postgres. The site's data lives in tables: `experiences`, `concepts`, `links` | SQL is in nearly every data science interview loop |
| 1 | **"Can you trust this number?"** | Confounding, bias, Simpson's paradox (Intro, Responsible DS, Causal Inference) | A toggle that splits data by a hidden variable, so the trend flips in front of you | Opens with a real-looking stat about the site's visitors that turns out to be confounded | Data scientists are judged on rigor, and this is *the* data science mindset |
| 2 | **"Drawing the best line"** | Regression as **projection** (the 3b1b-style insight: least squares = projecting y onto the column space) → regularization (Linear Algebra, PDS II) | Drag points; watch the residual vectors and the projection; a slider for the ridge/lasso penalty | — | Ties linear algebra to modeling, which is great to talk through |
| 3 | **"Learning by walking downhill"** | Gradient descent and optimization (FML, PDS II) | A ball rolling down a loss surface; adjustable learning rate (too high and it diverges) | — | Basic material for every ML interview |
| 4 | **"Drawing boundaries"** | Logistic regression → trees → ensembles; the bias-variance tradeoff (FML, PDS II) | Paint points of two classes; watch each model's decision boundary; a complexity slider shows overfitting | — | Classic material for data science and ML interviews |
| 5 | **"Turning words into vectors"** | Dot product, cosine similarity, embeddings (Linear Algebra → Transformers) | Type two phrases and see the angle between their vectors | Powers the **"related projects"** links on each page | The bridge to LLMs |
| 6 | **"Squashing 1,000 dimensions into 3"** | PCA (eigenvectors!) → MDS → UMAP, and clustering (PDS II, FML) | Rotate a cloud to find the direction of most variance; morph the layout PCA → UMAP | **This chapter turns into the 3D galaxy.** The page "zooms out" into the galaxy of your work, built with exactly these methods | The visual high point of the site; it shows dimensionality reduction and clustering in practice |
| 7 | **"How the assistant thinks"** | Attention and Transformers → RAG → vector search → **agent loops, harnesses, tools, MCP** (PDS II, Agentic AI workshop, Data Structures) | A small attention heatmap. An **HNSW graph search** that hops toward the nearest neighbors (data structures and graph theory). Then a **live animated agent-loop diagram** for every reply: think → call tool → observe → repeat, with retrieved chunks glowing in the galaxy | The agent itself; a hand-written harness and loop; an **MCP server** that exposes the same tools | Agentic AI, made visible instead of a black box |
| 8 | **"Is it any good?"** | Hypothesis testing, A/B tests, evals (Intro, Causal Inference) | The agent's live eval dashboard; a p-value simulator showing why low traffic means low statistical power | A real **A/B test** on the site, for example two hero headlines, with honest results and caveats | DS interviews love A/B testing; ML/AI roles love evals |

### Standout feature: "Query my portfolio with SQL"
- An in-browser SQL playground: **PGlite** (Postgres compiled to WebAssembly) or DuckDB-WASM. It runs on a read-only copy of your portfolio's own tables: `experiences`, `concepts`, `links`, `courses`.
- Visitors can run queries like these, with starter queries provided:
  - `SELECT c.name FROM links l JOIN concepts c ON … WHERE l.depth = 'applied'`;
  - "Which course taught me the most skills I've applied?" (`GROUP BY`).
- **The agent gets a `query_portfolio_sql` tool.** It writes SQL in front of the visitor, runs it in the sandbox, and shows the query alongside the answer. That combines SQL, text-to-SQL, and agentic tool use in one feature. It's safe because the database is read-only and runs on the visitor's own device.
- It works on phones. The editor can use CodeMirror 6, which handles touch.

### Where the other courses show up
- **Covariance and correlation:** a short lead-in to chapter 6. The eigenvectors of the covariance matrix *are* the PCA directions, a 3b1b-style "aha" moment.
- **pandas, NumPy, matplotlib:** the real `pipeline/` code. Link a short notebook from chapter 6 showing how the galaxy was built.
- **Git and version control:** the public repo, ADRs, and evals gating each PR. Add a small "How this site was built" page.
- **MongoDB:** one ADR, "Why Postgres over MongoDB for this data": relational links, joins, pgvector.
- **Data structures:** yes, it's relevant:
  - HNSW graphs in vector search (chapter 7);
  - inverted indexes in BM25;
  - BVH/octree spatial indexes to make star picking fast in the 3D galaxy (`three-mesh-bvh`);
  - priority queues in k-NN.

  Mention each one where it's used. It gives you good interview stories, and none of it needs its own chapter.

**Optional extras, if time allows** (each uses ML on the site):
- **Multi-armed bandit (RL)** picks which suggested question chips the agent shows, and the site shows its explore/exploit counts.
- **Recommender system**: "visitors who viewed X then viewed Y".
- **Markov chains and stochastic matrices**: PageRank over the concept graph to show your "most central" skills, using eigenvectors again.
- **Fairness audit** (Responsible DS): check whether the agent answers equally well for differently phrased or differently framed questions.

### Coursework: what to feature and what to drop
- **Feature** (it's visual, used on the site, or tested in interviews):
  - confounding and bias;
  - hypothesis testing;
  - regression and projection;
  - regularization;
  - gradient descent;
  - logistic regression, trees, ensembles;
  - bias-variance;
  - dot product and cosine similarity;
  - PCA and eigenvectors;
  - MDS/UMAP;
  - clustering;
  - neural networks;
  - attention and Transformers;
  - RL bandits;
  - recommenders;
  - causal inference;
  - fairness;
  - **SQL and joins, ER design, APIs and scraping, covariance;**
  - **agent loops, harnesses, tools, MCP;**
  - **HNSW and graph search.**
- **Mention only on the coursework page** (as background skills, without a dedicated visual):
  - determinants and diagonalization;
  - LDA;
  - SVMs;
  - autoencoders and SOMs;
  - RNNs and LSTMs;
  - KNN;
  - MongoDB;
  - Python, NumPy and pandas basics.
- **Leave out:** row reduction, echelon forms, one-to-one/onto, the rank theorem as a procedure, solution-set mechanics.

### Two ways in
- **"60-second view"**, for recruiters: the plain résumé, top projects, and skills. It's server-rendered, needs no 3D, and is linked from the top of every page.
- **"Take the tour"**, for hiring managers and the curious: the scroll story, then the galaxy, then the agent.

### Honest downsides
- **3Blue1Brown-quality visuals take a lot of time.** Grant Sanderson spends weeks on a single video. *Mitigation:* ship chapters 1, 6, 7 and 8 first, since they're the most distinctive and the best for your target roles. Add one chapter about every 1 to 2 weeks after that.
- **It could become a textbook that buries your work.** *Rule:* every chapter ends with a "Where I used this" card, or an honest "learned, not yet applied" tag. The 60-second view always stays one click away.
- **Wrong math on a public page hurts you.** Check each explainer against course notes, or ask a TA to review it.
- **Low traffic means a weak A/B test.** Turn that into the lesson in chapter 8 (statistical power), rather than hiding it.

---

## Architecture
```
content/   concepts/*.md · experiences/*.md (tagged with concepts + depth: learned|practiced|applied) · coursework.md
pipeline/  Python: embed → PCA/MDS/UMAP layouts → HDBSCAN → galaxy.json · chunks → pgvector
web/       Next.js · scroll story (GSAP ScrollTrigger) · 2D visuals (D3 / Mafs / canvas)
           · galaxy (React Three Fiber) · chat panel with live agent trace · 60-second view
api/       FastAPI · hand-written agent loop · tools: search_profile, get_item, focus_galaxy,
           explain_concept, fetch_github · rate limits · Langfuse · evals/ in CI
```

## Tools for the 3Blue1Brown style
- **Interactive 2D math visuals:** Mafs (React components for math) or D3; KaTeX for equations.
- **Scroll-driven animation:** GSAP ScrollTrigger, or Motion (formerly Framer Motion).
- **Short pre-rendered clips:** **Manim Community Edition**, the open-source Python version of the engine 3Blue1Brown uses. It fits your Python background and is worth putting on the résumé. Export short loops as MP4/WebM to use where full interactivity isn't needed.
- **3D galaxy:** React Three Fiber + drei, with InstancedMesh and bloom on capable devices.

## Works on every device (hard requirement)
- Content first: every chapter's text and a static image of its visual are server-rendered. Interactivity and 3D load on top of that.
- Fall back to static visuals when WebGL is unavailable, on low-end GPUs (`detect-gpu`), when `prefers-reduced-motion` is set, or when Save-Data is on.
- Touch-friendly controls: tap targets of at least 44px, and drag interactions that work with a finger. The chat is a bottom sheet on phones.
- Budgets: under about 200 KB JS before the heavy visuals load. Lighthouse mobile score of 90 or higher.
- Test matrix: iPhone Safari, mid-range Android Chrome, tablet, cheap laptop, keyboard only, and a screen reader.

## Stack (check current versions at setup)
- **Front end:** Next.js + TypeScript + Tailwind, R3F + drei, D3/Mafs, GSAP, KaTeX.
- **Pipeline:** Python, an embedding API or `sentence-transformers`, `umap-learn`, `scikit-learn` (PCA/MDS), `hdbscan`.
- **API:** FastAPI + Pydantic, SSE streaming.
- **Models:** a small fast model for chat (e.g. Claude Haiku 5.5), a stronger one as the eval judge.
- **Data and retrieval:** Postgres + pgvector (Supabase/Neon), plus BM25 for hybrid search.
- **Evals and ops:** pytest + LLM-as-judge in GitHub Actions, Langfuse, Upstash rate limits, a spend cap.
- **Hosting:** Vercel (web), Railway/Fly/Modal (API).
- **In-browser SQL:** PGlite or DuckDB-WASM, with CodeMirror 6 as the editor.
- **MCP server** (now a core piece, building on what you learned in the agentic workshop): exposes the same tools through the official Python SDK.

## How we'll work: VibeWise learning mode
VibeWise 0.1.43 is installed locally but isn't available in this scratch session.
1. Create the project folder, restart Claude Code there, and run `/vibe-wise:learn`.
2. Each phase starts with **you proposing the approach**. Claude asks about tradeoffs and explains concepts, then writes code only once you decide the design is ready.
3. **You write the core pieces yourself, with review:**
   - the embedding and layout pipeline;
   - the agent loop;
   - the eval metrics;
   - each chapter's explanation text.
4. End each phase with an ADR (`docs/decisions/NNN-*.md`) in your own words. These become your interview script.

## Phases
1. **Foundation (1 to 2 weeks):**
   - design the Postgres schema and ER diagram (`experiences`, `concepts`, `links`, `courses`);
   - write the content, tagging each experience with concepts;
   - build the 60-second view and deploy.
2. **Pipeline and galaxy (2 to 3 weeks):** embeddings → layouts → `galaxy.json`, then the R3F galaxy with a mobile fallback.
3. **Chapter 6 (1 to 2 weeks):** the dimensionality reduction scroll story that turns into the galaxy. It's the centerpiece.
4. **Agent and chapter 7 (2 to 3 weeks):**
   - the hand-written loop, retrieval, and the `focus_galaxy` tool;
   - the live trace visualization;
   - rate limits.
5. **Evals and chapter 8 (1 to 2 weeks):** the eval set in CI, the public eval dashboard, the site A/B test.
6. **Chapter 0 and the SQL playground (1 to 2 weeks):** the join animator, the PGlite sandbox, and the `query_portfolio_sql` agent tool.
7. **Chapter 1, then chapters 2 to 5, then extras:** about one chapter every 1 to 2 weeks.

## First action on approval
Apply the ChelseaHeat résumé edits above to the master résumé. (`content/coursework.md` is already saved.)

Earlier note: save the appendix below verbatim as `content/coursework.md`. If no project folder has been chosen yet, save it in this session's folder and move it later.

## Verification
- Lighthouse mobile ≥ 90. The site reads fully with JavaScript off and with WebGL disabled.
- Manual pass on the device test matrix above.
- Evals pass in CI. Hallucination and refusal rates are tracked over time. The red-team prompt set is handled correctly.
- Each chapter's math is checked against course notes or by a TA.
- Self-check after each phase: can you explain each component without notes?

## Résumé → chapter map (from MASTER_RESUME.tex, Sep 29, 2026)

### Your strongest narrative hook: you're already a teacher
You have:
- 150+ hours of Wyzant tutoring with a 5.0 rating;
- co-founded Seeds of Change (100+ students);
- worked as the PDS II peer notetaker;
- led a Camp Galileo STEM cohort.

So "this portfolio teaches you data science" isn't a gimmick. It's who you are. Say so in one line on the landing page, for example: *"I've spent 150+ hours explaining math to people. This site does the same, using my own data science work as the examples."*

### "Where I used this" cards
| Ch | Concept | Strongest evidence (depth) | Backup |
|---|---|---|---|
| 0 | APIs, SQL, joins | **MarginGuard**: DuckDB pipeline joining 1.2M labor logs, 103K field notes, 90K billing records and 22K RFIs (*applied*) | eLab Apps Script + Google/Perplexity API enrichment with fallback routing; Amazon's 12 REST APIs + 70% fewer calls via caching; TutorPlatform on Postgres |
| 1 | Can you trust this number? (data quality, bias, confounding) | **eLab pipeline audit**: 31 pipelines, 24 discrepancies traced to root causes, 7 untracked scripts writing to production (*applied*) | Music capstone's leakage-free pipeline; Attio deduplication. Confounding itself: *learned* (Causal Inference, in progress) |
| 2 | Regression = projection, regularization | **Stock portfolio**: CAPM β *is* a regression slope, from excess-return regressions (*practiced*) | Amazon linear regression price forecast; Ad CTR L1/L2 logistic regression tuned with GridSearchCV |
| 3 | Gradient descent | Andrew Ng ML Specialization, FML (*learned / practiced*), shown honestly | Ad CTR logistic regression fitting |
| 4 | Decision boundaries, ensembles | **Music genre capstone**: logistic vs RF vs HistGB vs XGBoost, *why* trees won on non-linearly separable features; 0.919 AUC (*practiced*) | Ad CTR random forest |
| 5 | Embeddings, cosine similarity | **Corner datathon**: user taste vectors from averaged place embeddings (*applied*) | Amazon TextBlob sentiment (NLP) |
| 6 | PCA, t-SNE/UMAP, clustering | **Corner datathon**: t-SNE + HDBSCAN + K-Means compared, with cold-start users handled separately. **Music capstone**: PCA 24 → 15 components (95% of variance), K-Means labels as features, t-SNE showing Rap/Hip-Hop overlap (*applied*) | The galaxy reuses exactly these methods, so this chapter is your strongest |
| 7 | Agents, grounding, tools | **MarginGuard**: Claude agent grounded in pre-computed signals so every diagnosis traces back to source data, scanning 405 projects (*applied*) | eLab AI agent registry and ontology audit; eLab LLM enrichment pipeline; ElderlyAI speech-enabled AI helper; Agentic AI workshop |
| 8 | Metrics, evals, testing | **Ad CTR**: "83% accuracy, zero ranking ability (0.50 AUC)", which becomes an interactive "accuracy lies" demo with a threshold slider and ROC curve (*practiced*) | **Attio 50-row gold set** for duplicate detection (that's an eval set, which links directly to your agent's evals); Amazon's sentiment checked against labeled headlines plus 26 tests; Music confusion matrix |

### Hottakes / ChelseaHeat (2nd place). Not yet in the résumé; to add
Source: local repo `C:\Users\yaoch\OneDrive\Documents\GitHub\hot-takes` (README, Oct 4, 2026), live at hot-takes.streamlit.app.
- **What it is:**
  - Event: Grundfos Waste Heat Reuse Challenge, team "Hot Takes" (Nathan Liu, Ryan Mehta, Christopher Hsu). The repo calls the event the NYU Sustainable Data Center Hackathon.
  - Built: an hourly simulation (8,760 hours) of a phase-change thermal battery storing data-center heat for NYCHA hot water;
  - an ML demand forecast;
  - a 30-year financial model;
  - a Streamlit app with 20 inputs;
  - "Ask ChelseaHeat", a Claude assistant with 4 read-only tools that rerun the model.
- **Where it plugs into the portfolio:**
  - **Ch 7 (agents), as strong as MarginGuard:** a tool-using assistant that takes every number from a tool result, shows its tool calls, and has hard limits (5 tool calls per answer, 15 questions per session). It's the same design you'll use for the portfolio agent.
  - **Ch 8 (evaluation):** the forecast reaches 8.5% MAPE, against baselines of 9.0% (hour-of-day profile) and 17.5% (same as yesterday). **On real BDG2 dorm meters, the ML model lost to the baseline on 3 of 10 dorms**, and you published that. It's a great "honest evaluation" story.
  - **Ch 4 (ensembles):** HistGradientBoosting used to forecast demand.
  - **Ch 1 (can you trust this number?):** an explicit data-provenance section saying the demand data is synthetic, and the model contradicting your own brief (15-year payback, not 4 to 8).
  - **Galaxy:** this is also your only award, so it gets a highlighted star.
- **Your role (confirmed):** the web app, the simulation, the ML forecast and validation, and the AI assistant. **Not** the financial model or the pitch, so the bullets don't claim those.
- **On approval, edit `C:\Users\yaoch\Downloads\MASTER_RESUME (1).tex`:**
  - Add a Projects entry (after MarginGuard, Oct. 2026):
    ```latex
    \resumeProjectHeading
      {\textbf{\href{https://hot-takes.streamlit.app/}{ChelseaHeat (Hot Takes)}} $|$ \emph{Python, scikit-learn, Streamlit, Claude API}}{Oct. 2026}
      % NYU Sustainable Data Center Hackathon, Grundfos Waste Heat Reuse Challenge, team of 3. 2nd place.
      % Nathan built the web app, simulation, ML forecast + validation, and AI assistant; teammates did finance + pitch.
      \resumeItemListStart
        \resumeItem{Won 2nd place at the NYU Sustainable Data Center Hackathon (Grundfos Waste Heat Reuse Challenge, team of 3) with a decision tool for routing data center waste heat through a thermal battery to NYCHA housing}
        \resumeItem{Built an hourly (8,760 h) simulation of heat supply, hot water demand, and thermal storage dispatch, served as a Streamlit app with 20 adjustable inputs}
        \resumeItem{Forecast day-ahead hourly demand with gradient boosting (8.5\% MAPE vs 9.0\% and 17.5\% baselines), then validated it on real metered steam from 10 dorm buildings (BDG2), beating both baselines on the combined series}
        \resumeItem{Built a Claude assistant that answers judges' questions by calling 4 read-only tools that rerun the simulation, taking every number from a tool result and showing each tool call}
      \resumeItemListEnd
    ```
  - In Hackathons & Competitions, add as the first entry: `NYU Sustainable Data Center Hackathon 2026, \textbf{2nd Place} (ChelseaHeat) $|$`.
  - Optionally add `Streamlit` to Frameworks.

### What this changes in the plan
- **Build order:** ship **6 → 7 → 8 → 0** first. They have the strongest *applied* evidence. Chapter 1 comes next, built on the eLab audit story. Chapters 2 to 4 follow, and 3 stays honestly labeled "learned".
- **SQL playground:** use **DuckDB-WASM** instead of PGlite. It continues the MarginGuard story ("I used DuckDB to join 1.4M+ rows; now you can query my portfolio with it"), and it's the analytics-SQL engine that data science teams use.
- **The agent's design mirrors MarginGuard.** It's grounded in pre-computed, structured facts, and every answer cites its source. That gives you a consistent story across projects in interviews.
- **Galaxy stars:**
  - 6 experiences;
  - 9 projects;
  - 4 course projects;
  - the leadership roles;
  - the featured concepts.

  Tutoring and teaching roles form their own cluster, which shows the "teacher" story visually.
- **Replace the existing 24natel.com site** with the new one. Update its résumé bullet once v1 ships.
- **Gaps** (for later projects, not to fake): no applied causal inference or deep learning project yet. The portfolio itself fills the transformers, RAG and evals gaps.

### Privacy and confidentiality (must do before anything is public)
- The agent's corpus uses **only uncommented, confirmed bullets**. Exclude:
  - your phone number;
  - GPA;
  - LaTeX comments, TODOs, "unconfirmed" lines;
  - internal PR numbers.
- **eLab and Amazon:** check with your eLab supervisor before publishing pipeline-audit details, such as the "7 untracked scripts writing to production". Keep internal specifics general on the public site.

## Your friend's RAG site: learning from it and standing apart
RAG is already part of this plan (chapter 7, pgvector + BM25). Since a friend already has a RAG agent of themselves, yours needs to be clearly *more* than "RAG over my résumé":
- **Measure, don't assume:** run the experiment comparing full context (with prompt caching) against basic RAG against hybrid RAG with reranking. Your corpus is small, so plain RAG may well lose. Publish the numbers.
- **Evals in CI:** track hallucination and refusal rates over time.
- **Show the inside:** a live trace of retrieval and tool calls, with retrieved chunks glowing in the galaxy.
- **Go beyond retrieval:** tools like SQL over your portfolio, `focus_galaxy`, and GitHub, plus an MCP server.
- **Questions to ask your friend:**
  - How do they chunk documents, and which embedding model and vector database do they use?
  - How do they stop the agent from making things up?
  - Do they evaluate answers, or log what visitors actually ask?
  - What would they do differently?

  Their answers are free lessons, and they make good material for your ADRs.

## Open questions
- ~~Share your master résumé~~ Done; see the "Résumé → chapter map" section.
- Is it OK to describe the eLab audit and agent-registry work publicly, and in how much detail?
- Backend in Python FastAPI (recommended) or TypeScript only? Budget for model API calls (roughly $5 to $20 a month with caps)?
- The exact name of your first-semester course.

---

## Appendix: `content/coursework.md` (save as-is)

```markdown
# Coursework: ML, statistics & math learned at NYU

Context about the data science / ML topics I've studied in classes at NYU (plus learning outside of class).

## Intro course (first semester)
- Confounding
- Bias
- Statistical inference / hypothesis testing
- Correlation
- Intro to regression
- Multiple regression
- Prediction
- Logistic regression

## Linear Algebra
- Linear systems and matrices
- Row reduction
- Vectors and vector equations
- Matrix equations and solution sets
- Linear independence and subspaces
- Basis and dimension
- Rank theorem
- Matrix operations
- One-to-one and onto
- Linear transformations
- Matrix multiplication
- Determinants
- Eigenvalues / eigenvectors
- Diagonalization
- Stochastic matrices
- Dot product and orthogonality
- Projections
- Graph theory

## Principles of Data Science II (favorite class)
**Essential math for ML**
- Linear algebra and vectors
- Matrices & calculus
- Vector calculus
- Probability

**Supervised learning**
- Linear modeling
- Multivariable linear modeling
- Regularized regression
- Logistic regression
- Classification metrics
- Margin-based classifiers and SVMs
- Decision trees
- Ensemble methods (bagging and boosting)
- Regression and classification with feedforward ANNs
- Deep neural networks

**Unsupervised learning**
- Linear dimensionality reduction
- Iterative & nonlinear dimensionality reduction (MDS & unfolding)
- Basic clustering methods
- Advanced clustering methods
- Unsupervised ANNs
- Autoencoders and self-organizing maps

**Reinforcement learning & sequence models**
- Basic reinforcement learning
- Deep reinforcement learning
- RNNs
- LSTMs
- Transformers
- LLMs
- Generative AI

## Fundamentals of Machine Learning
- k-nearest neighbors (KNN)
- Linear regression
- Optimal prediction
- Linear discriminant analysis (LDA)
- Optimization
- Logistic regression
- PCA
- Trees
- Clustering
- Ensembles
- Neural networks
- Recommender systems
- Natural language processing
- Reinforcement learning

## Data Management & Analysis
- Git and version control
- Python basics
- NumPy basics
- Matplotlib basics
- pandas basics
- Covariance and correlation
- Scraping data from the web and using APIs
- requests and BeautifulSoup
- Intro to databases (PostgreSQL)
- SELECT statements
- Creating tables, INSERT
- ORDER BY, DISTINCT, WHERE, GROUP BY
- Aggregations
- Joins: inner join, left join, full outer join, self join
- Database design and foreign keys
- ER diagrams
- Sample databases for Postgres
- Intro to MongoDB

## Data Structures
- (Topics to be filled in)

## Current courses (in progress)
- Responsible Data Science
- Causal Inference
- Agentic AI workshop: agents and harnesses, agent loops, tools & MCP
```
