[Setup Guide](docs/SETUP_GUIDE.md) | [Ingestion Guide](docs/INGESTION_GUIDE.md) | [Play Store](docs/PLAYSTORE_PUBLISH.md) | [GitHub Release](docs/GITHUB_RELEASE.md) | [Categories](docs/CATEGORIES.md)

---

# Data Science MCQ

**Master Data Science, one question at a time.**

![Platform](https://img.shields.io/badge/platform-Android%20%7C%20iOS-blue)
![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B)
![License](https://img.shields.io/badge/license-Proprietary-red)
![Status](https://img.shields.io/badge/status-Active-brightgreen)

An offline-first Flutter app for practising Data Science interview questions across 28 domains. Includes a Python ingestion pipeline that uses a local LLM to parse and deduplicate questions from any text source.

---

## Features

- **Offline-first** — SQLite database, zero network dependency at runtime
- **28 categories** — Python, SQL, ML, DL, Statistics, Cloud (AWS, GCP, Azure), Spark, Kafka, Kubernetes, Docker, Airflow, MLOps, Databricks, Snowflake, dbt, NLP, CV, and more
- **Multiple quiz modes** — Quick Practice, Timed Test, Daily Challenge, Category-based
- **Streak tracking** — daily streak maintained with local notifications
- **XP and levelling system** — earn XP for correct answers, level up over time
- **Achievements** — unlock badges for milestones
- **Bookmarks** — save questions for later review
- **Notes** — attach personal notes to any question
- **Review mode** — go back through your answers after each session
- **Mistakes review** — focus on questions you got wrong
- **Statistics dashboard** — accuracy, session history, category breakdown
- **Dark / AMOLED / Light themes** — Material 3 design
- **Backup and restore** — export and import your local database

---

## Architecture

```
+-----------------------------------------------+
|                  Flutter App                  |
|                                               |
|  Presentation  (Riverpod + flutter_animate)   |
|       |                                       |
|  Providers  (Riverpod)                        |
|       |                                       |
|  Repositories                                 |
|       |                                       |
|  Drift ORM  ---------->  SQLite DB            |
+-----------------------------------------------+
              ^
              |  questions.db  (seed asset)
              |
+-----------------------------------------------+
|          Python Ingestion Pipeline            |
|                                               |
|  TextExtractor / DoclingExtractor             |
|       |                                       |
|  RegexParser  --or--  LLMParser               |
|                           |                   |
|               OpenAI-compatible API           |
|               (local Gemma server)            |
|       |                                       |
|  4-Layer Deduplication                        |
|  (Exact hash, MinHash, SimHash, Semantic)     |
|       |                                       |
|  SQLiteWriter  ---------->  questions.db      |
+-----------------------------------------------+
```

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| UI framework | Flutter 3.x / Dart |
| State management | Riverpod 2.x |
| Database ORM | Drift 2.x (SQLite) |
| Animations | flutter_animate, Lottie, Confetti |
| Typography | google_fonts (Inter) |
| Notifications | flutter_local_notifications |
| Ingestion — HTTP | httpx (OpenAI-compatible) |
| Ingestion — Embeddings | sentence-transformers (all-MiniLM-L6-v2) |
| Ingestion — CLI | Typer |
| Ingestion — Logging | Loguru |
| Ingestion — Dedup | datasketch (MinHash), SimHash (stdlib), semantic cosine |

---

## Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/khushalpareta/data-science-mcq.git
cd data-science-mcq

# 2. Install Flutter dependencies
cd app
flutter pub get

# 3. Generate Drift database code
dart run build_runner build --delete-conflicting-outputs

# 4. Generate app icon
python assets/icons/generate_icon.py
dart run flutter_launcher_icons

# 5. Run on a connected device or emulator
flutter run
```

---

## Project Structure

```
data_science_mcq/
├── app/                          # Flutter application
│   ├── android/
│   ├── ios/
│   ├── assets/
│   │   ├── animations/           # Lottie JSON files
│   │   ├── icons/                # App icon source + generator
│   │   └── seed/questions.db     # Seed SQLite database
│   ├── lib/
│   │   ├── core/
│   │   │   ├── constants/        # app_colors, app_strings, app_themes
│   │   │   ├── enums/            # difficulty, question_type, test_mode
│   │   │   ├── extensions/       # string_ext, int_ext
│   │   │   └── utils/            # date_utils, validators, id_generator
│   │   ├── data/
│   │   │   ├── database/         # Drift DB, tables, seed_loader
│   │   │   ├── models/           # question, test_result, user_stats
│   │   │   └── repositories/     # question, achievements, stats, test, user
│   │   ├── providers/            # Riverpod providers
│   │   ├── services/             # notification, streak, sound, backup, permission
│   │   └── presentation/
│   │       ├── screens/          # 15 screens
│   │       └── widgets/          # reusable widgets
│   ├── pubspec.yaml
│   └── analysis_options.yaml
├── ingestion/                    # Python ingestion pipeline
│   ├── src/mcq_ingest/
│   │   ├── cli.py
│   │   ├── pipeline.py
│   │   ├── extractors/
│   │   ├── parsers/
│   │   ├── dedup/
│   │   ├── writers/
│   │   ├── llm/
│   │   └── models/
│   ├── config/
│   │   ├── settings.yaml
│   │   └── prompts/
│   ├── .env.example
│   └── pyproject.toml
├── shared/schema/questions_schema.sql
├── docs/
├── LICENSE
├── .gitignore
├── melos.yaml
└── README.md
```

---

## Categories

28 domains are supported. See [docs/CATEGORIES.md](docs/CATEGORIES.md) for the full reference table.

| Slug | Display Name | Slug | Display Name |
|------|-------------|------|-------------|
| `python` | Python | `aws` | AWS |
| `sql` | SQL | `gcp` | Google Cloud Platform |
| `machine_learning` | Machine Learning | `azure` | Microsoft Azure |
| `deep_learning` | Deep Learning | `snowflake` | Snowflake |
| `statistics` | Statistics | `databricks` | Databricks |
| `data_visualization` | Data Visualization | `dbt` | dbt |
| `big_data` | Big Data | `nlp` | NLP |
| `spark` | Apache Spark | `computer_vision` | Computer Vision |
| `kafka` | Apache Kafka | `feature_engineering` | Feature Engineering |
| `kubernetes` | Kubernetes | `model_evaluation` | Model Evaluation |
| `docker` | Docker | `data_engineering` | Data Engineering |
| `airflow` | Apache Airflow | `mathematics` | Mathematics |
| `mlops` | MLOps | `software_engineering` | Software Engineering |
| `cloud` | Cloud (General) | `general` | General / Other |

---

## Build and Run

```bash
# Debug APK
flutter build apk --debug

# Release APK (requires a keystore — see docs/PLAYSTORE_PUBLISH.md)
flutter build apk --release

# Release App Bundle for the Play Store
flutter build appbundle --release
```

---

## Ingestion Pipeline

```bash
cd ingestion
pip install -e .
cp .env.example .env        # set LLM_BASE_URL and LLM_MODEL

# Ingest a file
mcq-ingest ingest path/to/questions.pdf --category machine_learning

# Ingest copy-pasted text
mcq-ingest ingest-text --category python < my_notes.txt

# View database statistics
mcq-ingest stats

# List all categories
mcq-ingest categories
```

See [docs/INGESTION_GUIDE.md](docs/INGESTION_GUIDE.md) for the complete guide.

---

## Documentation

| Document | Description |
|----------|-------------|
| [Setup Guide](docs/SETUP_GUIDE.md) | Step-by-step environment setup for beginners |
| [Ingestion Guide](docs/INGESTION_GUIDE.md) | Full ingestion pipeline documentation |
| [Play Store Guide](docs/PLAYSTORE_PUBLISH.md) | Publishing to Google Play Store |
| [GitHub Release Guide](docs/GITHUB_RELEASE.md) | Creating releases and automating builds |
| [Categories Reference](docs/CATEGORIES.md) | All 28 categories with example topics |

---

## Contact

**Khushal Pareta**
- Email: [khushalpareta9@gmail.com](mailto:khushalpareta9@gmail.com)
- GitHub: [github.com/khushalpareta](https://github.com/khushalpareta)
- Website: [khushalpareta.dev](https://khushalpareta.dev)

---

## License

Copyright (c) 2024-2026 Khushal Pareta. All rights reserved.

This software is proprietary. No part of this codebase may be reproduced, distributed, or used commercially without explicit written permission from the author. See [LICENSE](LICENSE) for full terms.
