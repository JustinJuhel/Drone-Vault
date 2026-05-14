---
tags:
  - daily_note
---

```calendar-nav
```
___
## <% moment(tp.file.title, 'YYYY-MM-DD').format("dddd, DD MMMM YY") %>
### Programmées
```tasks
not done
due <% tp.date.now("YYYY-MM-DD") %>
sort by path
sort by priority
hide due date
```
### En retard
```tasks
not done
sort by priority
due before <% tp.date.now("YYYY-MM-DD") %>
```
### A faire dans les deux prochains jours
```tasks
not done
due after <% tp.date.now("YYYY-MM-DD") %>
due before <% tp.date.now("YYYY-MM-DD", 3) %>
sort by due date
```
### Ajoutées aujourd'hui

___
## Notes