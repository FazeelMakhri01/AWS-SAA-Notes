## **Machine Learning Services**

## **1) Amazon Rekognition**

- 📌 **What it is**
    - **ML-powered image & video analysis** service
    - Detect **objects, people, text**, scenes
    - **Facial analysis & search** for verification, people counting
    - Build a **face collection** or compare against **celebrity database**
- ✅ **Use cases**
    - **Labeling** images/videos
    - **Content moderation** (detect unsafe or unwanted content)
    - **Text detection** in images
    - **Face detection & analysis**
    - **Face search & verification**
    - **Celebrity recognition**
    - **Pathing/people tracking** across frames
- ⚙️ **Content Moderation Flow**
    - Image/Video → **Rekognition** → **Confidence threshold** → (optional) **A2I manual review**
    - Set a **minimum confidence score** to flag items
- 🧠 **Exam tip**
    - “**Detect objects/faces/text** in images/videos” ⇒ **Rekognition**

---

## **2) Amazon Transcribe**

- 📌 **What it is**
    - Automatic **speech-to-text** using **Automatic Speech Recognition (ASR)**
    - Converts audio to text **quickly & accurately**
    - **Redacts PII** automatically
    - **Auto language identification** for multi-lingual audio
- ✅ **Use cases**
    - Transcribe **customer service calls**
    - **Closed captioning** & **subtitles** for videos
- 🧠 **Exam tip**
    - “**Audio → text** with PII redaction” ⇒ **Transcribe**

---

## **3) Amazon Polly**

- 📌 **What it is**
    - **Text-to-speech (TTS)** using deep learning
    - Generate **lifelike speech** from text
- 🌟 **Key Features**
    - **Neural voices** for natural sound
    - **Pronunciation Lexicons** – customize word/acronym pronunciation
        - e.g., make “AWS” spoken as “Amazon Web Services”
    - **SSML (Speech Synthesis Markup Language)**
        - Control speech with **breaks**, **emphasis**, **whispering**, phonetics
- ✅ **Use cases**
    - Voice-enabled apps, audiobooks, accessibility tools
- 🧠 **Exam tip**
    - “**Text → speech**, customize with SSML & lexicons” ⇒ **Polly**

---

## **4) Amazon Translate**

- 📌 **What it is**
    - **Neural machine translation** for natural, accurate language conversion
    - Scales for **large text volumes**
- ✅ **Use cases**
    - **Website/app localization** for global audiences
    - **Real-time chat translation**
- 🌐 **Examples**
    - English: *Hi, my name is Stephen*
    - French: *Bonjour, je m'appelle Stephen*
    - Portuguese: *Olá, meu nome é Stephen*
    - Hindi: *नमस्ते, मेरा नाम स्टीफन है।*
- 🧠 **Exam tip**
    - “**Language translation at scale**” ⇒ **Translate**

---

## **5) Amazon Lex & Amazon Connect**

- 📌 **Amazon Lex**
    - Powers **Alexa** tech
    - **Automatic Speech Recognition (ASR)** → speech to text
    - **Natural Language Understanding (NLU)** → intent detection
    - Build **chatbots & call center bots**
- 📌 **Amazon Connect**
    - **Cloud-based contact center** with **no upfront cost**
    - ~**80% cheaper** than traditional solutions
    - Visual **contact flows**, integrates with **CRM** & AWS services
- 🔗 **Smart Contact Center Pattern**
    - Phone call → **Amazon Connect** → **Lex** analyzes intent → **Lambda** executes action (e.g., schedule appointment)
- 🧠 **Exam tip**
    - “**Build a voice/chat bot**” ⇒ **Lex**
    - “**Managed contact center**” ⇒ **Connect**

---

## **6) Amazon Comprehend**

- 📌 **What it is**
    - **Natural Language Processing (NLP)** service
    - Fully managed & **serverless**
- 🌟 **Capabilities**
    - **Language detection**
    - **Entity & key phrase extraction** (people, places, brands, events)
    - **Sentiment analysis** (positive/negative/neutral)
    - **Topic modeling** – organize large text collections
    - **Tokenization & parts-of-speech tagging**
- ✅ **Use cases**
    - Analyze **customer feedback/emails** for sentiment & trends
    - Auto-group **articles/documents** by discovered topics
- 🧠 **Exam tip**
    - “**Extract insights from text / NLP**” ⇒ **Comprehend**

---

## **7) Amazon SageMaker**

- 📌 **What it is**
    - **Fully managed ML platform** for developers & data scientists
    - Build, train, and deploy ML models at scale
- 🌟 **Features**
    - Integrated **Jupyter notebooks**, **training jobs**, **model hosting**
    - Built-in **algorithms** and support for custom frameworks
- ✅ **Use cases**
    - End-to-end ML workflow: **data prep → training → deployment**
- 🧠 **Exam tip**
    - “**Train & deploy ML models quickly**” ⇒ **SageMaker**

---

## **8) Amazon Kendra**

- 📌 **What it is**
    - **Intelligent search engine** powered by ML
    - Provides **natural language search** across docs
- ✅ **Use cases**
    - Enterprise document search (PDFs, manuals, knowledge bases)
- 🧠 **Exam tip**
    - “**Enterprise document search**” ⇒ **Kendra**

---

## **9) Amazon Personalize**

- 📌 **What it is**
    - Real-time **personalized recommendations** engine
    - Same tech as **Amazon.com** recommendations
- ✅ **Use cases**
    - E-commerce product recommendations
    - Personalized content/news feeds
- 🧠 **Exam tip**
    - “**ML-powered product or content recommendations**” ⇒ **Personalize**

---

## **10) Amazon Textract**

- 📌 **What it is**
    - **OCR + ML** to extract text & data from **scanned docs** (beyond simple OCR)
    - Detects **tables**, **forms**, and key-value pairs
- ✅ **Use cases**
    - Automate form processing (invoices, IDs, contracts)
- 🧠 **Exam tip**
    - “**Extract text + structured data from docs**” ⇒ **Textract**

---

## **Machine Learning Summary**

- 🏷️ **Overview of AWS ML Services**
    - **Rekognition** – Face detection, labeling, celebrity recognition
    - **Transcribe** – Audio → Text
    - **Polly** – Text → Audio
    - **Translate** – Language translation
    - **Lex + Connect** – Conversational bots & cloud contact center
    - **Comprehend** – Natural Language Processing (NLP)
    - **SageMaker** – Full ML model build/train/deploy
    - **Kendra** – Intelligent document search
    - **Personalize** – Real-time personalized recommendations
    - **Textract** – Text & data extraction from documents
- 🧠 **Key Takeaways**
    - AWS provides **end-to-end ML services**: vision (Rekognition), speech (Transcribe/Polly), language (Translate, Lex, Comprehend), modeling (SageMaker), search (Kendra), personalization (Personalize), and document processing (Textract).
    - Remembering this lineup helps quickly match **exam scenarios to services**.
