# Lesson 1 — Variables, Types and Your First Real Program

**Goal:** Understand Python's basic data types, store values in variables, take input from the user, and format output. Then go through a full code-review cycle: PR → review → fixes → merge.

**Time:** About 1 hour

---

## 1.1 Set up your branch

Every lesson gets its own branch. In your exercises project (`python-journey`):

```bash
cd ~/code/python-journey
git switch main
git pull
git switch -c lesson-01-basics
mkdir exercises
```

> ⚠️ **Check where you are first.** Run `pwd` before anything else. If you have more than one project open in VS Code, the Terminal may start in the wrong one, and your branch, folder and files will all land in the wrong repo.

---

## 1.2 Experiment in the REPL

```bash
uv run python
```

The `>>>` prompt is the **REPL** (Read–Evaluate–Print Loop). It runs each line as soon as you press Enter, which makes it ideal for experimenting.

```python
name = "Murali"
age = 30
height = 5.9
is_learning = True

type(name)          # <class 'str'>
type(age)           # <class 'int'>
type(height)        # <class 'float'>
type(is_learning)   # <class 'bool'>
```

A **variable** is a name attached to a value. Python's four basic types:

| Type | Example | Used for |
|---|---|---|
| `str` | `"Murali"` | Text (always in quotes) |
| `int` | `30` | Whole numbers |
| `float` | `5.9` | Decimal numbers |
| `bool` | `True` / `False` | Yes/no values |

### Predict, then run

Guess each result **before** pressing Enter:

```python
age + 5                 # 35
"5" + "5"               # '55'  — joining text, not adding numbers
"5" * 3                 # '555' — repeating text
age + "5"               # TypeError!
int("5") + 5            # 10
f"My name is {name} and I am {age}"
```

`age + "5"` raises a **TypeError**: Python refuses to add a number to text. Convert between types with `int()`, `float()` and `str()`.

The last line is an **f-string**: put `f` before the quotes, and anything inside `{ }` is replaced with its value.

Leave the REPL with `exit()`.

---

## 1.3 Your first program

Create `exercises/01_trip_cost.py` and **type** it yourself:

```python
# Trip cost calculator
distance = float(input("Trip distance (miles): "))
mpg = float(input("Your car's miles per gallon: "))
gas_price = float(input("Gas price per gallon ($): "))

gallons = distance / mpg
cost = gallons * gas_price

print(f"You'll use {gallons:.1f} gallons.")
print(f"Trip cost: ${cost:.2f}")
```

Run it **from the project's main folder**:

```bash
uv run python exercises/01_trip_cost.py
```

What's new:

- `input()` shows a prompt and waits for the user to type. It **always returns a `str`**, so wrap it in `float()` or `int()` to do math.
- `:.2f` inside an f-string rounds to 2 decimal places (`.1f` → 1 place).
- Lines starting with `#` are **comments**. Python ignores them.

### Test it by hand

Enter 100 miles, 25 mpg, $3.50. You should get:

```
You'll use 4.0 gallons.
Trip cost: $14.00
```

Comparing output with an answer you worked out yourself is the simplest form of testing.

---

## 1.4 🎯 Exercise: bill splitter

Create `exercises/01_split_bill.py` that:

1. Asks for the bill total, the tip percentage and the number of people
2. Prints the tip, the grand total, and each person's share, all to 2 decimal places

**Hints:** tip = `bill * tip_percent / 100`. Use `int()` for the number of people.

**Bonus:** what happens if you type `abc` for the bill? What about `0` people? Write down the error names. You'll learn to handle them later.

<details>
<summary>One possible solution (try it yourself first!)</summary>

```python
# Bill splitter
bill_amount = float(input("Bill total ($): "))
tip_percent = float(input("Tip (%): "))
num_people = int(input("Number of people: "))

tip_amount = bill_amount * tip_percent / 100
total = bill_amount + tip_amount
per_person = total / num_people

print(f"Tip: ${tip_amount:.2f}")
print(f"Total: ${total:.2f}")
print(f"Each person pays: ${per_person:.2f}")
```

</details>

---

## 1.5 Commit, push, open a PR

```bash
git status
git add exercises/
git commit -m "Add Lesson 1 exercises: trip cost and bill splitter"
git push -u origin lesson-01-basics
```

Open the PR on GitHub with a What/Why/How description. **Don't merge it yourself.** Wait for review.

### Staging happens per change, not per file

If you edit a file after `git add`, those edits **aren't** included until you add again. Every round of changes goes through the same three stages:

1. **Modified**: you edited it (yellow **M** in VS Code)
2. **Staged**: `git add` marks this version for the next commit
3. **Committed**: `git commit` saves the snapshot

---

## 1.6 Common beginner mistakes (from a real code review)

These came up in a real review of this exercise.

| Mistake | Example | Fix |
|---|---|---|
| **camelCase names** (Java/JavaScript style) | `billAmount`, `tipPercentage` | Python uses **snake_case** ([PEP 8](https://peps.python.org/pep-0008/)): `bill_amount`, `tip_percent` |
| **Abbreviations** | `partyCnt` | `num_people`: readers shouldn't have to decode names |
| **Unit-first names** | `amt_bill`, `amt_tip` | Read like English: `bill_amount`, `tip_amount` |
| **Sloppy user-facing text** | `"Whats the Bill Amount"`, double spaces in output | Proofread prompts. Users see every typo |
| **Copied comments** | `# Trip cost calculator` at the top of the bill splitter | Comments must match the code, or they mislead readers |
| **Merging your own PR before review** | — | On a team, wait for an approval |
| **Running from the wrong folder** | `can't open file '.../exercises/exercises/01_trip_cost.py'` | Run `pwd`. Run commands from the project's main folder |

### The review cycle

After review, don't edit the merged branch. Start a new one:

```bash
git switch main
git pull
git switch -c lesson-01-review-fixes
# make the fixes
git add exercises/
git commit -m "Apply review fixes: snake_case names, prompt text, header comment"
git push -u origin lesson-01-review-fixes
```

Open a second PR. Fixing things after a review is normal. Every professional developer does it daily.

---

## Concepts learned

| Concept | Example |
|---|---|
| Variable | `age = 30` |
| Types | `str`, `int`, `float`, `bool` |
| Check a type | `type(age)` |
| Convert types | `int("5")`, `float("3.5")`, `str(30)` |
| User input | `input("Prompt: ")`, which always returns `str` |
| f-strings | `f"Hello {name}"` |
| Decimal formatting | `f"{cost:.2f}"` |
| Comments | `# this is ignored` |
| TypeError | `30 + "5"` |

## ✅ Checklist

- [ ] Explored types in the REPL
- [ ] Trip cost calculator runs and gives the right answer
- [ ] Bill splitter exercise done
- [ ] PR opened, reviewed, fixed and merged
- [ ] Variable names use snake_case

**Next:** Lesson 2 — Making decisions: `if`, comparisons and handling bad input.
