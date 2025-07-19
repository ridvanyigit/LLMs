# From Foundations to Autonomous Agents

Welcome to my Large Language Model (LLM) Engineering repository! This repository documents an intensive journey, progressing from fundamental concepts to the design, development, and deployment of a complex, autonomous multi-agent AI system. Each folder herein represents a week of learning and hands-on projects, culminating in the capstone project: the **Autonomous Deal Hunter Framework**.

This collection showcases a wide range of practical skills, including API integration, Retrieval-Augmented Generation (RAG), model fine-tuning (Q-LoRA), production-level deployment with Modal, and the implementation of sophisticated agentic AI workflows.

## 🚀 Core Projects Showcase

- **[Autonomous Deal Hunter Framework](./Autonomous_Deal_Hunter_Framework/) :** A multi-agent system that autonomously finds, evaluates, and reports online product deals using a hybrid AI ensemble and real-world notifications.
- **[Open-Source LLM Fine-Tuning with Q-LoRA](./Open-Source-LLM-Fine-Tuning-Q-LoRA/) :** Successfully fine-tuned a Llama-3.1 8B model on a commodity GPU to outperform GPT-4o on a specialized price prediction task.
- **[LLM-Fine-Tuning-for-Price-Prediction](./LLM-Fine-Tuning-for-Price-Prediction/) :** A deep dive into the full ML lifecycle, from data curation and baseline modeling (Classic ML vs. Frontier LLMs) to fine-tuning.
- **[Expert Knowledge Worker RAG Agent](./Expert_Knowledge_Worker_RAG_Agent.ipynb) :** Built a robust RAG pipeline with LangChain, ChromaDB/FAISS, and vector embeddings to create a specialized knowledge assistant.
- **[Python to C++ LLM Converter](./Python_to_C++_LLM_Converter.ipynb) :** A practical tool for code optimization, using LLMs to translate Python into high-performance C++ and deploying open-source models as API endpoints.

---

### **Foundations and First APIs**
Focused on setting up a professional development environment (Anaconda, Jupyter Lab) and making the first programmatic calls to LLMs.
- **Key Skills:** Environment setup, API key management (`.env`), web scraping with BeautifulSoup, and fundamental API calls to both OpenAI and local open-source models via Ollama.
- **Project:** [Website Summarizer](./Website_Summarizer_with_ollama.ipynb)

### **Multi-API Integration, UIs, and Multi-Modality**
Explored the broader LLM ecosystem by integrating with multiple frontier models (Claude, Gemini, DeepSeek). The focus shifted to creating interactive user experiences and leveraging multi-modal capabilities.
- **Key Skills:** Multi-API integration, rapid UI prototyping with Gradio, building conversational chatbots with memory, implementing LLM "Tools" for external actions, and generating images (DALL-E) and audio (TTS).
- **Project:** [Multi-Modal Airline AI Assistant](./Airline_AI_Agent_MultiModal_Chatbot.ipynb)

### **Deep Dive into the Hugging Face Ecosystem**
Moved from high-level APIs to the core components of the Hugging Face `transformers` library, gaining granular control over the model inference pipeline.
- **Key Skills:** Using high-level `pipelines`, understanding `Tokenizers` and chat templates, loading models directly with `AutoModelForCausalLM`, and resource management with quantization.
- **Project:** [Audio-to-Meeting-Minutes Generator](./Audio-to-Meeting-minutes-whisper-llama3.ipynb)

### **Practical Applications & Model Deployment**
Focused on a high-value use case for LLMs: code generation and optimization. This week also introduced the concept of deploying open-source models as production-ready API endpoints.
- **Key Skills:** Advanced prompt engineering for code translation, performance benchmarking (Python vs. C++), and deploying models on Hugging Face Inference Endpoints.
- **Project:** [Python to C++ Code Converter](./Python_to_C++_LLM_Converter.ipynb)

### **Retrieval-Augmented Generation (RAG)**
Built a complete RAG system from the ground up to address LLM hallucination and ground responses in factual, domain-specific data.
- **Key Skills:** Document loading and chunking with LangChain, creating vector embeddings (OpenAI, Sentence Transformers), building and visualizing vector stores (ChromaDB, FAISS), and constructing conversational RAG chains.
- **Project:** [Expert Knowledge Worker RAG Agent](./Expert_Knowledge_Worker_RAG_Agent.ipynb)

### **The Full LLM Lifecycle & Fine-Tuning**
A two-week deep dive into training a custom LLM for a specific task. This covered the entire machine learning lifecycle, from data curation to performance evaluation and advanced fine-tuning.
- **Key Skills:** Large-scale data curation, establishing classic ML and frontier model baselines, and performing Q-LoRA fine-tuning on an open-source model (Llama-3.1 8B). The result was a specialized model that achieved a **70% success rate**, significantly outperforming both its base model (28%) and GPT-4o (55%).
- **Projects:** [Product Price Prediction](./LLM-Fine-Tuning-for-Price-Prediction/) & [Q-LoRA Fine-Tuning](./Open-Source-LLM-Fine-Tuning-Q-LoRA/)

---

## 🌟 Capstone Project Deep Dive: The Autonomous Deal Hunter Framework

**[Project Folder: `./Autonomous_Deal_Hunter_Framework/`](./Autonomous_Deal_Hunter_Framework/)**

The Work capstone project integrates all skills learned throughout the course into a single, cohesive, and autonomous AI system. This multi-agent framework operates independently to find, analyze, and report on valuable online product deals.

### **Architectural Overview**

The system is designed as a team of collaborating AI agents, orchestrated by a central **`PlanningAgent`**:

1.  **`ScannerAgent` (The Scout):** Ingests real-time data by parsing RSS feeds for potential deals. It then uses an LLM with structured output (`pydantic`) to intelligently select and summarize the 5 most promising deals with clear product descriptions and prices.

2.  **`EnsembleAgent` (The Evaluation Committee):** To achieve robust and accurate price estimations, this agent employs a hybrid "wisdom of the crowd" approach, combining predictions from three distinct models:
    - **`SpecialistAgent`:** Calls our custom fine-tuned Llama-3.1 model, which is deployed as a production-ready, serverless GPU service on **Modal**. This provides a fast, specialized opinion.
    - **`FrontierAgent`:** Implements a RAG pipeline, querying a **400,000-item ChromaDB vector store** to provide rich context to a powerful frontier model (GPT-4o), ensuring grounded, data-driven estimates.
    - **`RandomForestAgent`:** Utilizes a classic Scikit-learn Random Forest model trained on sentence embeddings to provide a purely statistical pricing perspective.
    
    The final estimate is determined by a linear regression model trained on the outputs of these three experts.

3.  **`MessagingAgent` (The Herald):** If the `EnsembleAgent` identifies a deal with a discount exceeding a predefined threshold, this agent takes action in the real world by sending a push notification directly to a user's device via the Pushover API.

### **Key Technologies and Concepts in Action**

- **Production-Level Deployment with Modal:** The `SpecialistAgent`'s model isn't just a script; it's a deployed service (`pricer_service2.py`). This demonstrates mastery of serverless GPU computing, cold-start optimization via shared volumes for model caching, and a class-based structure (`@app.cls`) for persistent, high-performance inference.

- **True Agentic Behavior:** The framework operates autonomously in a continuous loop. It independently perceives its environment (RSS feeds), reasons about the data (pricing and evaluation), and acts upon its conclusions (sending notifications). It maintains a state (`memory.json`) to avoid re-evaluating past deals.

- **Advanced Interactive Dashboard:** The system is monitored through a sophisticated Gradio UI (`price_is_right_final.py`). This dashboard features asynchronous updates for real-time logging, a dynamic table of discovered opportunities, and an interactive 3D visualization of the product vector space, providing a comprehensive view of the agent's operations.
