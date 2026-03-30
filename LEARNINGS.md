# Learning Records - Inky Project

## Session: March 28, 2024

### Mistake 1: Ink Script - Missing Initial Divert

**What I Did Wrong:**
Created an Ink script starting with `=== start ===` without an initial divert statement.

**What Happened:**
The story immediately showed "End of story" without displaying any content or choices.

**Root Cause:**
Ink requires either:
- Content outside of knots (runs automatically), OR
- An explicit initial divert statement (e.g., `-> start`)

**Correct Solution:**
Add `-> start` as the first line of the file to tell Ink where to begin.

**Code Before (Wrong):**
```ink
=== start ===
You enter the room and see two people waiting for you.
* [Approach Person A] -> person_A
* [Approach Person B] -> person_B
```

**Code After (Correct):**
```ink
-> start

=== start ===
You enter the room and see two people waiting for you.
* [Approach Person A] -> person_A
* [Approach Person B] -> person_B
```

**Key Learning:**
Always start Ink files with either:
1. A divert statement: `-> knot_name`
2. OR content outside of any knots (which runs automatically)

**Reference:**
WritingWithInk.md - Section on "Knots" mentions that content outside knots runs automatically, but knots themselves need explicit diverts.

---

## Next Steps
- Remember to always include initial divert or starter content
- Test in Inky before claiming success
