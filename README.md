# Втора домашна задача по Софтверско инженерство

**Име:** [Петар]  
**Презиме:** [Јанески]  
**Број на индекс:** [243200]

---

## Control Flow Graph

### searchBookByTitle

Јазли: N1=`if(title.isEmpty())`, N2=`throw IAE`, N3=`results=new ArrayList()`, N4=for-јамка, N5=`if(title matches && !borrowed)`, N6=`results.add(book)`, N7=`if(results.isEmpty())`, N8=`return null`, N9=`return results`

---

### borrowBook

Јазли: N1=if(title.isEmpty()||author.isEmpty()), N2=throw IAE, N3=for-јамка, N4=if(title && author match), N5=if(!book.isBorrowed()), N6=setBorrowed(true)+print, N7=return, N8=throw RE(already borrowed), N9=throw RE(not found)

---

## Цикломатска комплексност

### searchBookByTitle

Цикломатската комплексност за searchBookByTitle е 5, истата ја добив преку формулата M = E − N + 2P, каде E е бројот на ребра, N е бројот на јазли и P е бројот на поврзани компоненти (P=1).

E=14, N=11 → M = 14 − 11 + 2 = 5

Одлуки (предикатни јазли): if(title.isEmpty()), for-јамка, if(title matches && !borrowed), if(results.isEmpty()) → 4 одлуки + 1 = 5

### borrowBook

Цикломатската комплексност за `borrowBook` е 5, истата ја добив преку формулата M = E − N + 2P.

E=14, N=11 → M = 14 − 11 + 2 = 5

Одлуки: `if(title.isEmpty()||author.isEmpty())`, for-јамка, `if(title && author match)`, `if(!book.isBorrowed())` → 4 одлуки + 1 = **5**

---

## Тест случаи според критериумот Every Statement

### searchBookByTitle — функција `searchBookEveryStatementTest`

|                                                                                          | test 1 | test 2 | test 3 |
|------------------------------------------------------------------------------------------|--------|--------|--------|
| L55: `if (title.isEmpty())`                                                              | ✓      | ✓      | ✓      |
| L56: `throw new IllegalArgumentException("Invalid title")`                               | ✓      |        |        |
| L58: `List<Book> results = new ArrayList<Book>()`                                        |        | ✓      | ✓      |
| L59: `for (Book book : books)`                                                           |        | ✓      | ✓      |
| L60: `if (book.getTitle().equalsIgnoreCase(title) && !book.isBorrowed())`               |        | ✓      | ✓      |
| L61: `results.add(book)`                                                                 |        |        | ✓      |
| L64: `if (results.isEmpty())`                                                            |        | ✓      | ✓      |
| L65: `return null`                                                                       |        | ✓      |        |
| L67: `return results`                                                                    |        |        | ✓      |

- **test 1** – `title=""` → IllegalArgumentException (L55 true, L56)
- **test 2** – library:{„Clean Code"}, `title="The Hobbit"` → null (L55 false, L58–L60 false, L64 true, L65)
- **test 3** – library:{„Clean Code" слободна}, `title="Clean Code"` → List[1] (L60 true, L61, L64 false, L67)

**Минимален број на тест случаи за `searchBookByTitle` според Every Statement критериумот е 3.**

---

## Тест случаи според критериумот Every Branch

### borrowBook — функција `borrowBookEveryBranchTest`

|                                                                          | test 1 | test 2 | test 3 | test 4 |
|--------------------------------------------------------------------------|--------|--------|--------|--------|
| B1: N1→N2 (condition=true → throw IAE)                                  | ✓      |        |        |        |
| B2: N1→N3 (condition=false → продолжи)                                  |        | ✓      | ✓      | ✓      |
| B3: N3→N4 (has next book)                                                |        | ✓      | ✓      | ✓      |
| B4: N3→N9 (no more books → throw „not found")                           |        | ✓      |        |        |
| B5: N4→N5 (title+author match)                                           |        |        | ✓      | ✓      |
| B6: N4→N3 (no match → следна итерација)                                 |        | ✓      |        |        |
| B7: N5→N6 (!borrowed=true → setBorrowed, return)                        |        |        | ✓      |        |
| B8: N5→N8 (!borrowed=false → throw „already borrowed")                  |        |        |        | ✓      |

- **test 1** – `title=""`, `author="X"` → IllegalArgumentException
- **test 2** – library:{„Clean Code"}, borrow „The Hobbit" → RuntimeException(„Book not found")
- **test 3** – library:{„Clean Code" слободна}, borrow „Clean Code" → успешно
- **test 4** – library:{„Clean Code" изнајмена}, borrow „Clean Code" → RuntimeException(„already borrowed")

**Минимален број на тест случаи за `borrowBook` според Every Branch критериумот е 4.**

---

## Тест случаи според критериумот Multiple Condition

### searchBookByTitle — функција `searchBookMultipleConditionTest`

Услов: `if (book.getTitle().equalsIgnoreCase(title) && !book.isBorrowed())`

C1 = `book.getTitle().equalsIgnoreCase(title)` | C2 = `!book.isBorrowed()`

| C1 | C2 | C1 && C2 | test   |
|----|----|----------|--------|
| T  | T  | T        | test 1 |
| T  | F  | F        | test 2 |
| F  | T  | F        | test 3 |
| F  | F  | F        | test 4 |

- **test 1** – library:{„Clean Code" слободна}, `title="Clean Code"` → List[1]
- **test 2** – library:{„Clean Code" изнајмена}, `title="Clean Code"` → null
- **test 3** – library:{„Clean Code" слободна}, `title="Effective Java"` → null
- **test 4** – library:{„Clean Code" изнајмена}, `title="Effective Java"` → null

**Минимален број на тест случаи за овој услов е 4.**

---

### borrowBook — функција `borrowBookMultipleConditionTest`

Услов: `if (title.isEmpty() || author.isEmpty())`

A = `title.isEmpty()` | B = `author.isEmpty()`

| A | B | A \|\| B | test   |
|---|---|----------|--------|
| T | T | T        | test 1 |
| T | F | T        | test 2 |
| F | T | T        | test 3 |
| F | F | F        | test 4 |

- **test 1** – `title=""`, `author=""` → IllegalArgumentException
- **test 2** – `title=""`, `author="Robert C. Martin"` → IllegalArgumentException
- **test 3** – `title="Clean Code"`, `author=""` → IllegalArgumentException
- **test 4** – `title="Clean Code"`, `author="Robert C. Martin"` → успешно изнајмување

**Минимален број на тест случаи за овој услов е 4.**
