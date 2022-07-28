---
title: Kiyo - AI Browser Extension
date: 2022-07-27T14:17:12+02:00
draft: false 
description: A chromium extensions which summarizes the text highlighted in your browser via AI.
---

![header image](/kiyo_smug.png)


### 01 Introduction 💡
BERT/Transformers is a hot topic in the machine learning world. This technology offers state of the art language processing and even understanding.

For this project I wanted to build something that could actually be useful in everyday life.
Reading long webpages can be time consuming, and that's when a summarization browser extension came to mind.  
Since I'm working a lot with **NLP** (natural language processing) anyways, using it in a real-life scenario seemed like an interesting challenge.

The main idea is:
1. you highlight a few paragraphs of text in your browser and click on the **extension**
2. then the extension reads this text and sends it to a **Python API** 
3. the API uses a **machine learning** model to summarize the text and returns the result.


### 02 Design 📝
Essentially, project consits of **three** main parts: The browser extension (Frontend) and the API (backend) and the machine learning.

#### Frontend
In contrast to what I initally thought, the frontend was the most time consuming part of this project.

#### Backend

#### Machine Learning
To keep it simple, I went with a **Huggingface** pretrained model. They can be imported into python easily and offer state of the art performance.

```py
import huggingace
code here::
```
Instead of paraphrasing, **bart-base-cnn** weights the importance of each sentence, which makes it faster than some of the alternatives. Therefore, the result is a selection of unchanged sentences from the original text.

With that, the machine learning was done aswell! ✅

### 03 Deployment 🚀
After trying out different hosting methods, I've settled for **AWS**. 

- Huggingface has great Sagemaker support
- Lambda functions are more cost efficient than a full-fetched server
- I've never worked with AWS and wanted to learn something new

The final pipeline looks like this:
![pipeline image](/aws_pipeline.png)
