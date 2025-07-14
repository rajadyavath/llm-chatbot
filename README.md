🧪 Building an Open-Ended Conversational Chatbot
This project is a time-constrained, rapid prototype of an open-domain conversational chatbot built under a strict 5-hour deadline. The challenge was to demonstrate the process of selecting, adapting, and testing a pre-trained language model for interactive dialogue — with minimal hardware resources and maximum learning intent.

🧠 Context & Constraints
Time available: ~5 hours

Hardware: Limited to local CPU and Colab’s free tier (no GPU guarantees)

Goal: Prototype a conversational agent that could engage users in human-like dialogue

Scope: End-to-end pipeline including model selection, lightweight finetuning, simple evaluation, and interaction

Note: While aware of advanced fine-tuning techniques (RLHF, PEFT, instruction-tuning), the scope and constraints of this project prioritized fast experimentation and implementation simplicity.

⚙️ Project Setup
To run the project locally or on Colab:

bash
Copy
Edit
docker compose up
# Then manually run:
<configure training in model_finetune.ipynb>
python3 model_interact.py
📏 Defining Evaluation Metrics (Naive but Practical)
📊 Interaction-Based Metrics
Metrics focusing on user engagement, session dynamics, and return behavior:

Conversation length: Avg. number of turns per session

Response delay: Time between user input and model reply (proxy for engagement)

Feedback prompts: Post-session question like "Were you satisfied? (Y/N)" or 1–10 rating

Return rate: How often users come back

Early abandonment: % of users who drop after 1 or 2 exchanges

🧾 Text-Based Metrics
Metrics related to generation quality:

Distinct n-grams: Measures lexical diversity (proxy for fluency)

Embedding similarity: Between user input and bot reply (proxy for coherence)

Perplexity on prompts: Though more relevant for language modeling, not dialogue

BLEU / ROUGE / F1: Poor fit for open-ended dialog, but useful for factual Q&A tasks

User sentiment extraction: e.g., "bad bot", "thanks", "you're wrong" — challenging due to sarcasm, emotion, etc.

For deeper analysis, one could frame user-bot interaction as a natural language inference task (e.g., contradiction vs entailment), or use external models like sentiment classifiers.

🤖 Specialized Evaluation
There are emerging tools like IEVAL that assess specific attributes like empathy, factuality, and toxicity — useful but require additional models and setup.

🏗️ Model Selection
Given the tight compute limits, models were filtered for:

Size <13B (ideally <6B for Colab feasibility)

Availability on Hugging Face

Causal language modeling suitability

✅ Candidates Considered:
Model	Parameters	Notes
OPT	1.3B – 6.7B	Lightweight, easy to run
GPT-J	6B	Strong performer, but slower
LLaMA-2	7B	Excellent results, but not practical on Colab
GPT-Neo	1.3B	Readily available

Chosen Model: OPT-1.3B
It offered a good tradeoff between size and leaderboard performance. Attempted larger models (LLaMA-2) were not feasible due to memory constraints.

🧩 Fine-Tuning Strategy
Method: LoRA (Low-Rank Adaptation) — lightweight and simple

Reasoning: Easy to implement, low memory cost, suitable for prototyping

Alternative Considered: Prefix Tuning (more abstract task control)

📚 Dataset Selection & Challenges
Several dialog datasets were explored:

Dataset	Status	Notes
DailyDialog	✅ Used	Clean, emotion-tagged
Cornell Movie Corpus	❌ Broken loader	Huggingface load_dataset() fails
HC3	⚠️ Partial	Some splits broken
ConvAI2/3	✅ Considered	Complex structure, better for RLHF
NPS Chat (NLTK)	❌ Not suitable	Lacks turn coherence, unstructured

❗Challenge:
Many datasets lack structured turns, or context tokens like USER:, BOT:, which are essential for proper prompting. Manual preprocessing was required to ensure coherence.

🧪 Interaction Flow Design
Initial Prompting: Seeded the model with a few template-driven conversation starters.

Emotion Simulation: Injected emotional context (e.g., flirtation, empathy) to test expressiveness. No RL or conditioning used.

Command Handling: Simple input/output management using history-based prompting.

✅ Summary of Learnings
💡 What Worked:
Small LLMs like OPT-1.3B can function within Colab limits

LoRA offers quick wins for adaptation

DailyDialog offers balanced, usable data

🚧 What Didn't:
Datasets on Hugging Face are often broken or undocumented

Fine-grained evaluation requires sophisticated tooling and time

Hugging Face pipelines can be restrictive for custom tasks
