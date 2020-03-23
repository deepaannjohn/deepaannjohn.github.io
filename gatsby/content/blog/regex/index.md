---
title: Regex Fundamentals
date: "2020-03-01T23:50:37.121Z"
---

Regex can be used for **Pattern Recognition**.

**Typical Uses for Regex:**
Input Validation
Search and (replace)

Regex is supported in almost all major programming languages (Java, JS, C, C++, C#, Python, Perl etc)

/#[ABCDEF01234567890]/ 
// => limiters, start and end
[ABCDEF01234567890] - allowed characters
? => Match 0 or 1 time
* =>    Match 0 or more times
+ => Match 1 or more times


/#?[ABCDEF01234567890]+/ 
/#?[A-F0-9]{6}|[A-F0-9]{6}/
/#?([A-F0-9]{6}|[A-F0-9]{3})/
/#?([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})/
/#?([A-F0-9]{6}|[A-F0-9]{3})/i
/^#?([A-F0-9]{6}|[A-F0-9]{3})$/i