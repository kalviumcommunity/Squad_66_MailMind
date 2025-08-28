# MailMind 

MailMind is a full-stack AI-powered email assistant built using React.js, Express, and OpenAI (or Hugging Face) APIs, designed to help users automatically organize, summarize, and respond to emails. It fetches emails securely via IMAP, applies NLP for prioritization and summarization, and provides intelligent draft suggestions for fast communication.

🔧 #Core Concepts
🧠 Prompting

We use carefully designed prompts to guide the AI model into generating relevant, context-aware summaries and email replies. The prompts dynamically include details from email threads to improve response quality.

📊 #Structured Output

The AI is guided to produce structured JSON responses (e.g., labels, priorities, summaries) using prompt templates and few-shot examples. This structured output makes it easier to categorize emails, generate summaries, and suggest actions inside the UI.

⚙️ #Function Calling

The AI can trigger backend functions for tasks like:

Fetching unread emails via IMAP

Summarizing long threads

Drafting reply suggestions

Assigning priority labels to new messages

Function definitions are included in prompts, allowing the model to decide when to call them.

🔍 #RAG (Retrieval-Augmented Generation)

MailMind can integrate a basic RAG pipeline:

Important emails or previous user replies are indexed locally.

Relevant email chunks are retrieved and injected into the prompt context.

This improves factual accuracy and keeps replies consistent with the user’s prior communication style.

⚙️ #Implementation Stack

Frontend: React.js with TailwindCSS

Backend: Express.js + Node.js

Database: MongoDB for storing preferences and labeled emails

AI Layer: OpenAI API (or Hugging Face Transformers) for NLP

Email Integration: IMAP for fetching emails, SMTP for sending replies

Authentication: OAuth 2.0 for Gmail/Outlook

✅ Evaluation Criteria
✔️ Correctness

Email categorization, summarization, and draft suggestions align with actual user intent and context.

⚡ Efficiency

Fetching emails, processing summaries, and generating replies are optimized to minimize latency and API cost.

📈 Scalability

The system is modular: frontend deployable on Vercel, backend on any Node-compatible host, and AI layer can be scaled horizontally.



Zero-shot prompting → Asking an AI to perform a task without giving any examples, only instructions.

Summarization Prompt:
“Summarize this email in two sentences. Email: Hi, your package has shipped. Expected delivery: Sept 1. Tracking ID: 12345.”
Output: “Package shipped. Delivery expected Sept 1.”

Triage Prompt:
“Label this email as urgent, personal, or newsletter. Email: Meeting moved to tomorrow 10 AM — please confirm ASAP.”
Output: Urgent

Reply Draft Prompt:
“Write a polite reply confirming Friday’s meeting. Email: Hi, can we meet Friday at 3 PM to discuss the project?”
Output: “Hi, Friday 3 PM works perfectly. Looking forward to it.”




One-shot prompting → Giving the AI one example of how to do the task, then asking it to do the same for a new input.

Summarization Prompt:
“Example → Email: Hi, your payment was received. → Summary: Payment confirmed.
Now summarize: Your flight booking is confirmed for 6 PM, ref# A123.”
Output: “Flight confirmed for 6 PM.”

Triage Prompt:
“Example → Email: Don’t miss our summer sale! → Label: Newsletter
Now label: Urgent server outage — fix needed immediately.”
Output: Urgent

Reply Draft Prompt:
“Example → Email: Lunch tomorrow? → Reply: Yes, see you at noon.
Now reply to: Thanks for sending the files, I’ll review them today.”
Output: “Thanks, let me know if you need anything else.”


System Prompt (MailMind)

"You are an intelligent email assistant for MailMind. Your role is to summarize, analyze, and classify emails quickly and accurately. Always maintain a professional tone, avoid adding personal opinions, and respond concisely. Output must strictly follow the requested format without deviation."

RTFC in System Prompt:

R (Role): Intelligent email assistant for MailMind

T (Task): Summarize, analyze, and classify emails

F (Format): Must follow requested format strictly

C (Context): Used inside MailMind to help users manage emails professionally

User Prompt (MailMind)

"Summarize the following email in 3 bullet points, highlight the action item if any, and classify it as Business, Personal, or Promotion. Email: 'Hi, the quarterly meeting is rescheduled to 3 PM tomorrow. Please confirm attendance.' "

RTFC in User Prompt:

R (Role): MailMind email assistant

T (Task): Summarize, highlight action item, classify

F (Format): 3 bullet points + classification label

C (Context): Real-world email processing for users



Multi-Shot Prompting

What is Multi-Shot Prompting?
Multi-shot prompting is when you give an AI multiple examples in your prompt to help it learn the task pattern before making a prediction. This improves accuracy because the model understands the desired format and logic from prior examples.

Prompt Used in MailMind:
We used multi-shot prompting to help MailMind classify emails by priority (High, Medium, Low). By showing several labeled examples, the AI quickly learned how to judge urgency.

Example Prompt:

You are an AI email assistant. Classify each email as High, Medium, or Low priority.  
Here are examples:

Email: "Meeting with CEO at 4 PM today. Urgent updates required."  
Priority: High

Email: "Team lunch scheduled for Friday afternoon."  
Priority: Low

Email: "Client requested revised proposal by tomorrow."  
Priority: High

Email: "Monthly newsletter from HR."  
Priority: Low


Mailmind supports chain of thought prompting in its backend. Chain of thought prompting is a technique where the AI is encouraged to reason step-by-step, breaking down its thought process before arriving at a final answer. This is especially useful for writing or analyzing complex emails, as it helps the AI provide more structured, logical, and professional responses.

Example:
You are helping a user write a professional follow-up email after an interview.
Q: How should I write a polite follow-up email if I haven’t heard back in two weeks?
A: Let's think step by step.
First, I would confirm the purpose of the email — to politely inquire about the interview status.
Next, I would ensure the tone is professional and appreciative, thanking them for the opportunity.
Then, I would include a subtle reminder of my interest in the role without sounding pushy.
Finally, I would end with a courteous call to action, such as asking if they need any further information.
In summary, a follow-up email should be polite, appreciative, and concise while reiterating interest in the role.

This technique is implemented in the backend as a utility function and can be used to encourage the AI to reason step-by-step for any email writing or analysis task.


Tokens-and-tokenization

Mailmind logs the number of tokens used in the console/terminal after every AI call.
Tokens are small units of text (words or word-parts) that the AI model processes to generate responses.
For example, the sentence “Mailmind is amazing!” might be split into the tokens: “Mail”, “mind”, “ is”, “ amazing”, and “!”.
Logging tokens helps developers track and optimize usage, since AI costs and performance are directly tied to how many tokens are consumed in each request.

This functionality is implemented in the backend so every time Mailmind makes a call to the AI, the token count is printed automatically for better monitoring and cost control.


Prompt:

"Update the temperature parameter in the AI call for Mailmind to control response randomness, and create a pull request with this change. In the video, explain what 'Temperature' means in Large Language Models (LLMs) and how adjusting it affects Mailmind's responses. Also, describe how this update is reflected in the README file."

README Update Text (for Temperature):

Temperature Control in Mailmind
Mailmind now supports configurable temperature for AI responses.

What is Temperature?
Temperature determines how predictable or creative the AI’s responses are.

Low temperature (e.g., 0.1–0.3): More focused and deterministic responses.

High temperature (e.g., 0.7–1.0): More diverse and creative responses.

Usage: You can adjust the temperature in the code to suit your email personalization needs.



