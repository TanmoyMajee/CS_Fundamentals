# SQL Functions and Clauses — Quick Reference Notes

This document summarizes the key SQL functions and clauses discussed.

---

## 🧩 TO_CHAR()

**Purpose:** Converts a number or date to a string (text).  
**Syntax:**
```sql
TO_CHAR(value [, format])
```

**Examples:**
```sql
SELECT TO_CHAR(12345) FROM dual;
SELECT TO_CHAR(SYSDATE, 'DD-Mon-YYYY HH24:MI:SS') FROM dual;
SELECT TO_CHAR(1234567.89, '9,999,999.99') FROM dual;
```

**Common Date Formats:**
| Format | Meaning | Example |
|:--|:--|:--|
| YYYY | 4-digit year | 2025 |
| MON | 3-letter month | OCT |
| MONTH | Full month name | OCTOBER |
| DD | Day of month | 07 |
| DY | 3-letter day | TUE |
| HH24 | 24-hour format | 14 |
| MI | Minutes | 35 |
| SS | Seconds | 45 |
| AM/PM | Meridian | PM |

---

## 🗃 DUAL Table

**Purpose:** Dummy table used in Oracle SQL for selecting expressions or functions.  
**Examples:**
```sql
SELECT SYSDATE FROM dual;
SELECT 5 + 3 FROM dual;
SELECT TO_CHAR(SYSDATE, 'DD-Mon-YYYY') FROM dual;
```

---

## ✂️ SUBSTR()

**Purpose:** Extracts a substring from a string.  
**Syntax:**
```sql
SUBSTR(string, start_position [, length])
```

**Examples:**
```sql
SELECT SUBSTR('HELLO WORLD', 1, 5) FROM dual;   -- HELLO
SELECT SUBSTR('HELLO WORLD', 7) FROM dual;      -- WORLD
SELECT SUBSTR('HELLO WORLD', -5, 3) FROM dual;  -- WOR
```

**Common Uses:**
- Extract part of an ID or code
- Get domain from email
- Extract last 4 digits of a phone number

---

## 🔁 REPLACE()

**Purpose:** Replaces all occurrences of a substring.  
**Syntax:**
```sql
REPLACE(string, search_string, replacement_string)
```

**Examples:**
```sql
SELECT REPLACE('Hello World', 'World', 'Tanmoy') FROM dual;
SELECT REPLACE('123-456-7890', '-', '') FROM dual;  -- 1234567890
```

---

## 🔢 LIMIT / FETCH

**Purpose:** Restrict number of rows returned.  
**Usage differs by SQL dialect.**

### MySQL / PostgreSQL:
```sql
SELECT * FROM employees LIMIT 5;
SELECT * FROM employees LIMIT 5 OFFSET 10;
```

### Oracle / SQL Server:
```sql
SELECT * FROM employees OFFSET 0 ROWS FETCH NEXT 5 ROWS ONLY;
SELECT * FROM employees OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY;
```

---

## 🎯 ROUND()

**Purpose:** Rounds numbers or dates.

**Syntax (Numbers):**
```sql
ROUND(number [, decimal_places])
```
**Syntax (Dates):**
```sql
ROUND(date [, format])
```

**Examples:**
```sql
SELECT ROUND(123.456, 2) FROM dual;  -- 123.46
SELECT ROUND(TO_DATE('15-OCT-2025', 'DD-MON-YYYY'), 'MONTH') FROM dual;  -- 01-NOV-2025
```

---

## 🗓 MONTHS_BETWEEN()

**Purpose:** Finds the number of months between two dates.  
**Syntax:**
```sql
MONTHS_BETWEEN(date1, date2)
```

**Examples:**
```sql
SELECT MONTHS_BETWEEN(TO_DATE('07-OCT-2025','DD-MON-YYYY'),
                      TO_DATE('07-JAN-2025','DD-MON-YYYY')) FROM dual;  -- 9
SELECT ROUND(MONTHS_BETWEEN(TO_DATE('15-FEB-2025','DD-MON-YYYY'),
                             TO_DATE('01-JAN-2025','DD-MON-YYYY'))) FROM dual;  -- 1
```

---

## ✅ Summary

| Function | Purpose | Example |
|-----------|----------|----------|
| TO_CHAR() | Convert number/date to string | `TO_CHAR(SYSDATE, 'DD-Mon-YYYY')` |
| DUAL | Dummy table for expressions | `SELECT SYSDATE FROM dual;` |
| SUBSTR() | Extract part of string | `SUBSTR('HELLO WORLD', 7)` |
| REPLACE() | Replace substring | `REPLACE('ABCDEF', 'ABC', 'XYZ')` |
| LIMIT/FETCH | Restrict rows | `LIMIT 5` / `FETCH NEXT 5 ROWS ONLY` |
| ROUND() | Round number or date | `ROUND(123.456, 2)` |
| MONTHS_BETWEEN() | Find month difference | `MONTHS_BETWEEN(date1, date2)` |

---

**Author:** Tanmoy Majee  
**Topic:** SQL Function Notes  
**Date:** October 2025
