# cube-ds-toon-vs-json

A practical benchmark comparing **TOON (Token-Oriented Object Notation)** and **JSON** for Large Language Model (LLM) prompts, with a focus on **token usage, cost efficiency, and context utilization**.

---

## 🚀 Why this project exists

Most LLM APIs are billed by **token count**, yet structured data is often sent in verbose JSON format.

**TOON** is a serialization format designed specifically for LLMs that:

* Reduces token usage
* Improves context packing
* Lowers inference cost at scale

This repository demonstrates these differences using **the same data, same prompt, and same model**, changing **only the serialization format**.

---

## 🧪 What is being compared

| Aspect               | JSON                   | TOON                          |
| -------------------- | ---------------------- | ----------------------------- |
| Serialization format | Verbose, key-repeating | Compact, schema-first         |
| Token efficiency     | ❌ Low                  | ✅ High                        |
| LLM cost             | Higher                 | Lower                         |
| Latency              | Similar                | Similar (indirect gains only) |
| Use case             | APIs, storage          | LLM prompts                   |

> ⚠️ **Important**
> TOON does **not** make models reason faster.
> Its benefits come from **token reduction**, which leads to **lower cost** and **better scalability**.

---

## 📁 Project structure

```
cube-ds-toon-vs-json/
├── data/
│   └── users.json
├── src/
│   ├── toon_encoder.py
│   ├── json_prompt.py
│   ├── toon_prompt.py
│   ├── token_counter.py
│   └── run_benchmark.py
├── results/
│   └── benchmark.csv
├── requirements.txt
└── README.md
```

---

## 🧠 Example: JSON vs TOON

### JSON

```json
[
  {"id": 1, "name": "Alice", "role": "admin"},
  {"id": 2, "name": "Bob", "role": "user"}
]
```

### TOON

```toon
users[2]{id,name,role}:
 1,Alice,admin
 2,Bob,user
```

Same data, significantly fewer tokens.

---

## 🧮 How token usage is measured

Token counts are calculated using the **model-specific tokenizer** (via `tiktoken`) to ensure realistic cost estimation.

```python
import tiktoken

enc = tiktoken.encoding_for_model("gpt-4o-mini")
len(enc.encode(prompt))
```

---

## ▶️ How to run the benchmark

### 1️⃣ Install dependencies

```bash
pip install -r requirements.txt
```

### 2️⃣ Run comparison

```bash
python src/run_benchmark.py
```

### 3️⃣ Example output

```text
JSON tokens: 312
TOON tokens: 147
Token reduction: 52.9%
```

---

## 📊 Typical results (approximate)

| Dataset size       | JSON tokens | TOON tokens | Reduction |
| ------------------ | ----------- | ----------- | --------- |
| Small (10 rows)    | 300         | 150         | ~50%      |
| Medium (100 rows)  | 2,800       | 1,200       | ~57%      |
| Large (1,000 rows) | 28,000      | 11,500      | ~59%      |

---

## ✅ When TOON is a good fit

* RAG pipelines
* Analytics / tabular data
* Agent → agent communication
* Prompt caching scenarios
* Cost-sensitive LLM workloads

## ❌ When JSON is better

* External APIs
* Deeply nested irregular data
* Small payloads
* Human-edited configs

---

## 🧩 Key takeaway

> **JSON is optimized for machines.
> TOON is optimized for LLMs.**

Convert JSON → TOON **only at the LLM boundary** for maximum benefit.

---

## 🔗 References

* TOON official repository: [https://github.com/toon-format/toon](https://github.com/toon-format/toon)
* TOON specification: [https://github.com/toon-format/spec](https://github.com/toon-format/spec)
* Token-efficient prompting overview: [https://www.freecodecamp.org/news/what-is-toon-how-token-oriented-object-notation-could-change-how-ai-sees-data/](https://www.freecodecamp.org/news/what-is-toon-how-token-oriented-object-notation-could-change-how-ai-sees-data/)

---

## 📌 License

MIT
