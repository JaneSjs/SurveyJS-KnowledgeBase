---
title: How to answer a survey offline
tags: [offline, pdf, export]
last_updated: 2025-07-31
---

## Question

I want to create a survey, send it to respondents, and give them the option to answer it offline, then save it locally.

## Answer

Use [SurveyJS PDF Generator](https://surveyjs.io/pdf-generator/examples/save-completed-forms-as-pdf-files/reactjs) to export a survey as a fillable PDF. To export a blank survey, do not define `surveyPDF.data` before exporting a survey. Send the PDF to the respondent. They can fill it offline using any PDF reader and email it back to you.

You can then manually enter the results to a database or parse the PDF using a tool like `pdf-lib` or `PDF.js`.


## Tags

offline, pdf, export
