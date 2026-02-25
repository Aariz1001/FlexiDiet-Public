# 🥗 FlexiDiet

<div align="center">

  **AI-powered nutrition tracking & meal planning iOS & Android app**

  [![Flutter](https://img.shields.io/badge/Flutter-3.x-blue?logo=flutter)](https://flutter.dev/)
  [![Firebase](https://img.shields.io/badge/Firebase-Backend-orange?logo=firebase)](https://firebase.google.com/)
  [![Python](https://img.shields.io/badge/Cloud%20Functions-Python-blue?logo=python)](https://cloud.google.com/functions)
  [![LangChain](https://img.shields.io/badge/LangChain-AI%20Pipelines-green)](https://langchain.com/)
  [![RevenueCat](https://img.shields.io/badge/RevenueCat-Subscriptions-red)](https://revenuecat.com/)
  [![Version](https://img.shields.io/badge/Version-2.4.3-brightgreen)](#)
  [![Platform](https://img.shields.io/badge/Platform-iOS-lightgrey?logo=apple)](https://apps.apple.com)

  > 🔒 Source code is private — available upon request for technical interviews.

</div>

---

## 📱 What is FlexiDiet?

FlexiDiet is a **third-generation AI nutrition coach** — a shipped, monetised iOS & Android app that replaces tedious calorie-counting with conversational AI, camera-based food recognition, and an audio coaching podcast generated entirely from your personal nutrition data.

| Generation | Examples | Experience |
|---|---|---|
| Gen 1 | MyFitnessPal | Manual entry, large databases |
| Gen 2 | Cronometer | Data-heavy, chart overload |
| **Gen 3** | **FlexiDiet** | Conversational AI, automated planning, audio coaching |

---

## ✨ Key Features

### 🤖 AI Chat Assistant
- Natural language food logging: *"I just ate a chicken salad"* → structured nutrition card
- Function-calling AI that can log food, generate meal plans, and search recipes in a single turn
- Built on **LangChain** + **Google Gemini Flash** via Firebase Cloud Functions

### 📸 AI Image Food Analysis
- Snap a photo → instant multi-item nutritional breakdown
- Powered by **Google Gemini Vision** with portion estimation and cooking method detection
- 60 scans/month for premium users

### 🎙️ AI Progress Meetings *(unique feature)*
- Weekly **AI-generated audio podcast** (5–10 min) summarising your nutrition journey
- Two AI agents debate wins, challenges, and next steps — similar to Google NotebookLM
- Fully automated: data in → audio out, no user input required

### 📅 Intelligent Meal Planning
- One-click 7-day meal plan using **RAG** (Retrieval-Augmented Generation) over a 145+ recipe corpus stored in Firestore vector index
- Respects dietary restrictions, allergens, and calorie targets
- Auto-generates a categorised grocery shopping list

### 🛒 Grocery & Receipt Scanning
- Smart shopping list with "To Buy" / "Bought" tabs and partial quantity tracking
- Premium AI OCR receipt scanner extracts items and prices automatically

### 📊 Progress Dashboard
- Real-time calorie ring, macro breakdown (protein / carbs / fat)
- Weekly trend charts, weight tracking, water logging

### 🔔 AI Check-In System
- Proactive push notifications with 4 message types: Celebration, Encouragement, Educational, Reminder
- Personalised based on recent logging behaviour

### 🍳 Recipe Library
- 145+ curated recipes with full nutrition data, dietary tags, and difficulty levels
- Barcode scanner with **OpenFoodFacts** database integration
- Custom recipe creation (premium)

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────┐
│                  Flutter (iOS)                      │
│  Provider · Riverpod · GoRouter · fl_chart          │
│  RevenueCat SDK · Firebase Auth · Firestore SDK     │
└──────────────────────┬──────────────────────────────┘
                       │ Firebase SDK / HTTPS callables
┌──────────────────────▼──────────────────────────────┐
│          Firebase Cloud Functions (Python)          │
│                                                     │
│  ai_chat.py          → LangChain + Gemini Flash     │
│  meal_planning_ai.py → RAG + Firestore vector index │
│  podcast_agents.py   → Multi-agent audio pipeline   │
│  receipt_scanner.py  → Gemini Vision OCR            │
│  reengagement.py     → Scheduled push notifications │
│  nutrition.py        → Mifflin-St Jeor calculations │
└──────────────────────┬──────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────┐
│                Firebase Platform                    │
│  Firestore (NoSQL)  ·  Firebase Auth                │
│  Firebase Storage   ·  Cloud Messaging (FCM)        │
│  Firebase Analytics ·  Firestore Vector Search      │
└─────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Mobile** | Flutter (Dart) — iOS & Android |
| **State management** | Riverpod + Provider |
| **Backend** | Firebase Cloud Functions (Python 3.12) |
| **AI / LLM** | LangChain · Google Gemini Flash · Gemini Vision |
| **RAG pipeline** | Firestore vector index + semantic retrieval |
| **Auth** | Firebase Auth — Email, Google Sign-In, Sign in with Apple |
| **Database** | Cloud Firestore (NoSQL) |
| **Storage** | Firebase Storage |
| **Push notifications** | Firebase Cloud Messaging + flutter_local_notifications |
| **Subscriptions** | RevenueCat (App Store + Play Store) |
| **Food database** | OpenFoodFacts REST API |
| **Charts** | fl_chart + percent_indicator |
| **Navigation** | GoRouter |
| **CI/CD** | Codemagic |

---

## 💳 Monetisation

- **Free tier** — food logging, barcode scan, 145+ recipes, 3 AI messages/day
- **Premium** (£7.99/month) — unlimited AI chat, 60 image scans/month, 5 audio progress meetings/month, AI meal planning, receipt scanning
- Subscription infra handled entirely through **RevenueCat** (cross-platform, webhook-driven Firestore sync)

---

## 📂 Backend Structure (`functions/`)

```
functions/
├── main.py                   # Cloud Function entry point & router
├── ai_chat.py                # LangChain conversational AI + tool calling
├── meal_planning_ai.py       # RAG meal planner with Gemini Flash
├── agentic_meal_planner_v2.py# Multi-agent agentic planner (v2)
├── podcast_agents.py         # Dual-agent audio podcast generation
├── ai_insights.py            # Weekly nutrition insight summaries
├── ai_check_in.py            # Proactive notification content generation
├── receipt_scanner.py        # Gemini Vision OCR receipt parsing
├── nutrition.py              # BMR/TDEE calculations (Mifflin-St Jeor)
├── recipes.py                # Recipe CRUD & vector index management
├── firestore_rag.py          # RAG retrieval utilities
├── reengagement.py           # Scheduled re-engagement campaigns
├── barcode_lookup.py         # OpenFoodFacts barcode API
└── onboarding.py             # New user setup & goal initialisation
```

---

## 📊 FlexiBench

FlexiDiet ships with an internal AI benchmarking suite (`FlexiBench/`) used to evaluate and optimise LLM accuracy on nutrition tasks:
- Canonical food dataset with ground-truth nutrition values
- Automated accuracy benchmarks comparing model outputs
- Optimisation pipeline to improve AI prompt quality
- Reports calorie/macro accuracy across different models and prompting strategies

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Support** | [aariz1001.github.io/FlexiDiet/support](https://aariz1001.github.io/FlexiDiet-Portfolio/support/) |
| 🔒 **Privacy Policy** | [aariz1001.github.io/FlexiDiet/privacy](https://aariz1001.github.io/FlexiDiet-Portfolio/privacy/) |
| 📜 **Terms of Use** | [aariz1001.github.io/FlexiDiet/terms](https://aariz1001.github.io/FlexiDiet-Portfolio/terms/) |

---

## 🚀 Engineering Highlights

- **Shipped & monetised** — live on the App Store with paying subscribers
- **Full-stack solo build** — Flutter mobile + Python serverless backend + AI pipelines
- **Multi-agent AI** — dual-agent podcast generation pipeline with real user data as context
- **RAG implementation** — custom vector retrieval over Firestore for contextualised meal planning
- **Scalable serverless** — all heavy compute runs as Firebase Cloud Functions, zero idle cost
- **CI/CD pipeline** — automated builds and App Store deployments via Codemagic
- **Internal benchmarking** — custom evaluation framework (FlexiBench) to measure and improve LLM accuracy

---

<div align="center">
  <sub>Built with Flutter · Firebase · LangChain · Google Gemini · RevenueCat</sub>
</div>
