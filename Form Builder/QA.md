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
creator.JSON = // Load an existing survey
```
To make a question's `name` property read-only, implement the  [`creator.onPropertyGetReadOnly`](https://surveyjs.io/survey-creator/documentation/api-reference/survey-creator#onPropertyGetReadOnly) function.

```js
creator.onPropertyGetReadOnly.add((sender, options) => {
  if(options.property.name === "name" && options.element.getType() === "uniquewidget") {
    options.readOnly = true;
  }
})
```
---

## Setting Default Locale for SurveyJS Creator and Survey

### Question
How do I set the default locale for SurveyJS Creator (Editor) and Survey to German?

### Answer
To change the UI locale of SurveyJS Creator to German, set the `creator.locale` property:

```javascript
creator.locale = "de";
```
To change the default locale for all surveys created in the editor (so German text becomes the fallback instead of English), modify the `defaultLocale` setting:
```
import { surveyLocalization } from "survey-core";

surveyLocalization.defaultLocale = "de";
```
With this configuration, all German survey texts will be saved under the "default" key, and survey editors can still translate to other languages using the Translation tab or by changing the current survey locale.

See also:

* [Form Library: Localization](https://surveyjs.io/form-library/documentation/survey-localization)
* [Creator: Localization](https://surveyjs.io/survey-creator/documentation/survey-localization-translate-surveys-to-different-languages)

#localization #internationalization #creator-settings #default-locale