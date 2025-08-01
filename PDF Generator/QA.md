## Entry Title

### Question
...

### Answer
...

#tag1 #tag2 #tag3

---

## Render International Characters when exporting a survey to PDF

### Question

How can I properly configure SurveyJS PDF export to correctly render non-Latin characters and international text (including Arabic, Russian, Chinese, Greek, and other foreign languages) without garbled output, incorrect spacing, or encoding issues?
...

### Answer

The issue occurs because default PDF fonts don't support non-Latin characters. You need to add custom fonts that support your target languages. To achieve this, register a custom font with international support (like Noto Sans).
```
import { SurveyPDF, DocController } from "survey-pdf";

// Add custom font (convert TTF to Base64 first)
DocController.addFont("NotoSans", base64FontString, "normal");

// Configure PDF with custom font
const pdfOptions = {
  fontName: "NotoSans",
  useCustomFontInHtml: true
};

const surveyPdf = new SurveyPDF(surveyJson, pdfOptions);
```

For more information, refer to the following documentation and demo:

* [Documentation: Special Characters in PDF](https://surveyjs.io/pdf-generator/documentation/customize-pdf-form-settings#custom-fonts)
* [Live Demo: Render Special Characters in PDF](https://surveyjs.io/pdf-generator/examples/special-characters-in-pdf-form/)

#internationalization #character-encoding #nonlatin #custom-fonts

---
