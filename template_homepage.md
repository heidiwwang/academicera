<%*    
var file = await app.workspace.getActiveFile() // retrieves the currently active file  
await app.vault.modify(file, "")  // modifies the content of the retrieved file, setting it to an empty string, effectively clearing the note  
%>
```widgets  
type: clock  
```

<% tp.user.quote() %>

---
## Countdown to Sunflower :sun_small_cloud: 

```widgets  
type: countdown  
date: 2026-11-22
```

---

> [!multi-column] Overview
>
>> [!note]+ Journal
>> - Today: [<% tp.date.now("YYYY-MM-DD") %>](obsidian://daily?vault=bioluminescence)
>
>> [!info]+ Favourites 
>> ```dataview  
>> LIST WITHOUT ID link(file.path, aliases[0])
>> FROM #bookmark  
>> SORT file.ctime DESC 
>>LIMIT 5
>> ```
>
>> [!missing]+ Upcoming
>> ```dataview  
>> TASK
>> WHERE scheduled >= date(today) AND scheduled <= date(today) + dur(30 days)
>> SORT scheduled ASC
>> ```
>
>> [!warning]+ Assignments
>> ```dataview
>> TASK
>> WHERE due >= date(today) AND due <= date(today) + dur(30 days) AND contains(tags, "#todo") AND !completed
>> SORT due ASC
>> ```
>
>> [!summary]+ Reading List
>> ```dataview
>> TASK
>> WHERE due >= date(today) AND due <= date(today) + dur(14 days) AND contains(tags, "#tbr")
>> SORT due ASC
>> ```
