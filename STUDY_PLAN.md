# План обучения → Applied AI Engineer (LLM-инженер)

> Персональный план на основе курса **AI Engineering from Scratch** (503 урока, 20 фаз).
> Профиль: **программист без ML → LLM-инженер (RAG/агенты)**, ~15–20+ ч/нед, цель — карьерный свитч за ~4–5 месяцев.

---

## Стратегия за 60 секунд

Курс огромный (~314 ч). Читать всё подряд — ошибка для твоего профиля: ты потонешь в фазах про
3D/Gaussian Splatting и Fourier transform, которые LLM-инженеру почти не нужны. Поэтому фазы
разбиты на **3 уровня приоритета**:

| Уровень | Что значит | Доля времени |
|---|---|---|
| 🔴 **CORE** | Основа профессии. Проходишь **глубоко**: выводишь математику, пишешь код, делаешь артефакты. | ~70% |
| 🟡 **SUPPORTING** | Нужно понимать "что и зачем", но не переписывать с нуля. Быстрый проход: читаешь docs, запускаешь 1–2 ключевых урока. | ~20% |
| ⚪ **OPTIONAL** | По интересу / после старта работы. Можно пропустить без ущерба для цели. | ~10% |

**Главный принцип:** к фазе 11 (LLM Engineering) ты должен прийти с пониманием «что такое
токен, эмбеддинг, attention, loss». Всё, что не обслуживает этот путь, — вторично.

---

## Окружение (настроено 2026-07-16)

Изолированный venv на Python 3.12, всё `requirements.txt` установлено. CUDA НЕ ставится
(локально модели не гоняем — только инференс через API и CPU-демо).

**Активация venv в каждом новом терминале (Windows, Git Bash / cmd / PowerShell):**

```bash
# из корня репозитория
.venv\Scripts\activate        # cmd / PowerShell
source .venv/Scripts/activate  # Git Bash
```

После активации `python` и `pip` указывают на `.venv` (Python 3.12.10).

**Запуск урока:**

```bash
cd phases/NN-phase/MM-lesson/code
python main.py
python -m unittest discover tests -v
```

**LLM в уроках → через z.ai gateway.** Учебный код ходит в API тем же ключом, что и сам `pi`
агент (новые ключи НЕ заводились). Используй `anthropic` SDK — он наследует переменные
окружения автоматически:

```python
from anthropic import Anthropic
client = Anthropic()  # подхватит ANTHROPIC_BASE_URL + ANTHROPIC_AUTH_TOKEN
resp = client.messages.create(
    model="claude-sonnet-4-5",  # или glm-4.5 / glm-4.6 / claude-opus-4-5
    max_tokens=256,
    messages=[{"role": "user", "content": "..."}],
)
```

Доступные модели (проверено): `claude-sonnet-4-5`, `claude-opus-4-5`, `glm-4.6`, `glm-4.5`.
OpenAI-совместимый эндпоинт z.ai на этом ключе отдаёт `model_access_denied` — поэтому
в коде уроков используем именно `anthropic` SDK.

Шаблон конфигурации (комментируемые плейсхолдеры, безопасно для коммита): см.
[`.env.example`](.env.example). Реальные значения класть в `.env` (он в `.gitignore`).

---

## Карта фаз (приоритеты)

| Фаза | Название | Уровень | Зачем LLM-инженеру |
|---|---|---|---|
| 0 | Setup & Tooling | 🟡 | Окружение, Docker, GPU. Быстро проверь, что есть нужные ключи/среда. |
| 1 | Math Foundations | 🟡 | Только ключи: векторы/матрицы, производные/градиенты, вероятность, KL. Пропусти SVD/Fourier/graph theory. |
| 2 | ML Fundamentals | 🟡 | Метрики (precision/recall), eval, векторное представление, kNN → основа RAG-ретривала. |
| 3 | Deep Learning Core | 🟡 | Backprop, loss, оптимизаторы, dropout. Прочувствуй **один раз** на мини-фреймворке. |
| 4 | Computer Vision | ⚪ | Пропустить. Разве что урок 18 (CLIP) — пригодится для эмбеддингов/multimodal RAG. |
| 5 | NLP | 🔴 | Фундамент для LLM: токенизация, эмбеддинги, attention, retrieval, chunking, eval (RAGAS). |
| 6 | Speech & Audio | ⚪ | Пропустить. Вернёшься, если пойдёшь в voice-агенты. |
| 7 | Transformers | 🔴 | Архитектура, attention, BERT/GPT, KV cache, FlashAttention. Сердце всего. |
| 8 | Generative AI | ⚪ | Пропустить (GAN/diffusion/3D). Не относится к текстовым LLM. |
| 9 | Reinforcement Learning | 🟡 | Только уроки 9 (RLHF) — нужно понимать, как обучают ассистентов. Остальное по диагонали. |
| **10** | **LLMs from Scratch** | 🔴 | SFT, DPO, RLHF, квантизация, inference optimization, QLoRA. |
| **11** | **LLM Engineering** | 🔴 | **Главная фаза.** Prompting, RAG, advanced RAG, function calling, MCP, LoRA, eval. |
| 12 | Multimodal AI | 🟡 | По диагонали: ViT, CLIP, LLaVA, multimodal RAG, ColPali. Для VLM-задач. |
| **13** | Tools & Protocols | 🔴 | MCP (клиент/сервер), function calling, A2A, OTel. |
| **14** | Agent Engineering | 🔴 | Agent loop, memory, LangGraph, OpenAI/Claude SDK, observability, failure modes. |
| 15 | Autonomous Systems | 🟡 | По диагонали: kill switches, cost governors, HITL, durable execution. Safety-контекст. |
| 16 | Multi-Agent & Swarms | 🟡 | Orchestration patterns, handoffs, A2A. Полезно, но не blocking. |
| **17** | Infrastructure & Production | 🔴 | vLLM, SGLang, quantization, observability, FinOps, gateways. Как это живёт в проде. |
| 18 | Ethics & Safety | 🟡 | Prompt injection (15, 16), red-teaming, guardrails. Обязательно для production-LLM. |
| **19** | Capstone Projects | 🔴 | 2–3 проекта в портфолио. Это твоё доказательство компетенции для найма. |

---

## План по неделям (~16 недель)

Расчёт: **17 ч/нед** в среднем × 16 недель ≈ **270 ч** чистого времени. Это покрывает все 🔴 фазы
глубоко, 🟡 — с нужной глубиной, ⚪ — точечно.

### 🗺️ ЭТАП 1 — Фундамент (недели 1–3, ~50 ч)

Цель: понять «что ML делает под капотом», чтобы не быть чёрным ящиком.

| Нед. | Что делаю | Фазы/уроки | Объём |
|---|---|---|---|
| **1** | Окружение + мат.минимум | P0 (01,04,06,07 — быстро) + P1 (01,02,04,05,06,08) | ~16 ч |
| **2** | ML-метрики и DL-основы | P2 (09,06,18) + P3 (01,02,03,04,05,06) — backprop и loss руками | ~17 ч |
| **3** | NLP-фундамент | P5 (01,03,10,22,23) — токены, эмбеддинги, attention, chunking | ~17 ч |

> ✅ **Чекпоинт Этапа 1:** можешь объяснить своими словами — что такое градиент, loss,
> эмбеддинг, attention, и почему cosine-similarity работает для поиска. Запусти `/check-understanding 3`.

---

### 🗺️ ЭТАП 2 — Архитектура LLM (недели 4–7, ~68 ч)

Цель: понять трансформер и весь цикл обучения/тюнинга модели.

| Нед. | Что делаю | Фазы/уроки | Объём |
|---|---|---|---|
| **4** | Transformers deep dive | P7 (01,02,03,04,05,06,07) — до GPT | ~17 ч |
| **5** | KV cache + оптимизация + RLHF-контекст | P7 (12,16) + P9 (09) — FlashAttention, spec decoding, RLHF | ~16 ч |
| **6** | LLMs from Scratch — обучение | P10 (01,02,03,04,06,07,08) — токенайзеры, SFT, DPO, RLHF | ~18 ч |
| **7** | LLMs from Scratch — inference + тюнинг | P10 (11,12,08-QLoRA из P11/08) | ~17 ч |

> ✅ **Чекпоинт Этапа 2:** объясни архитектуру transformer end-to-end (input → output),
> в чём отличие pre-training / SFT / DPO / RLHF, и что делает QLoRA. Запусти capstone-track B (уроки 30–41) — собери mini-GPT.

---

### 🗺️ ЭТАП 3 — Прикладной LLM (недели 8–11, ~68 ч) 🎯 СЕРДЦЕВАЯ ФАЗА

Цель: стать dangerous в RAG, prompting, function calling, agents. Это то, за что платят.

| Нед. | Что делаю | Фазы/уроки | Объём |
|---|---|---|---|
| **8** | Prompting + RAG основы | P11 (01,02,03,04,05,06,07) + capstone-track F (64–69) | ~17 ч |
| **9** | Advanced RAG + eval | P11 (07,10) + P5 (27 RAGAS) + track F (65,66,67,68) | ~16 ч |
| **10** | Tools, MCP, function calling | P13 (01,02,03,04,06,07,08,09) + P11 (09,14) + track 13 (MCP server) | ~18 ч |
| **11** | Agent engineering | P14 (01,06,07,09,12,13,16,17,26,27) + capstone-track A (20–29) | ~17 ч |

> ✅ **Чекпоцент Этапа 3:** собери работающий **RAG с hybrid retrieval + reranker + eval метриками**
> и **agentic loop с tool use**. Это уже можно показывать на интервью.

---

### 🗺️ ЭТАП 4 — Production & специализация (недели 12–14, ~50 ч)

Цель: понимать, как LLM-системы живут в проде, и закрыть safety/мультимодалку.

| Нед. | Что делаю | Фазы/уроки | Объём |
|---|---|---|---|
| **12** | Inference-стек | P17 (04,06,08,09,13,14,16,19,22,28) — vLLM, SGLang, metrics, gateways, load testing | ~17 ч |
| **13** | Multimodal + observability | P12 (01,02,05,23,24) + P14 (23,24) — VLM, multimodal RAG, OTel, Langfuse | ~16 ч |
| **14** | Safety + multi-agent | P18 (12,13,15,16,29) + P16 (05,11,12,28) — prompt injection, guardrails, orchestration | ~17 ч |

> ✅ **Чекпоинт Этапа 4:** разверни локально vLLM/Ollama с моделью, натрави на неё
> guardrails + prompt-injection-детектор, померь TTFT/throughput.

---

### 🗺️ ЭТАП 5 — Capstone / портфолио (недели 15–16, ~34 ч) 🎯

Цель: 2–3 проекта уровня "можно показать на GitHub и на интервью".

Выбери **минимум 2** из capstone-проектов (фаза 19), лучше — 3:

| Проект | Зачем | Часов |
|---|---|---|
| **08 — Production RAG Chatbot** | Флагман. Редкая вертикаль + RAG + guardrails. Самый частый запрос на рынке. | ~20 ч |
| **01 — Terminal Coding Agent** | Agent loop + tool use + MCP. Доказывает, что понимаешь агентов. | ~20 ч |
| **11 — LLM Observability Dashboard** | Eval + OTel + cost. Отличает "джун сделал чатбот" от "инженер". | ~15 ч |
| **10 — Multi-Agent Software Team** | Если хочешь в agentic-направление. | ~20 ч |

> ✅ **Финальный чекпоинт:** 2 залитых на GitHub проекта с README, демо, метриками.
> Обновлённое резюме/LinkedIn с проектами. Готов к интервью на LLM/AI Engineer.

---

## Привычки и правила

1. **Один урок = один артефакт.** Не переходи дальше, пока не запустил код и не сохранил
   `outputs/` (prompt/skill). Артефакты = твоё портфолио.
2. **Build It, потом Use It.** Сначала пишешь с нуля, потом то же самое в PyTorch/LangChain —
   так понимаешь, что делает фреймворк.
3. **`/check-understanding <phase>`** после каждой 🔴 фазы. Если <70% — вернись к слабым урокам.
4. **Закрепляй публично.** Раз в неделю — короткий пост/заметка о выученном (X/Telegram/блог).
   Это и портфолио, и сигнал рекрутерам.
5. **Не застревай в математике.** Цель — интуиция, а не экзамен по матану. Если 2 часа не можешь
   вывести формулу — прими результат и двигайся.
6. **GPU/бюджет:** для большинства уроков хватит CPU + бесплатных API (OpenAI/Anthropic credits,
   Google Colab для training). Закладывай $20–40/мес на API-токены.

---

## Минимальный "skip-если-нет-времени" путь

Если время сожмётся — этот путь даёт ~80% ценности за ~50% времени:

```
P3 (03,05,06) → P5 (03,10,23) → P7 (02,03,05,07,12) → P10 (06,08) →
P11 (ВСЁ, 17 уроков) → P13 (06,07,08) → P14 (01,06,07) → P17 (04,08,13) → Capstone 08
```

---

## Ссылки

- Сам курс: [aiengineeringfromscratch.com](https://aiengineeringfromscratch.com)
- Скиллы в агента: `npx skills add rohitg00/ai-engineering-from-scratch`
- Визуальная карта прогресса: см. `study-map.html` (открыть в браузере)
- Проверка знаний: `/check-understanding <phase>` внутри агента со скиллами курса
