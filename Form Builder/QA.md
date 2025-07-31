# How to limit a custom component to one instance and make its name read-only
## Question

Can I add a custom component only once and prevent users from changing its name?

## Answer

Yes — you can hide the `name` property to make it read-only, and use Survey Creator events to ensure the component:

- Can only be added once
- Is always named consistently
- Re-enables itself if deleted

Here’s a code example:

```js
// Make the 'name' property hidden in the property grid
Serializer.getProperty('uniquewidget', 'name').visible = false;

// Disable the toolbox item if the widget already exists when loading
creator.onSurveyInstanceCreated.add((sender, options) => {
  if (options.area === 'designer-tab') {
    const existing = options.survey
      .getAllQuestions(false, true, true)
      .find((q) => q.name === 'UniqueItem' && q.getType() === 'uniquewidget');
    if (existing) {
      const toolboxItem = sender.toolbox.getItemByName('uniquewidget');
      toolboxItem.enabled = false;
    }
  }
});

// When added, set the name and disable further addition
creator.onQuestionAdded.add((sender, options) => {
  if (options.question.getType() === 'uniquewidget') {
    options.question.name = 'UniqueItem';
    const toolboxItem = sender.toolbox.getItemByName('uniquewidget');
    toolboxItem.enabled = false;
  }
});

// Re-enable the widget in the toolbox if it is removed
creator.onElementDeleting.add((sender, options) => {
  if (options.elementType === 'question') {
    const removed = options.element as Question;
    if (removed.getType() === 'uniquewidget') {
      const toolboxItem = sender.toolbox.getItemByName('uniquewidget');
      toolboxItem.enabled = true;
    }
  }
});
```
---