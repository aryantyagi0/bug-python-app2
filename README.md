import json
import os
from datetime import datetime
import requests  # missing from requirements
import pandas as pd  # missing from requirements

# ── Bug 1: wrong file path ──────────────────────────────
def load_data():
    with open("data/users.json", "r") as f:  # folder doesn't exist
        return json.load(f)

# ── Bug 2: wrong key name ───────────────────────────────
def process_users(users):
    results = []
    for user in users:
        results.append({
            "name":  user["full_name"],   # key is "name" not "full_name"
            "age":   user["age"],
            "email": user["email"],
        })
    return results

# ── Bug 3: wrong math ───────────────────────────────────
def calculate_stats(numbers):
    total   = sum(numbers)
    average = total / len(numbers)
    maximum = max(numbers)
    minimum = min(numbers)
    return {
        "total":   total,
        "average": average,
        "max":     maximum,
        "min":     minimum,
        "range":   maximum + minimum,  # should be maximum - minimum
    }

# ── Bug 4: missing argument ─────────────────────────────
def save_report(data, filename):
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    report = {
        "generated_at": timestamp,
        "data":         data,
    }
    with open(filename, "w") as f:
        json.dump(report, f, indent=2)
    print(f"Report saved: {filename}")

def main():
    print("Starting data processor...")

    # hardcoded sample data (since file doesn't exist)
    users = [
        {"name": "Alice", "age": 30, "email": "alice@example.com"},
        {"name": "Bob",   "age": 25, "email": "bob@example.com"},
        {"name": "Carol", "age": 35, "email": "carol@example.com"},
    ]

    processed = process_users(users)
    print(f"Processed {len(processed)} users")

    ages  = [u["age"] for u in users]
    stats = calculate_stats(ages)
    print(f"Stats: {stats}")

    # Bug 4 in action — missing filename argument
    save_report(processed)  # missing second argument

    print("Done!")

if __name__ == "__main__":
    main()
```

---

### `requirements.txt` — missing packages
```
requests
# pandas missing on purpose