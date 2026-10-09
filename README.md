# 🏥 SCIAMMA: Sinhala Conversational Interface for Appointment Management and Medical Advice

A conversational AI assistant that lets patients **book doctor appointments and get medical advice by chatting in Sinhala**, built with the **Rasa** open-source framework and Python.

📄 **Published research:** [IEEE ICAC 2020 – 2nd International Conference on Advancements in Computing](https://ieeexplore.ieee.org/document/9357155)

---

## 💡 The Problem
Most healthcare chatbots only understand English, which leaves out many Sinhala-speaking patients in Sri Lanka. SCIAMMA (also called **ADAMS**, the Automated Doctor Appointment Management System) understands Sinhala user messages and replies in Sinhala, so patients can manage appointments in their own language.

## ✨ Features
- **Sinhala natural language understanding:** classifies Sinhala user messages into 12 intents
- **Doctor appointment booking:** guides the patient step by step through choosing the type of doctor, the doctor, and the hospital, date, and time, then confirms the booking
- **Medical advice flow:** answers patient questions about diseases and medicines
- **Returning-user recognition:** greets new and returning users differently and verifies returning users with a secret name
- **Time-aware greetings:** adjusts the greeting (morning, afternoon, evening, night) to the current time
- **Login-attempt awareness:** gives first-time users example phrases, and gives shorter prompts to experienced users

## 🛠️ Tech Stack
| Area | Technology |
|---|---|
| Language | Python |
| Conversational AI | Rasa (Rasa NLU + Rasa Core) |
| Intent classification | DIET Classifier |
| Dialogue management | TED Policy, Memoization Policy, Mapping Policy |
| Custom logic | Rasa SDK custom actions (9 actions) |
| Integration | REST API calls to a backend service for user, doctor, and session data |

## 🏗️ How It Works
```
Patient (Sinhala message)
   │
   ▼
Rasa NLU ── tokenizer + character n-gram features (1–4) ──► DIET Classifier ──► intent
   │
   ▼
Rasa Core (TED Policy) ──► decides the next action
   │
   ▼
Custom Actions (Rasa SDK) ──► Backend REST API (user, doctor, and session data)
   │
   ▼
Sinhala response to the patient
```

**Why character n-grams?** Sinhala has limited pretrained NLP tooling. Character-level n-gram features let the model learn from Sinhala word fragments directly, without needing a Sinhala-specific language model.

## 📊 Model Evaluation
The `results/` folder contains Rasa's evaluation output:

| Metric | Score |
|---|---|
| Intent accuracy | 92.6% |
| Weighted F1-score | 0.93 |
| Weighted precision | 0.96 |

<p align="center">
  <img src="results/confmat.png" alt="Intent classification confusion matrix" width="500">
</p>

## 📁 Project Structure
```
├── actions.py              # Custom actions: greetings, booking flow, medical advice
├── config.yml              # NLU pipeline and dialogue policies
├── domain.yml              # Intents, actions, and responses
├── data/
│   ├── nlu.json            # Sinhala training examples
│   └── stories.md          # Conversation flows
├── tests/                  # Conversation tests
├── results/                # Evaluation reports and confusion matrices
├── endpoints.yml           # Action server endpoint
└── credentials.yml         # Channel configuration (REST)
```

## 🚀 Getting Started
> This project uses the **Rasa 1.x** format (Markdown stories and `MappingPolicy`). Use a Rasa 1.x version in a matching Python environment.

```bash
# 1. Install Rasa 1.x
pip install "rasa<2.0"

# 2. Train the model
rasa train

# 3. Start the custom action server (in a separate terminal)
rasa run actions

# 4. Chat with the assistant
rasa shell
```

The custom actions expect a backend REST API at `http://127.0.0.1:5000/api/adams` that supplies user, doctor, and session data. This backend isn't included in this repository.

## 🎓 Academic Context
Developed as a final-year research project for the BSc (Hons) in Information Technology at the Sri Lanka Institute of Information Technology (SLIIT), and published at IEEE ICAC 2020.

## 👤 Author
**Binod Nishshanka** · [LinkedIn](https://www.linkedin.com/in/binod-nishshanka-8a3491188/) · [GitHub](https://github.com/DilshanNish97)
