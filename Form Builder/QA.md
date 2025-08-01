## Entry Title

### Question
...

### Answer
...

#tag1 #tag2 #tag3

---

## How to limit a custom component to one instance and make its name read-only
### Question

Can I add a certain question or a custom component only once and prevent users from changing its name?

### Answer

To ensure that a question can only be added once, implement the [`creator.onQuestionAdded`](https://surveyjs.io/survey-creator/documentation/api-reference/survey-creator#onQuestionAdded) function and call the [`creator.toolbox.removeItem(questionType)`](https://surveyjs.io/survey-creator/documentation/api-reference/questiontoolbox#removeItem) function to remove your custom template from a list of available element types. Within the `onQuestionAdded` function, you may also define the name of a newly added question.
Additionally, if you load an existing survey, check whether the survey contains the specified question entry. If it does, disable the toolbox item.
```js
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
To make a question's `name` property read-only, implement the  [`creator.onPropertyGetReadOnly`](https://surveyjs.io/survey-creator/documentation/api-reference/survey-creator#onPropertyGetReadOnly) function.

```js
```
---