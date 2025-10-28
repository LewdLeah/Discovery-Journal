Library:
- [Library code](./src/library.js)

Input:
```javascript
// Your "Input" tab should look like this
const modifier = (text) => {
  // Your other input modifier scripts go here (preferred)
  text = AutoCards("input", text);
  text = LocalizedLanguages("input", text);
  // Your other input modifier scripts go here (alternative)
  return { text };
};
modifier(text);
```
Context:
```javascript
// Your "Context" tab should look like this
const modifier = (text) => {
  // Your other context modifier scripts go here (preferred)
  [text, stop] = AutoCards("context", text, stop);
  text = LocalizedLanguages("context", text);
  // Your other context modifier scripts go here (risky)
  return { text, stop };
};
modifier(text);
```
Output:
```javascript
// Your "Output" tab should look like this
const modifier = (text) => {
  // Your other output modifier scripts go here (preferred)
  text = AutoCards("output", text);
  // Your other output modifier scripts go here (alternative)
  if (globalThis.state?.DiscoveryJournal && ((
      !globalThis.state.DiscoveryJournal?.refresh
      && Array.isArray(globalThis.history)
      && (1 < globalThis.history.length)
    ) || (600000 < (Date.now() - (globalThis.state.DiscoveryJournal?.refresh ?? Infinity)))
  ) && Array.isArray(globalThis.storyCards) && (typeof addStoryCard === "function")) {
    delete state.DiscoveryJournal.refresh;
    const len = globalThis.storyCards.length;
    const content = new Array(len);
    for (let i = 0; i < len; i++) {
      const card = globalThis.storyCards[i];
      if (card && (typeof card === "object") && !Array.isArray(card)) {
        content[i] = [
          card.keys || "",
          card.entry || "",
          card.type || "",
          card.title || "",
          card.description || ""
        ];
      } else {
        content[i] = ["", "", "", "", ""];
      }
    }
    delete globalThis.worldInfo;
    globalThis.storyCards = [];
    for (let i = 0; i < len; i++) {
      addStoryCard(...content[i]);
    }
    state.DiscoveryJournal.refresh = Date.now();
  }
  return { text };
};
modifier(text);
```
