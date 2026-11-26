# Java vs. C++: Strings & Type Conversions Cheat Sheet

A quick-reference guide for C++ developers transitioning to Java, highlighting syntax differences, mutability rules, and common pitfalls in strings and type conversions.

---

## 1. Core String Differences (`std::string` vs. `java.lang.String`)

| Feature | C++ (`std::string`) | Java (`String`) | Key Difference / Gotcha |
| :--- | :--- | :--- | :--- |
| **Mutability** | Mutable (can edit in place) | **Immutable** (cannot be changed) | Every modification creates a new `String` object. Use `StringBuilder` for loops. |
| **Length** | `s.length()` or `s.size()` | `s.length()` | Only `.length()` exists (with parentheses). |
| **Empty Check** | `s.empty()` | `s.isEmpty()` / `s.isBlank()` | `isBlank()` (Java 11+) also returns `true` for whitespace-only strings. |
| **Access Char** | `s[i]` | `s.charAt(i)` | Bracket indexing `s[i]` does **not** compile in Java. |
| **Modify Char** | `s[i] = 'x';` | *Not possible on `String`* | Convert to `char[]` via `s.toCharArray()` or use `StringBuilder`. |
| **Equality** | `s1 == s2` | `s1.equals(s2)` | `==` checks **memory reference**, not string content! |
| **Compare Order**| `s1 < s2` or `s1.compareTo(s2)` | `s1.compareTo(s2)` | `<`, `>`, `<=`, `>=` operators do not work on Java `String`. |
| **Substring** | `s.substr(start, len)` | `s.substring(start, end)` | **Trap:** Java's 2nd param is the **exclusive end index**, not length (`s.substring(i, i + len)`). |
| **Find Substring**| `s.find("ab")` (`string::npos`) | `s.indexOf("ab")` (`-1`) | Java returns `-1` if not found. Also has `s.lastIndexOf("ab")`. |
| **Contains** | `s.find("ab") != string::npos` | `s.contains("ab")` | Built-in boolean check in Java. |
| **Prefix/Suffix**| `s.starts_with("a")` (C++20) | `s.startsWith("a")` / `s.endsWith("z")` | Built-in boolean checks in Java. |
| **Concatenation** | `s += "abc";` (O(1) amortized) | `s += "abc";` (**O(N) copy!**) | `+=` on Java `String` inside a loop is **O(N²)**. Use `StringBuilder`. |
| **Split String** | Manual `stringstream` loop | `s.split(" ")` | Takes a **regex** (escape special chars like `s.split("\\.")`). |
| **Join Strings** | Manual loop | `String.join(",", list)` | Joins an array or `Iterable<String>` with a delimiter. |
| **Trim Spaces** | Manual `erase` / `find_first_not_of` | `s.trim()` or `s.strip()` | `strip()` (Java 11+) is Unicode-aware; both remove leading/trailing whitespace. |
| **Case Change** | `transform(..., ::tolower)` | `s.toLowerCase()` / `s.toUpperCase()` | Returns a new `String`; does not mutate original. |
| **Replace** | `s.replace(pos, len, str)` | `s.replace("old", "new")` | Replaces **all** literal occurrences. (`replaceAll` uses regex). |
| **Repeat** | `string(n, 'a')` | `"a".repeat(n)` | Java 11+ built-in method to repeat a string `n` times. |

---

## 2. Mutable Strings: `StringBuilder` (C++ `std::string` Equivalent)

Whenever you build, modify, or reverse a string dynamically, use `StringBuilder` to achieve **O(1) amortized** appends instead of O(N²) string copying.

```java
StringBuilder sb = new StringBuilder("hello");

// Append (C++: s += " world"; s.push_back('!');)
sb.append(" world");
sb.append('!');

// Read & Modify Character (C++: char c = s; s = 'H';)
char c = sb.charAt(1);
sb.setCharAt(0, 'H');

// Delete (C++: s.pop_back(); s.erase(0, 5);)
sb.deleteCharAt(sb.length() - 1);
sb.delete(0, 5); // Note: end index is exclusive!

// Insert & Reverse (C++: s.insert(0, "Hi"); reverse(s.begin(), s.end());)
sb.insert(0, "Hi");
sb.reverse();

// Convert back to immutable String
String finalStr = sb.toString();
```

---

## 3. Sorting a String (C++ `sort(s.begin(), s.end())`)

Because Java `String` is immutable, you cannot sort it directly. Convert it to a `char[]` first:

```java
String s = "dcba";
char[] arr = s.toCharArray();
Arrays.sort(arr);
String sorted = new String(arr); // "abcd"
```

---

## 4. Character Operations (`<cctype>` vs. `java.lang.Character`)

C++ uses global functions on `char`; Java uses static methods on the `Character` wrapper class.

| Operation | C++ (`<cctype>`) | Java (`Character`) |
| :--- | :--- | :--- |
| **Is Digit (`0-9`)** | `isdigit(ch)` | `Character.isDigit(ch)` |
| **Is Letter (`a-z, A-Z`)** | `isalpha(ch)` | `Character.isLetter(ch)` |
| **Is Alphanumeric** | `isalnum(ch)` | `Character.isLetterOrDigit(ch)` |
| **Is Whitespace** | `isspace(ch)` | `Character.isWhitespace(ch)` |
| **Is Upper / Lower** | `isupper(ch)` / `islower(ch)` | `Character.isUpperCase(ch)` / `Character.isLowerCase(ch)` |
| **To Upper / Lower** | `toupper(ch)` / `tolower(ch)` | `Character.toUpperCase(ch)` / `Character.toLowerCase(ch)` |
| **Char Digit to Int** | `ch - '0'` | `ch - '0'` or `Character.getNumericValue(ch)` |
| **Int Digit to Char** | `digit + '0'` | `(char) (digit + '0')` *(explicit cast required!)* |

---

## 5. Type Conversions (C++ vs. Java)

Java is strictly typed and does **not** allow implicit narrowing conversions (e.g., `long` to `int` or `double` to `int` requires an explicit cast).

### A. String $\leftrightarrow$ Numbers

| Conversion | C++ | Java | Notes |
| :--- | :--- | :--- | :--- |
| **`String` $\rightarrow$ `int`** | `stoi(s)` | `Integer.parseInt(s)` | Throws `NumberFormatException` if invalid. |
| **`String` $\rightarrow$ `long`** | `stoll(s)` | `Long.parseLong(s)` | Use for 64-bit integers. |
| **`String` $\rightarrow$ `double`**| `stod(s)` | `Double.parseDouble(s)` | Parses floating-point strings. |
| **Number $\rightarrow$ `String`**| `to_string(num)` | `String.valueOf(num)` | Also works via `Integer.toString(num)` or `"" + num`. |
| **`char` $\rightarrow$ `String`** | `string(1, ch)` | `String.valueOf(ch)` | Also `Character.toString(ch)`. |
| **`char[]` $\rightarrow$ `String`**| `string(arr, len)` | `new String(arr)` | Or `String.valueOf(arr)`. |
| **Binary/Hex String $\rightarrow$ `int`** | `stoi(s, nullptr, 2)` | `Integer.parseInt(s, 2)` | Pass radix (`2`, `8`, `16`) as 2nd argument. |
| **`int` $\rightarrow$ Binary/Hex String** | `bitset<32>(n).to_string()` | `Integer.toBinaryString(n)` | Also `Integer.toHexString(n)`, `Integer.toOctalString(n)`. |

### B. Primitive Type Casting & Gotchas

```java
// 1. Widening (Implicit - Safe in both C++ & Java)
int a = 10;
long b = a;      // Automatic
double d = b;    // Automatic

// 2. Narrowing (Explicit Cast REQUIRED in Java - C++ allows implicit with warning)
long big = 100L;
int small = (int) big;        // Without (int), Java fails to compile!

double pi = 3.14;
int truncated = (int) pi;     // 3

// 3. Char arithmetic requires explicit cast back to char
char c = 'a';
c = (char) (c + 1);           // 'b' (In C++, c = c + 1 works without casting; Java promotes to int)

// 4. Boolean is NOT an integer in Java!
int x = 1;
// if (x) { ... }             // COMPILE ERROR in Java! Must write: if (x != 0)
// int flag = (int) true;     // COMPILE ERROR in Java! Must write: boolean ? 1 : 0
```

### C. Autoboxing & Wrapper Class Traps (`int` vs. `Integer`)
Java collections (`List<Integer>`, `Map<Character, Integer>`) cannot store primitives—they store **Wrapper Objects**.

```java
// Converting int[] <-> List<Integer>
int[] primArr = {1, 2, 3};
List<Integer> list = Arrays.stream(primArr).boxed().toList(); // Java 16+ (or .collect(Collectors.toList()))
int[] backToPrim = list.stream().mapToInt(Integer::intValue).toArray();

// CRITICAL TRAP: Comparing Wrapper Objects
Integer x = 200;
Integer y = 200;
System.out.println(x == y);      // FALSE! (Caches only -128 to 127; checks reference above 127)
System.out.println(x.equals(y)); // TRUE! Always use .equals() for Integer, Long, Character, String
```
