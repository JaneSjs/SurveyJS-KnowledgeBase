---
title: How to answer a survey offline
last_updated: 2025-07-31
---

## Question

I want to create a survey, send it to respondents, and give them the option to answer it offline, then save it locally.

## Answer

Use [SurveyJS PDF Generator](https://surveyjs.io/pdf-generator/examples/save-completed-forms-as-pdf-files/reactjs) to export a survey as a fillable PDF.

To export a blank survey, do **not** define `surveyPDF.data` before exporting the survey:

```js
const surveyPDF = new SurveyPDF.SurveyPDF(surveyJson);
// Do not set surveyPDF.data for a blank form
surveyPDF.save();
```
Send the generated PDF to the respondent. They can fill it offline using any PDF reader and email it back to you.

Once you receive the filled PDF:
- You can manually enter the answers into your system
- Or programmatically parse the PDF using tools like: pdf-lib, PDF.js.

## Tags
offline, pdf, export, fillable-forms