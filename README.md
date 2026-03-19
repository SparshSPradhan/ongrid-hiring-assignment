## Assignment submission guidelines

1. Find **atleast 5 issues** in the code base (Frontend and/or Backend)
2. Record a video sharing your screen and turning on your video to explain how did you go about solving the assignment.
3. Submit this form - https://forms.gle/AK9U6VjnuDFRS7R36


### Evaluation Criteria
- How have you reached at the bugs?
- Quality of your solutions
- After resolving all the bugs, the dashboard should look something like this - https://drive.google.com/file/d/1s07a-L_rAT8OgFg28D3gp-stxJJUHdd2/view?usp=sharing


## Expense tracker (assignment)

Flask API + React (Vite). MySQL for data.

Setup details are intentionally minimal — get the backend and frontend running, fix what breaks, and wire the app end-to-end.

- **backend/** — Flask application  
- **frontend/** — React application  

Setup the database and environment variables to connect.






-------------------------------------------------------------------------------------------->>>>>>>>>>>>>>>>>>>-------------------



#  Expense Tracker — Bug Fix Assignment




##  Overview

This project is a full-stack **Expense Tracker** application built using:

* **Backend:** Flask + MySQL
* **Frontend:** React + Recharts

The objective of this assignment was to identify and fix critical bugs affecting **pagination, data consistency, calculations, and UI behavior**.

---

##  Fixes Implemented

---

##  Backend Fixes (Flask)

### 🔹 BE-1 — Incorrect Pagination Offset

**Problem:**

```python
offset = page * per_page
```

* Page 1 skipped all records
* Result: Empty expense table

**Fix:**

```python
offset = (page - 1) * per_page
```

---

### 🔹 BE-2a — Missing Soft Delete Filter (Rows)

**Problem:**

```python
q = Expense.query
```

* Deleted expenses (`is_deleted = 1`) were still fetched
* Deleted rows reappeared after refresh

**Fix:**

```python
q = Expense.query.filter(Expense.is_deleted == 0)
```

---

### 🔹 BE-2b — Incorrect Total Count

**Problem:**

```python
total = Expense.query.count()
```

* Count included deleted records
* Caused incorrect pagination

**Fix:**

```python
total = Expense.query.filter(Expense.is_deleted == 0).count()
```

---

### 🔹 BE-3 — Amount Serialized as String

**Problem:**

```python
"amount": str(e.amount)
```

* Sent as string instead of number
* Caused frontend calculation issues

**Fix:**

```python
"amount": float(e.amount)
```

---

##  Frontend Fixes (React)

---

### 🔹 FE-1 — Incorrect Total Calculation

**Problem:**

```js
expenses.reduce((a, e) => a + e.amount, 0)
```

* Resulted in string concatenation

**Fix:**

```js
expenses.reduce((a, e) => a + parseFloat(e.amount), 0)
```

---

### 🔹 FE-2 — Incorrect Page Total Display

**Problem:**

```jsx
String(pageTotal)
```

* Displayed string instead of formatted number

**Fix:**

```jsx
pageTotal.toFixed(2)
```

---

### 🔹 FE-3 — Incorrect Pagination Logic

**Problem:**

```js
Math.floor(total / perPage)
```

* Ignored partial pages

**Fix:**

```js
Math.ceil(total / perPage)
```

---

### 🔹 FE-4 — Trend Chart Mapping Issue

**Problem:**
Old API response:

```js
data.monthly_series
```

**Fix:**
Updated mapping:

```js
const series = (data.trend_rows || []).map((row) => ({
  month: row.period,
  total: row.spend,
}));
```

---

### 🔹 FE-5 — Y-Axis Labels Clipping

**Problem:**

```jsx
margin={{ left: 0 }}
```

* Y-axis labels were cut off

**Fix:**

```jsx
margin={{ left: 20 }}
```

---

### 🔹 FE-6 — Incorrect Chart Data Key

**Problem:**

```jsx
<Bar dataKey="value" />
```

**Fix:**

```jsx
<Bar dataKey="amt" />
```

---

### 🔹 FE-7 — Delete UX Improvements

#### (a) No feedback during deletion

* Added disabled state
* Added `"Deleting..."` text

#### (b) Row remained visible after delete

**Fix (Optimistic UI):**

```js
setExpenses(prev => prev.filter(e => e.id !== id));
```

---

##  Key Learnings

* Proper handling of **pagination logic**
* Importance of **consistent backend filtering**
* Handling **soft deletes correctly**
* Avoiding **string vs number bugs**
* Syncing **API response with frontend**
* Improving UX with **optimistic updates**

---

##  Final Outcome

* ✔ Pagination works correctly
* ✔ Deleted items do not reappear
* ✔ Accurate calculations and totals
* ✔ Charts display correct data
* ✔ Improved user experience

---

## 🔗 Repository

https://github.com/SparshSPradhan/ongrid-hiring-assignment
