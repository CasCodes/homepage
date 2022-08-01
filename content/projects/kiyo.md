---
title: Kiyo - AI Browser Extension
date: 2022-07-27T14:17:12+02:00
draft: false 
description: A chromium extensions which summarizes the text highlighted in your browser via AI. Build with JS, Python and AWS

---
![header image](/md_img/kiyo_smug.png)


## 01 Introduction 💡
BERT/Transformers is a hot topic in the machine learning world bacause it offers state of the art language processing for everyone.

For this project I wanted to build something that could be useful in everyday life.
Reading long webpages can be time consuming, and that's when a summarization browser extension came to mind.  
Since I'm working a lot with **NLP** (natural language processing) anyways, using it in a real-life scenario seemed like an interesting challenge.

The main idea is:
1. you highlight a few paragraphs of text in your browser and click on a "go" button in the **extension**
2. then the extension reads this text and sends it to a **Python API** 
3. the API uses a Transformers **machine learning** model to summarize the text and returns the result.


## 02 Design 📝
Essentially, project consits of **three** main parts: The browser extension (Frontend) and the API (backend) and the machine learning.

### Frontend
In contrast to what I initally thought, the frontend was the most time consuming part of this project.
The main reason for this is the way chrome extensions need to be structured.
Every chrome extension requires three JS files:
![browser structure](/md_img/browser_structure.png)

- The **extension** is the little icon that appears in the top row of the browser. It uses the `background.js`
- The **pop-up** gets opened by clicking on the extension icon. It includes `widget.html`, `widget.css` and `widget.js`
- The content of the **web page** itself can be acessed through the `content.js`

Therefore the directory structure looks like this:
```
.
├── manifest.json --> required by chrome to define versions etc
├── res
│   └── icon.png
├── script
│   ├── background.js
│   ├── content.js
│   └── widget.js
├── style
│   ├── icons  -->all the images
│   └── style.css
├── summary.html  -->popup page for displaying the text
└── widget.html -->main popup page
```

For the UI (Widget), I ended up with this:
![UI img](/md_img/kiyo_ui.png)
Next to the orange start button is a loading bar and in the bottom left corner is a status text, which will change to "loading" once the button is pressed.

Because all three scipts only "see" a limited part of the browser, they need to communicate with each other in order to get the job done:
![communication img](/md_img/extension_communication.png)

The communication between the scripts as well as the API was rather time consuming, especially since I rarely use JS.

### Backend
I knew right from the beginning that I wanted to use **Python** for the API server. 

### Machine Learning
To keep it simple, I went with a **Huggingface** pretrained model. They can be imported into python easily and offer state of the art performance.

```py
import huggingace
code here::
```
Instead of paraphrasing, **bart-base-cnn** weights the importance of each sentence, which makes it faster than some of the alternatives. Therefore, the result is a selection of unchanged sentences from the original text.

With that, the machine learning was done aswell! ✅

## 03 Deployment 🚀
After trying out different hosting methods, I've settled for **AWS**. 

- Huggingface has great Sagemaker support
- Lambda functions are more cost efficient than a full-fetched server
- I've never worked with AWS and wanted to learn something new

The final pipeline looks like this:
![pipeline image](/md_img/aws_pipeline.png)

The `content.js` sends a POST request to API Gateway, which contains the highlighted text in its body.

```json
"body": {
    "text": "<the highlighted text>"
}
```

API Gateway then invokes my Lambda function, passing the JSON of the request.
It reads the text, passes it into the sagemaker endpoint and returns the summarized text as a JSON response.

The machine learning takes about 5 seconds and the entire process takes up to 10 seconds, which feels like a long time when waiting for something to load in a browser.

## Final Thoughts