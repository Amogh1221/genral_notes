# Serinity: Hackathon Demo Video Script

**Target Length:** 2-3 Minutes  
**Goal:** Clearly explain the problem, the solution, and prove the AI works entirely on-device.  

---

### Part 1: The Problem (0:00 - 0:30)
**[Visual: Screen recording showing a standard cloud chatbot (like ChatGPT) with a text overlay: "Mental Health Data is Sensitive"]**

**Speaker:**  
"Mental health resources are expensive and scarce. And while AI chatbots seem like a solution, they introduce a massive privacy risk. No one wants to share their deepest traumas or anxieties with a cloud server that might train on their data or suffer a breach. Furthermore, standard AI models suffer from 'Helpful Assistant Syndrome'—they offer toxic positivity and unsolicited advice instead of actually listening."

---

### Part 2: The Solution (0:30 - 0:50)
**[Visual: Switch to a slide or the `ARCHITECTURE.md` diagram showing the Local-First Multi-Agent System]**

**Speaker:**  
"Enter Serinity. Serinity is an offline-first, multi-agent Conversational AI designed to act as a highly empathetic clinical interviewer. By leveraging aggressive quantization, the entire pipeline—Speech-to-Text, Vector Database, and LLM Inference—runs 100% locally on standard consumer hardware. Zero cloud, absolute data sovereignty."

---

### Part 3: Live Demo - The Empathy Engine (0:50 - 1:40)
**[Visual: Turn off Wi-Fi on your computer to prove it's offline. Then, open the terminal and start the Uvicorn server. Open the browser to localhost:8000]**

**Speaker:**  
"Let's see it in action. As you can see, my Wi-Fi is completely turned off. I'm going to start the local backend."

**[Visual: Type a message into the chat UI: "I've been feeling really isolated lately. My exams are coming up and I have no friends to talk to."]**

**Speaker:**  
"Notice what the bot does here. A standard AI would immediately give me studying tips. But because we've engineered our system prompt for strict *Therapeutic Alliance*, Serinity suppresses the urge to give advice, and instead validates my feelings of isolation."

*Wait for the bot to generate the empathetic response (approx ~5 seconds).*

---

### Part 4: Live Demo - The Background Agent (1:40 - 2:10)
**[Visual: Open up VS Code and show the local `data/` or `logs/` directory where the JSON profile is saved. Open the JSON file to show the structured data.]**

**Speaker:**  
"But Serinity isn't just a basic chatbot. While Agent 1 was talking to me, our orchestrator conditionally triggered Agent 2 in the background. Agent 2 performs a deep clinical analysis on the conversation and silently updates my persistent Clinical Profile. 

As you can see in this local JSON file, the AI correctly identified 'Exam-related stress' and 'Isolation' as emotional themes, allowing the bot to remember my trajectory across multiple sessions—all without blocking the UI."

---

### Part 5: Under the Hood & Conclusion (2:10 - 2:30)
**[Visual: Show the Ragas Evaluation Table from the `TECHNICAL_REPORT.md`]**

**Speaker:**  
"Under the hood, we are using the Qwen2.5 7B model running in 4-bit quantization, achieving an average latency of just 5 seconds per chat. We rigorously evaluated our multi-agent architecture using the Ragas framework, achieving 100% Clinical Safety by ensuring the bot never attempts to diagnose or prescribe. 

Serinity provides clinical-grade empathy, with zero compromises on privacy."
