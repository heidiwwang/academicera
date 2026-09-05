---
area: "[[ua_mlis_2032]]"
tags:
  - concept
---

```dataviewjs
// Current note title (e.g., Note "concept_libraryweb" -> targets "concept_libraryweb")
const rawConcept = dv.current().file.name.replace(/^#/, "").replace(/^\[\[/, "").replace(/\]\]$/, ""); 

// Exclude config directory
const configFolderPath = "config"; 

// Match [[concept_name]] specifically while preventing partial matches on shorter substrings
const escapedConcept = rawConcept.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
const exactWikilinkRegex = new RegExp(`\\[\\[${escapedConcept}\\]\\]`, "gi");

const pages = dv.pages();

for (let page of pages) {
    // Skip current query note
    if (page.file.path === dv.current().file.path) continue;

    // Skip files inside config directory
    if (page.file.path.startsWith(configFolderPath + "/")) continue;

    const content = await dv.io.load(page.file.path);
    if (!content) continue;

    // Split content into distinct callout blocks starting with >[!
    const rawBlocks = content.split(/(?=\n>\s*\[!)/g);

    const matchingBlocks = [];

    for (let b of rawBlocks) {
        const trimmed = b.trim();
        // Reset regex state before running test
        exactWikilinkRegex.lastIndex = 0;
        
        if (trimmed.startsWith(">") && exactWikilinkRegex.test(trimmed)) {
            matchingBlocks.push(trimmed);
        }
    }

    if (matchingBlocks.length > 0) {
        dv.header(4, page.file.link);
        for (let block of matchingBlocks) {
            exactWikilinkRegex.lastIndex = 0;
            
            // Bold the matching wikilink in the display output
            const displayBlock = block.replace(exactWikilinkRegex, (match) => `**${match}**`);
            dv.paragraph(displayBlock);
        }
    }
}
```