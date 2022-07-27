---
title: "Kiyo - AI Browser Extension"
date: 2022-07-27T14:17:12+02:00
draft: false 
---
_A chromium extensions which summarizes the text highlighted in your browser via AI._

### 01 Introduction 💡

### 02 Design 📝
The project consits of two main parts: The browser extension (Frontend) and the API which includes the machine learning.

### 03 Deployment 🚀
After trying out different hosting methods, I've settled for **AWS**. 

- Huggingface has great Sagemaker support
- Lambda functions are more cost efficient than a full-fetched server
- I've never worked with AWS and wanted to learn it

The final pipeline looks like this:

> image of aws pipeline here