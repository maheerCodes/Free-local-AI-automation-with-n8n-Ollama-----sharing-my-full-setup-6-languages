# n8n + Local LLMs (Ollama) — Cost-Free AI Automation Workflows
### A Step-by-Step Guide with Difficulty Levels & Troubleshooting
### Available in: [English](#-english), [বাংলা](#-বাংলা-bengali), [हिन्दी](#-हिन्दी-hindi), [Español](#-español-spanish), [Français](#-français-french), [العربية](#-العربية-arabic)
---

---
Title: n8n + Ollama Local AI Automation Guide
Tags: [n8n, Ollama, Local LLM, RAG, Privacy-First AI, Zero-Cost]
Languages: [English, Bengali, Hindi, Spanish, French, Arabic]
---

## 🇬🇧 ENGLISH

### Why This Guide?
Most n8n + AI guides push you toward OpenAI API, which costs money per request. This guide shows how to connect **n8n with Ollama** (local LLMs like Llama 3, Mistral, Phi-3) so you can build automation workflows **completely free**, running on your own machine.

---

### Step 1: Install Ollama
**What to do:** Download and install Ollama on your computer.

- Windows/Mac: Download from official Ollama website, run installer.
- Linux: Run the install script via terminal.

**Difficulty by device:**
- 🟢 Easy (8GB+ RAM, modern CPU): Works smoothly with small models (Phi-3, Gemma 2B).
- 🟡 Medium (4-8GB RAM): Use smallest models only (TinyLlama, Phi-3 mini).
- 🔴 Hard (<4GB RAM): Ollama may fail to load models.

**Common Problems & Solutions:**
- *Problem:* "Command not found" after install (Windows).
  *Solution:* Restart terminal/PC; ensure Ollama is added to PATH automatically (reinstall if needed).
- *Problem:* Installer blocked by antivirus.
  *Solution:* Temporarily allow the installer or download from official source only.

---

### Step 2: Pull a Model
**What to do:** Open terminal and run a command to download a model (e.g., `llama3` or `phi3`).

**Difficulty by device:**
- 🟢 Easy: Stable internet + 8GB RAM → pull `llama3` (4-5GB download).
- 🟡 Medium: Limited RAM → pull `phi3` or `gemma:2b` (smaller, ~1.5-2GB).
- 🔴 Hard: Slow internet → download may take very long or fail midway.

**Common Problems & Solutions:**
- *Problem:* Download stuck/fails midway.
  *Solution:* Re-run the same command — Ollama resumes from where it stopped.
- *Problem:* "Out of memory" error when running model.
  *Solution:* Switch to a smaller model (e.g., from `llama3` to `phi3`).

---

### Step 3: Test Ollama Locally
**What to do:** Run the model directly in terminal to confirm it responds.

**Difficulty by device:**
- 🟢 Easy: Any system that completed Step 2 successfully.

**Common Problems & Solutions:**
- *Problem:* Model loads but responds very slowly.
  *Solution:* Close other heavy apps (browser tabs, etc.) to free RAM; this is normal on low-end devices.

---

### Step 4: Expose Ollama as an API
**What to do:** Ollama automatically runs a local API server (default: `http://localhost:11434`). Confirm it's running.

**Difficulty by device:**
- 🟢 Easy: Default setup works on all OS.

**Common Problems & Solutions:**
- *Problem:* n8n (if running in Docker) can't reach `localhost:11434`.
  *Solution:* Use `http://host.docker.internal:11434` instead of `localhost` in n8n's HTTP Request node (Windows/Mac). On Linux, use the host machine's actual IP address or run n8n with `--network=host`.

---

### Step 5: Set Up n8n
**What to do:** Install n8n (via npm, Docker, or desktop app) and open the editor.

**Difficulty by device:**
- 🟢 Easy: npm install (`npx n8n`) works on most systems with Node.js installed.
- 🟡 Medium: Docker setup requires basic Docker knowledge.

**Common Problems & Solutions:**
- *Problem:* "Port 5678 already in use."
  *Solution:* Stop the process using that port, or run n8n on a different port using environment variable `N8N_PORT`.
- *Problem:* npm install fails due to Node version.
  *Solution:* Use Node.js LTS version (18 or 20).

---

### Step 6: Connect n8n to Ollama via HTTP Request Node
**What to do:** Add an **HTTP Request** node in n8n. Set:
- Method: POST
- URL: `http://localhost:11434/api/generate` (or `host.docker.internal` if Docker)
- Body (JSON): include `model` name and `prompt` text
- Set "Response Format" to JSON

**Difficulty by device:**
- 🟢 Easy: Works on all systems once Step 4 networking is correct.

**Common Problems & Solutions:**
- *Problem:* Getting connection refused error.
  *Solution:* Double-check Ollama is running (`ollama serve` if not started automatically) and the URL/port matches.
- *Problem:* Response comes back empty or streaming format confuses n8n.
  *Solution:* Add `"stream": false` in the JSON body to get a single complete response instead of streamed chunks.

---

### Step 7: Build Your First Workflow
**What to do:** Example workflow — Trigger (Manual/Webhook) → HTTP Request (Ollama) → Process Output (Set/Code node) → Action (send email, save to Google Sheets, etc.)

**Difficulty by device:**
- 🟢 Easy: Any device that completed previous steps.
- 🟡 Medium: If using heavy models, expect 5-30 second response times — add appropriate timeouts in HTTP Request node settings.

**Common Problems & Solutions:**
- *Problem:* Workflow times out waiting for Ollama response.
  *Solution:* Increase "Timeout" setting in HTTP Request node (e.g., to 60000ms or higher) since local LLMs are slower than cloud APIs.
- *Problem:* JSON parsing error on output.
  *Solution:* Use a "Set" or "Code" node to extract `response` field from Ollama's JSON output before passing to next node.

---

### Step 8: Optimize for Your Hardware
**What to do:** Match model size to your device capability.

| Device Type | Recommended Model | Notes |
|---|---|---|
| 16GB+ RAM, GPU | llama3 8B, mistral | Fast, good quality |
| 8GB RAM, no GPU | phi3, gemma:2b | Balanced |
| 4GB RAM | tinyllama, phi3:mini | Basic tasks only |

**Common Problems & Solutions:**
- *Problem:* Model too slow for real-time automation.
  *Solution:* Use smaller quantized models (e.g., `model:q4` versions which are compressed and faster).

---

### Final Tips
- Run `ollama list` to see downloaded models.
- Run `ollama rm <model>` to free disk space.
- For production-like reliability, run Ollama as a background service (systemd on Linux, or startup app on Windows/Mac).

---

### 🛠️ SETUP GUIDE — Windows | macOS | Linux

**Step 1: Install Ollama**
- Go to **https://ollama.com/download**
- Click the button for your OS (Windows / macOS / Linux) and download the installer.
- **Windows:** run `OllamaSetup.exe`, click through with all defaults — no options to change.
- **macOS:** open the `.dmg`, drag Ollama into Applications, open it once (allow it in Security settings if blocked).
- **Linux:** run `curl -fsSL https://ollama.com/install.sh | sh` in terminal.
- Check it worked: open terminal and run `ollama --version`.

**Step 2: Download a model**
```
ollama pull phi3
```
(Use `llama3` instead if you have 16GB+ RAM for better quality. Don't pull both at first.)

**Step 3: Install Node.js (needed for n8n)**
- Go to **https://nodejs.org**
- Download the **LTS** version (not "Current") for your OS.
- Windows/Mac: run installer, keep all defaults, click Next → Next → Install.
- Linux: use your package manager, e.g. `sudo apt install nodejs npm`, or use nvm for the LTS version.
- Check: `node --version`

**Step 4: Run n8n**
```
npx n8n
```
- First run downloads packages (2-3 min) — normal, wait.
- If asked about telemetry/diagnostics, answer `n` (optional, privacy).
- When you see `Editor is now accessible via: http://localhost:5678`, open that URL in your browser.
- Create a local account (name/email/password) — stays on your machine only.

**Step 5: Connect n8n to Ollama (same on all OS)**
- Add an **HTTP Request** node → Method: `POST` → URL: `http://localhost:11434/api/generate`
- Turn **Send Body** ON → Body Content Type: `JSON`
- Paste:
```json
{ "model": "phi3", "prompt": "Hello!", "stream": false }
```
- Click "Test step" — you should see a `response` field in the output.

**Quick checklist (all OS):**
| Setting | What to do |
|---|---|
| Ollama installer | All defaults |
| Node.js version | LTS only |
| n8n telemetry prompt | `n` (optional) |
| HTTP Request → Method | change GET → POST |
| HTTP Request → Send Body | turn ON |
| Body Content Type | JSON |

---
---

## 🇧🇩 বাংলা (Bengali)

### এই গাইড কেন?
বেশিরভাগ n8n + AI গাইড তোমাকে OpenAI API ব্যবহার করতে বলে, যেখানে প্রতিটা রিকোয়েস্টে টাকা খরচ হয়। এই গাইডে দেখানো হবে কিভাবে **n8n কে Ollama (লোকাল LLM যেমন Llama 3, Mistral, Phi-3)** এর সাথে কানেক্ট করে সম্পূর্ণ ফ্রি অটোমেশন ওয়ার্কফ্লো বানানো যায়, যা তোমার নিজের কম্পিউটারে চলবে।

---

### ধাপ ১: Ollama ইনস্টল করো
**কী করতে হবে:** তোমার কম্পিউটারে Ollama ডাউনলোড ও ইনস্টল করো।
- Windows/Mac: অফিসিয়াল ওয়েবসাইট থেকে ডাউনলোড করে ইনস্টলার রান করো।
- Linux: টার্মিনালে ইনস্টল স্ক্রিপ্ট রান করো।

**ডিভাইস অনুযায়ী Difficulty:**
- 🟢 সহজ (8GB+ RAM): ছোট মডেল (Phi-3, Gemma 2B) দিয়ে স্মুথ চলবে।
- 🟡 মাঝারি (4-8GB RAM): শুধু সবচেয়ে ছোট মডেল (TinyLlama) ব্যবহার করো।
- 🔴 কঠিন (<4GB RAM): মডেল লোড না হতে পারে।

**সমস্যা ও সমাধান:**
- *সমস্যা:* ইনস্টলের পর "Command not found" (Windows)।
  *সমাধান:* টার্মিনাল/পিসি রিস্টার্ট দাও; প্রয়োজনে পুনরায় ইনস্টল করো।
- *সমস্যা:* Antivirus ইনস্টলার ব্লক করছে।
  *সমাধান:* অফিসিয়াল সোর্স থেকে ডাউনলোড করো এবং সাময়িকভাবে allow করো।

---

### ধাপ ২: মডেল ডাউনলোড করো (Pull)
**কী করতে হবে:** টার্মিনালে কমান্ড দিয়ে একটা মডেল ডাউনলোড করো (যেমন `llama3` বা `phi3`)।

**Difficulty:**
- 🟢 সহজ: ভালো ইন্টারনেট + 8GB RAM থাকলে `llama3` নাও।
- 🟡 মাঝারি: কম RAM থাকলে `phi3` বা `gemma:2b` নাও।
- 🔴 কঠিন: স্লো ইন্টারনেটে ডাউনলোড আটকে যেতে পারে।

**সমস্যা ও সমাধান:**
- *সমস্যা:* ডাউনলোড মাঝখানে আটকে যায়।
  *সমাধান:* একই কমান্ড আবার রান করো — Ollama যেখানে থেমেছিল সেখান থেকে চালিয়ে যাবে।
- *সমস্যা:* "Out of memory" এরর।
  *সমাধান:* ছোট মডেলে সুইচ করো (যেমন `llama3` থেকে `phi3`)।

---

### ধাপ ৩: লোকালি Ollama টেস্ট করো
**কী করতে হবে:** টার্মিনালে সরাসরি মডেল চালিয়ে দেখো রেসপন্স আসছে কিনা।

**Difficulty:**
- 🟢 সহজ: ধাপ ২ ঠিকমতো হলে এটা কাজ করবে।

**সমস্যা ও সমাধান:**
- *সমস্যা:* মডেল লোড হয় কিন্তু খুব স্লো রেসপন্স দেয়।
  *সমাধান:* অন্য হেভি অ্যাপ বন্ধ করে RAM ফ্রি করো — কম র‍্যামের ডিভাইসে এটা স্বাভাবিক।

---

### ধাপ ৪: Ollama কে API হিসেবে এক্সপোজ করো
**কী করতে হবে:** Ollama অটোমেটিক্যালি একটা লোকাল API সার্ভার চালায় (`http://localhost:11434`)। এটা চলছে কিনা কনফার্ম করো।

**Difficulty:**
- 🟢 সহজ: ডিফল্ট সেটাপ সব OS-এ কাজ করে।

**সমস্যা ও সমাধান:**
- *সমস্যা:* n8n যদি Docker-এ চলে, `localhost:11434` কানেক্ট করতে পারে না।
  *সমাধান:* `localhost` এর জায়গায় `http://host.docker.internal:11434` ব্যবহার করো (Windows/Mac)। Linux-এ host মেশিনের IP address ব্যবহার করো বা `--network=host` দিয়ে n8n রান করো।

---

### ধাপ ৫: n8n সেটআপ করো
**কী করতে হবে:** npm, Docker, বা desktop app দিয়ে n8n ইনস্টল করে এডিটর ওপেন করো।

**Difficulty:**
- 🟢 সহজ: Node.js থাকলে `npx n8n` দিয়ে চলবে।
- 🟡 মাঝারি: Docker সেটআপের জন্য বেসিক Docker জ্ঞান লাগবে।

**সমস্যা ও সমাধান:**
- *সমস্যা:* "Port 5678 already in use"।
  *সমাধান:* ওই পোর্ট ব্যবহার করা প্রসেস বন্ধ করো, বা `N8N_PORT` এনভায়রনমেন্ট ভ্যারিয়েবল দিয়ে আলাদা পোর্টে n8n চালাও।
- *সমস্যা:* Node ভার্সনের কারণে npm install ফেল করে।
  *সমাধান:* Node.js LTS ভার্সন (18 বা 20) ব্যবহার করো।

---

### ধাপ ৬: HTTP Request Node দিয়ে n8n-কে Ollama এর সাথে কানেক্ট করো
**কী করতে হবে:** n8n-এ একটা **HTTP Request** নোড যুক্ত করো:
- Method: POST
- URL: `http://localhost:11434/api/generate` (Docker হলে `host.docker.internal`)
- Body (JSON): `model` নাম এবং `prompt` টেক্সট দিয়ে
- "Response Format" JSON সেট করো

**Difficulty:**
- 🟢 সহজ: ধাপ ৪ এর নেটওয়ার্কিং ঠিক থাকলে কাজ করবে।

**সমস্যা ও সমাধান:**
- *সমস্যা:* "Connection refused" এরর।
  *সমাধান:* Ollama চলছে কিনা চেক করো (`ollama serve`) এবং URL/পোর্ট ঠিক আছে কিনা দেখো।
- *সমস্যা:* রেসপন্স খালি আসে বা streaming ফরম্যাট n8n-কে কনফিউজ করে।
  *সমাধান:* JSON body-তে `"stream": false` যুক্ত করো।

---

### ধাপ ৭: তোমার প্রথম ওয়ার্কফ্লো বানাও
**কী করতে হবে:** উদাহরণ — Trigger (Manual/Webhook) → HTTP Request (Ollama) → Output Process (Set/Code node) → Action (ইমেইল পাঠানো, Google Sheets-এ সেভ করা)

**Difficulty:**
- 🟢 সহজ: আগের ধাপগুলো শেষ হলে যেকোনো ডিভাইসে।
- 🟡 মাঝারি: হেভি মডেল হলে 5-30 সেকেন্ড সময় লাগতে পারে — HTTP Request নোডে timeout বাড়াও।

**সমস্যা ও সমাধান:**
- *সমস্যা:* ওয়ার্কফ্লো টাইমআউট হয়।
  *সমাধান:* HTTP Request নোডের Timeout সেটিং বাড়াও (যেমন 60000ms বা বেশি)।
- *সমস্যা:* Output-এ JSON parsing এরর।
  *সমাধান:* Ollama-র JSON আউটপুট থেকে `response` ফিল্ড এক্সট্র্যাক্ট করতে "Set" বা "Code" নোড ব্যবহার করো।

---

### ধাপ ৮: তোমার হার্ডওয়্যার অনুযায়ী অপ্টিমাইজ করো

| ডিভাইস টাইপ | সাজেস্টেড মডেল | নোট |
|---|---|---|
| 16GB+ RAM, GPU | llama3 8B, mistral | ফাস্ট, ভালো কোয়ালিটি |
| 8GB RAM, no GPU | phi3, gemma:2b | ব্যালেন্সড |
| 4GB RAM | tinyllama, phi3:mini | শুধু বেসিক কাজ |

**সমস্যা ও সমাধান:**
- *সমস্যা:* রিয়েল-টাইম অটোমেশনের জন্য মডেল স্লো।
  *সমাধান:* কোয়ান্টাইজড ছোট মডেল (`model:q4`) ব্যবহার করো।

---

### শেষ টিপস
- ডাউনলোড করা মডেল দেখতে `ollama list` রান করো।
- জায়গা খালি করতে `ollama rm <model>` রান করো।
- প্রোডাকশনের জন্য Ollama-কে background service হিসেবে চালাও (Linux-এ systemd, Windows/Mac-এ startup app)।

---

### 🛠️ সেটআপ গাইড — Windows | macOS | Linux

**Step ১: Ollama ইনস্টল করো**
- যাও **https://ollama.com/download**
- তোমার OS (Windows/macOS/Linux) অনুযায়ী বাটনে ক্লিক করে ডাউনলোড করো।
- **Windows:** `OllamaSetup.exe` রান করো, সব ডিফল্ট রেখে Next/Install — কোনো অপশন চেঞ্জ করার নেই।
- **macOS:** `.dmg` ফাইল ওপেন করে Ollama-কে Applications ফোল্ডারে ড্র্যাগ করো, একবার ওপেন করো (ব্লক করলে Security সেটিংসে allow করো)।
- **Linux:** টার্মিনালে রান করো: `curl -fsSL https://ollama.com/install.sh | sh`
- চেক করো: টার্মিনালে `ollama --version`

**Step ২: একটা মডেল ডাউনলোড করো**
```
ollama pull phi3
```
(16GB+ RAM থাকলে `llama3` নিতে পারো বেটার কোয়ালিটির জন্য — দুটো একসাথে না নাও।)

**Step ৩: Node.js ইনস্টল করো (n8n চালানোর জন্য)**
- যাও **https://nodejs.org**
- **LTS** ভার্সন ডাউনলোড করো (Current না)
- Windows/Mac: ইনস্টলার রান করো, সব ডিফল্ট রেখে Next → Next → Install
- Linux: প্যাকেজ ম্যানেজার দিয়ে, যেমন `sudo apt install nodejs npm`, বা nvm দিয়ে LTS ইনস্টল করো
- চেক করো: `node --version`

**Step ৪: n8n রান করো**
```
npx n8n
```
- প্রথমবার কিছু প্যাকেজ ডাউনলোড হবে (২-৩ মিনিট) — নরমাল, অপেক্ষা করো
- Telemetry প্রশ্ন আসলে `n` দিতে পারো (optional, প্রাইভেসির জন্য)
- `Editor is now accessible via: http://localhost:5678` দেখলে ব্রাউজারে ওই লিংক ওপেন করো
- নাম/ইমেইল/পাসওয়ার্ড দিয়ে লোকাল account বানাও (শুধু তোমার পিসিতে থাকবে)

**Step ৫: n8n-কে Ollama-র সাথে কানেক্ট করো (সব OS-এ একই)**
- **HTTP Request** node যুক্ত করো → Method: `POST` → URL: `http://localhost:11434/api/generate`
- **Send Body** ON করো → Body Content Type: `JSON`
- পেস্ট করো:
```json
{ "model": "phi3", "prompt": "Hello!", "stream": false }
```
- "Test step" ক্লিক করো — আউটপুটে `response` ফিল্ড দেখাবে

**চেকলিস্ট (সব OS):**
| সেটিং | কী করতে হবে |
|---|---|
| Ollama installer | সব ডিফল্ট |
| Node.js version | LTS-ই নিতে হবে |
| n8n telemetry প্রশ্ন | `n` (optional) |
| HTTP Request → Method | GET থেকে POST |
| HTTP Request → Send Body | ON করো |
| Body Content Type | JSON |

---
---

## 🇮🇳 हिन्दी (Hindi)

### यह गाइड क्यों?
ज़्यादातर n8n + AI गाइड्स आपको OpenAI API की तरफ ले जाते हैं, जिसमें हर रिक्वेस्ट का पैसा लगता है। यह गाइड दिखाएगा कि **n8n को Ollama (Llama 3, Mistral, Phi-3 जैसे लोकल LLM)** से कैसे जोड़ें ताकि आप अपने कंप्यूटर पर **पूरी तरह फ्री** ऑटोमेशन वर्कफ़्लो बना सकें।

---

### चरण 1: Ollama इंस्टॉल करें
- Windows/Mac: ऑफिशियल वेबसाइट से डाउनलोड कर इंस्टॉलर चलाएं।
- Linux: टर्मिनल में इंस्टॉल स्क्रिप्ट चलाएं।

**Difficulty:**
- 🟢 आसान (8GB+ RAM): छोटे मॉडल्स (Phi-3, Gemma 2B) स्मूथ चलेंगे।
- 🟡 मध्यम (4-8GB RAM): सिर्फ सबसे छोटे मॉडल (TinyLlama) इस्तेमाल करें।
- 🔴 कठिन (<4GB RAM): मॉडल लोड होने में दिक्कत हो सकती है।

**समस्या व समाधान:**
- *समस्या:* इंस्टॉल के बाद "Command not found" (Windows)।
  *समाधान:* टर्मिनल/PC रीस्टार्ट करें; ज़रूरत हो तो फिर से इंस्टॉल करें।
- *समस्या:* एंटीवायरस इंस्टॉलर को ब्लॉक कर रहा है।
  *समाधान:* केवल ऑफिशियल सोर्स से डाउनलोड करें और अस्थायी रूप से अनुमति दें।

---

### चरण 2: मॉडल डाउनलोड करें (Pull)
टर्मिनल में कमांड से मॉडल डाउनलोड करें (जैसे `llama3` या `phi3`)।

**Difficulty:**
- 🟢 आसान: अच्छा इंटरनेट + 8GB RAM → `llama3`
- 🟡 मध्यम: कम RAM → `phi3` या `gemma:2b`
- 🔴 कठिन: स्लो इंटरनेट में डाउनलोड बीच में रुक सकता है।

**समस्या व समाधान:**
- *समस्या:* डाउनलोड बीच में रुक जाता है।
  *समाधान:* वही कमांड फिर चलाएं — Ollama वहीं से जारी रखेगा।
- *समस्या:* "Out of memory" एरर।
  *समाधान:* छोटे मॉडल पर स्विच करें (जैसे `llama3` से `phi3`)।

---

### चरण 3: Ollama को लोकली टेस्ट करें
टर्मिनल में मॉडल चलाकर देखें कि रिस्पॉन्स आ रहा है या नहीं।

**Difficulty:**
- 🟢 आसान: चरण 2 सही हुआ तो यह काम करेगा।

**समस्या व समाधान:**
- *समस्या:* मॉडल बहुत धीरे जवाब देता है।
  *समाधान:* दूसरे हैवी ऐप बंद करें ताकि RAM खाली हो — कम-RAM डिवाइस पर यह सामान्य है।

---

### चरण 4: Ollama को API के रूप में एक्सपोज़ करें
Ollama अपने आप एक लोकल API सर्वर चलाता है (`http://localhost:11434`)।

**Difficulty:**
- 🟢 आसान: डिफ़ॉल्ट सेटअप हर OS पर काम करता है।

**समस्या व समाधान:**
- *समस्या:* Docker में चल रहा n8n `localhost:11434` से कनेक्ट नहीं हो पाता।
  *समाधान:* Windows/Mac पर `localhost` के बजाय `http://host.docker.internal:11434` इस्तेमाल करें। Linux पर होस्ट मशीन का IP या `--network=host` इस्तेमाल करें।

---

### चरण 5: n8n सेटअप करें
npm, Docker, या desktop app से n8n इंस्टॉल करें।

**Difficulty:**
- 🟢 आसान: Node.js हो तो `npx n8n` से काम चलेगा।
- 🟡 मध्यम: Docker सेटअप के लिए बेसिक Docker जानकारी चाहिए।

**समस्या व समाधान:**
- *समस्या:* "Port 5678 already in use"।
  *समाधान:* उस पोर्ट का इस्तेमाल कर रही प्रक्रिया बंद करें, या `N8N_PORT` से अलग पोर्ट सेट करें।
- *समस्या:* Node वर्शन के कारण npm install फेल।
  *समाधान:* Node.js LTS (18 या 20) इस्तेमाल करें।

---

### चरण 6: HTTP Request Node से n8n को Ollama से जोड़ें
- Method: POST
- URL: `http://localhost:11434/api/generate` (Docker में `host.docker.internal`)
- Body (JSON): `model` नाम और `prompt` टेक्स्ट
- "Response Format" को JSON सेट करें

**Difficulty:**
- 🟢 आसान: चरण 4 की नेटवर्किंग सही हो तो काम करेगा।

**समस्या व समाधान:**
- *समस्या:* "Connection refused" एरर।
  *समाधान:* Ollama चल रहा है या नहीं चेक करें (`ollama serve`) और URL/पोर्ट सही है या नहीं देखें।
- *समस्या:* रिस्पॉन्स खाली आता है या streaming फॉर्मेट से n8n कन्फ्यूज़ होता है।
  *समाधान:* JSON body में `"stream": false` जोड़ें।

---

### चरण 7: अपना पहला वर्कफ़्लो बनाएं
उदाहरण: Trigger (Manual/Webhook) → HTTP Request (Ollama) → Output Process (Set/Code node) → Action (ईमेल भेजना, Google Sheets में सेव करना)

**Difficulty:**
- 🟢 आसान: पिछले चरण पूरे होने पर किसी भी डिवाइस पर।
- 🟡 मध्यम: हैवी मॉडल में 5-30 सेकंड लग सकते हैं — timeout बढ़ाएं।

**समस्या व समाधान:**
- *समस्या:* वर्कफ़्लो टाइमआउट हो जाता है।
  *समाधान:* HTTP Request नोड का Timeout बढ़ाएं (60000ms या ज़्यादा)।
- *समस्या:* आउटपुट में JSON parsing एरर।
  *समाधान:* Ollama के JSON आउटपुट से `response` फ़ील्ड निकालने के लिए "Set"/"Code" नोड इस्तेमाल करें।

---

### चरण 8: अपने हार्डवेयर के अनुसार ऑप्टिमाइज़ करें

| डिवाइस टाइप | सुझाया मॉडल | नोट |
|---|---|---|
| 16GB+ RAM, GPU | llama3 8B, mistral | तेज़, अच्छी क्वालिटी |
| 8GB RAM, no GPU | phi3, gemma:2b | बैलेंस्ड |
| 4GB RAM | tinyllama, phi3:mini | सिर्फ बेसिक काम |

**समस्या व समाधान:**
- *समस्या:* रियल-टाइम ऑटोमेशन के लिए मॉडल धीमा।
  *समाधान:* क्वांटाइज़्ड छोटे मॉडल (`model:q4`) इस्तेमाल करें।

---

### आखिरी टिप्स
- डाउनलोड किए मॉडल देखने के लिए `ollama list` चलाएं।
- जगह खाली करने के लिए `ollama rm <model>` चलाएं।
- प्रोडक्शन के लिए Ollama को बैकग्राउंड सर्विस के रूप में चलाएं।

---

### 🛠️ सेटअप गाइड — Windows | macOS | Linux

**चरण 1: Ollama इंस्टॉल करें**
- जाएं **https://ollama.com/download**
- अपने OS (Windows/macOS/Linux) के लिए बटन पर क्लिक करें।
- **Windows:** `OllamaSetup.exe` चलाएं, सब डिफ़ॉल्ट रखें, Next/Install — कुछ बदलने की ज़रूरत नहीं।
- **macOS:** `.dmg` खोलें, Ollama को Applications में ड्रैग करें, एक बार खोलें (ब्लॉक हो तो Security सेटिंग्स में allow करें)।
- **Linux:** टर्मिनल में चलाएं: `curl -fsSL https://ollama.com/install.sh | sh`
- जांचें: `ollama --version`

**चरण 2: एक मॉडल डाउनलोड करें**
```
ollama pull phi3
```
(16GB+ RAM हो तो बेहतर क्वालिटी के लिए `llama3` लें — दोनों एक साथ न लें।)

**चरण 3: Node.js इंस्टॉल करें (n8n के लिए ज़रूरी)**
- जाएं **https://nodejs.org**
- **LTS** वर्शन डाउनलोड करें (Current नहीं)
- Windows/Mac: इंस्टॉलर चलाएं, सब डिफ़ॉल्ट रखें, Next → Next → Install
- Linux: पैकेज मैनेजर से, जैसे `sudo apt install nodejs npm`, या nvm से LTS इंस्टॉल करें
- जांचें: `node --version`

**चरण 4: n8n चलाएं**
```
npx n8n
```
- पहली बार कुछ पैकेज डाउनलोड होंगे (2-3 मिनट) — सामान्य है, इंतज़ार करें
- Telemetry सवाल आए तो `n` दें (वैकल्पिक, प्राइवेसी के लिए)
- `Editor is now accessible via: http://localhost:5678` दिखे तो ब्राउज़र में वो लिंक खोलें
- नाम/ईमेल/पासवर्ड से लोकल अकाउंट बनाएं (केवल आपके PC पर रहेगा)

**चरण 5: n8n को Ollama से जोड़ें (सभी OS पर एक जैसा)**
- **HTTP Request** node जोड़ें → Method: `POST` → URL: `http://localhost:11434/api/generate`
- **Send Body** ON करें → Body Content Type: `JSON`
- पेस्ट करें:
```json
{ "model": "phi3", "prompt": "Hello!", "stream": false }
```
- "Test step" क्लिक करें — आउटपुट में `response` फ़ील्ड दिखेगा

**चेकलिस्ट (सभी OS):**
| सेटिंग | क्या करें |
|---|---|
| Ollama installer | सब डिफ़ॉल्ट |
| Node.js version | केवल LTS |
| n8n telemetry सवाल | `n` (वैकल्पिक) |
| HTTP Request → Method | GET से POST |
| HTTP Request → Send Body | ON करें |
| Body Content Type | JSON |

---
---

## 🇪🇸 ESPAÑOL (Spanish)

### ¿Por qué esta guía?
La mayoría de guías de n8n + IA te dirigen hacia la API de OpenAI, que cobra por cada solicitud. Esta guía muestra cómo conectar **n8n con Ollama (LLMs locales como Llama 3, Mistral, Phi-3)** para crear flujos de automatización **completamente gratis**, en tu propia máquina.

---

### Paso 1: Instalar Ollama
- Windows/Mac: descarga desde el sitio oficial y ejecuta el instalador.
- Linux: ejecuta el script de instalación en terminal.

**Dificultad:**
- 🟢 Fácil (8GB+ RAM): modelos pequeños (Phi-3, Gemma 2B) funcionan bien.
- 🟡 Media (4-8GB RAM): usa solo modelos muy pequeños (TinyLlama).
- 🔴 Difícil (<4GB RAM): puede fallar al cargar modelos.

**Problemas y soluciones:**
- *Problema:* "Command not found" tras instalar (Windows).
  *Solución:* Reinicia la terminal/PC; reinstala si es necesario.
- *Problema:* El antivirus bloquea el instalador.
  *Solución:* Descarga solo desde la fuente oficial y permite temporalmente.

---

### Paso 2: Descargar un modelo (Pull)
Ejecuta un comando en terminal para descargar un modelo (ej. `llama3` o `phi3`).

**Dificultad:**
- 🟢 Fácil: buena conexión + 8GB RAM → `llama3`
- 🟡 Media: RAM limitada → `phi3` o `gemma:2b`
- 🔴 Difícil: internet lento puede hacer que la descarga falle.

**Problemas y soluciones:**
- *Problema:* La descarga se detiene a la mitad.
  *Solución:* Ejecuta el mismo comando otra vez — Ollama continúa donde lo dejó.
- *Problema:* Error "Out of memory".
  *Solución:* Cambia a un modelo más pequeño (de `llama3` a `phi3`).

---

### Paso 3: Probar Ollama localmente
Ejecuta el modelo directamente en terminal para confirmar que responde.

**Dificultad:**
- 🟢 Fácil: si el paso 2 funcionó correctamente.

**Problemas y soluciones:**
- *Problema:* El modelo responde muy lento.
  *Solución:* Cierra otras apps pesadas para liberar RAM — normal en equipos de gama baja.

---

### Paso 4: Exponer Ollama como API
Ollama ejecuta automáticamente un servidor API local (`http://localhost:11434`).

**Dificultad:**
- 🟢 Fácil: la configuración predeterminada funciona en todos los SO.

**Problemas y soluciones:**
- *Problema:* n8n en Docker no puede acceder a `localhost:11434`.
  *Solución:* Usa `http://host.docker.internal:11434` en lugar de `localhost` (Windows/Mac). En Linux usa la IP real del host o `--network=host`.

---

### Paso 5: Configurar n8n
Instala n8n vía npm, Docker, o app de escritorio.

**Dificultad:**
- 🟢 Fácil: con Node.js, `npx n8n` funciona.
- 🟡 Media: Docker requiere conocimientos básicos.

**Problemas y soluciones:**
- *Problema:* "Port 5678 already in use".
  *Solución:* Detén el proceso que usa ese puerto, o usa `N8N_PORT` para otro puerto.
- *Problema:* npm install falla por versión de Node.
  *Solución:* Usa Node.js LTS (18 o 20).

---

### Paso 6: Conectar n8n con Ollama vía HTTP Request Node
- Método: POST
- URL: `http://localhost:11434/api/generate` (o `host.docker.internal` en Docker)
- Body (JSON): incluye `model` y `prompt`
- Configura "Response Format" como JSON

**Dificultad:**
- 🟢 Fácil: funciona si el paso 4 está bien configurado.

**Problemas y soluciones:**
- *Problema:* Error "Connection refused".
  *Solución:* Verifica que Ollama esté corriendo (`ollama serve`) y que la URL/puerto coincidan.
- *Problema:* La respuesta llega vacía o el formato streaming confunde a n8n.
  *Solución:* Agrega `"stream": false` en el body JSON.

---

### Paso 7: Construir tu primer flujo
Ejemplo: Trigger (Manual/Webhook) → HTTP Request (Ollama) → Procesar salida (Set/Code) → Acción (enviar email, guardar en Google Sheets)

**Dificultad:**
- 🟢 Fácil: en cualquier dispositivo tras completar pasos anteriores.
- 🟡 Media: modelos pesados tardan 5-30 segundos — aumenta el timeout.

**Problemas y soluciones:**
- *Problema:* El flujo se agota esperando respuesta.
  *Solución:* Aumenta el "Timeout" en HTTP Request (60000ms o más).
- *Problema:* Error de parseo JSON en la salida.
  *Solución:* Usa un nodo "Set"/"Code" para extraer el campo `response`.

---

### Paso 8: Optimizar según tu hardware

| Tipo de dispositivo | Modelo recomendado | Notas |
|---|---|---|
| 16GB+ RAM, GPU | llama3 8B, mistral | Rápido, buena calidad |
| 8GB RAM, sin GPU | phi3, gemma:2b | Equilibrado |
| 4GB RAM | tinyllama, phi3:mini | Solo tareas básicas |

**Problemas y soluciones:**
- *Problema:* Modelo demasiado lento para automatización en tiempo real.
  *Solución:* Usa versiones cuantizadas (`model:q4`).

---

### Consejos finales
- `ollama list` para ver modelos descargados.
- `ollama rm <model>` para liberar espacio.
- Ejecuta Ollama como servicio en segundo plano para mayor fiabilidad.

---

### 🛠️ GUÍA DE INSTALACIÓN — Windows | macOS | Linux

**Paso 1: Instalar Ollama**
- Ve a **https://ollama.com/download**
- Haz clic en el botón para tu SO (Windows/macOS/Linux).
- **Windows:** ejecuta `OllamaSetup.exe`, deja todo por defecto, Next/Install — nada que cambiar.
- **macOS:** abre el `.dmg`, arrastra Ollama a Aplicaciones, ábrelo una vez (permite en Seguridad si se bloquea).
- **Linux:** en terminal: `curl -fsSL https://ollama.com/install.sh | sh`
- Verifica: `ollama --version`

**Paso 2: Descargar un modelo**
```
ollama pull phi3
```
(Con 16GB+ RAM puedes usar `llama3` para mejor calidad — no descargues ambos al inicio.)

**Paso 3: Instalar Node.js (necesario para n8n)**
- Ve a **https://nodejs.org**
- Descarga la versión **LTS** (no "Current")
- Windows/Mac: ejecuta el instalador, deja todo por defecto, Next → Next → Install
- Linux: usa tu gestor de paquetes, ej. `sudo apt install nodejs npm`, o nvm para LTS
- Verifica: `node --version`

**Paso 4: Ejecutar n8n**
```
npx n8n
```
- La primera vez descarga paquetes (2-3 min) — normal, espera
- Si pregunta sobre telemetría, responde `n` (opcional, privacidad)
- Cuando veas `Editor is now accessible via: http://localhost:5678`, abre ese enlace en tu navegador
- Crea una cuenta local (nombre/email/contraseña) — queda solo en tu equipo

**Paso 5: Conectar n8n con Ollama (igual en todos los SO)**
- Agrega un nodo **HTTP Request** → Method: `POST` → URL: `http://localhost:11434/api/generate`
- Activa **Send Body** → Body Content Type: `JSON`
- Pega:
```json
{ "model": "phi3", "prompt": "Hello!", "stream": false }
```
- Haz clic en "Test step" — verás un campo `response` en la salida

**Lista de verificación (todos los SO):**
| Ajuste | Qué hacer |
|---|---|
| Instalador de Ollama | Todo por defecto |
| Versión de Node.js | Solo LTS |
| Pregunta de telemetría n8n | `n` (opcional) |
| HTTP Request → Method | GET → POST |
| HTTP Request → Send Body | Activar |
| Body Content Type | JSON |

---
---

## 🇫🇷 FRANÇAIS (French)

### Pourquoi ce guide ?
La plupart des guides n8n + IA vous orientent vers l'API OpenAI, payante par requête. Ce guide montre comment connecter **n8n à Ollama (LLMs locaux comme Llama 3, Mistral, Phi-3)** pour créer des flux d'automatisation **entièrement gratuits**, sur votre propre machine.

---

### Étape 1 : Installer Ollama
- Windows/Mac : téléchargez depuis le site officiel et lancez l'installateur.
- Linux : exécutez le script d'installation dans le terminal.

**Difficulté :**
- 🟢 Facile (8GB+ RAM) : les petits modèles (Phi-3, Gemma 2B) fonctionnent bien.
- 🟡 Moyenne (4-8GB RAM) : utilisez uniquement les plus petits modèles (TinyLlama).
- 🔴 Difficile (<4GB RAM) : le chargement des modèles peut échouer.

**Problèmes et solutions :**
- *Problème :* "Command not found" après installation (Windows).
  *Solution :* Redémarrez le terminal/PC ; réinstallez si nécessaire.
- *Problème :* L'antivirus bloque l'installateur.
  *Solution :* Téléchargez uniquement depuis la source officielle et autorisez temporairement.

---

### Étape 2 : Télécharger un modèle (Pull)
Exécutez une commande dans le terminal pour télécharger un modèle (ex. `llama3` ou `phi3`).

**Difficulté :**
- 🟢 Facile : bonne connexion + 8GB RAM → `llama3`
- 🟡 Moyenne : RAM limitée → `phi3` ou `gemma:2b`
- 🔴 Difficile : internet lent peut faire échouer le téléchargement.

**Problèmes et solutions :**
- *Problème :* Le téléchargement se bloque.
  *Solution :* Relancez la même commande — Ollama reprend où il s'est arrêté.
- *Problème :* Erreur "Out of memory".
  *Solution :* Passez à un modèle plus petit (de `llama3` à `phi3`).

---

### Étape 3 : Tester Ollama localement
Exécutez le modèle directement dans le terminal pour confirmer qu'il répond.

**Difficulté :**
- 🟢 Facile : si l'étape 2 a réussi.

**Problèmes et solutions :**
- *Problème :* Le modèle répond très lentement.
  *Solution :* Fermez les autres applications lourdes pour libérer de la RAM — normal sur les appareils peu puissants.

---

### Étape 4 : Exposer Ollama comme API
Ollama exécute automatiquement un serveur API local (`http://localhost:11434`).

**Difficulté :**
- 🟢 Facile : la configuration par défaut fonctionne sur tous les OS.

**Problèmes et solutions :**
- *Problème :* n8n dans Docker ne peut pas accéder à `localhost:11434`.
  *Solution :* Utilisez `http://host.docker.internal:11434` au lieu de `localhost` (Windows/Mac). Sur Linux, utilisez l'IP de la machine hôte ou `--network=host`.

---

### Étape 5 : Configurer n8n
Installez n8n via npm, Docker, ou l'application de bureau.

**Difficulté :**
- 🟢 Facile : avec Node.js, `npx n8n` fonctionne.
- 🟡 Moyenne : Docker nécessite des connaissances de base.

**Problèmes et solutions :**
- *Problème :* "Port 5678 already in use".
  *Solution :* Arrêtez le processus utilisant ce port, ou utilisez `N8N_PORT` pour un autre port.
- *Problème :* npm install échoue à cause de la version de Node.
  *Solution :* Utilisez Node.js LTS (18 ou 20).

---

### Étape 6 : Connecter n8n à Ollama via HTTP Request Node
- Méthode : POST
- URL : `http://localhost:11434/api/generate` (ou `host.docker.internal` dans Docker)
- Body (JSON) : inclure `model` et `prompt`
- Réglez "Response Format" sur JSON

**Difficulté :**
- 🟢 Facile : fonctionne si l'étape 4 est correctement configurée.

**Problèmes et solutions :**
- *Problème :* Erreur "Connection refused".
  *Solution :* Vérifiez que Ollama fonctionne (`ollama serve`) et que l'URL/port correspondent.
- *Problème :* La réponse arrive vide ou le format streaming perturbe n8n.
  *Solution :* Ajoutez `"stream": false` dans le body JSON.

---

### Étape 7 : Construire votre premier flux
Exemple : Trigger (Manuel/Webhook) → HTTP Request (Ollama) → Traitement (Set/Code) → Action (envoi d'email, sauvegarde dans Google Sheets)

**Difficulté :**
- 🟢 Facile : sur n'importe quel appareil après les étapes précédentes.
- 🟡 Moyenne : les modèles lourds prennent 5-30 secondes — augmentez le timeout.

**Problèmes et solutions :**
- *Problème :* Le flux expire en attendant la réponse.
  *Solution :* Augmentez le "Timeout" du nœud HTTP Request (60000ms ou plus).
- *Problème :* Erreur de parsing JSON en sortie.
  *Solution :* Utilisez un nœud "Set"/"Code" pour extraire le champ `response`.

---

### Étape 8 : Optimiser selon votre matériel

| Type d'appareil | Modèle recommandé | Notes |
|---|---|---|
| 16GB+ RAM, GPU | llama3 8B, mistral | Rapide, bonne qualité |
| 8GB RAM, sans GPU | phi3, gemma:2b | Équilibré |
| 4GB RAM | tinyllama, phi3:mini | Tâches basiques uniquement |

**Problèmes et solutions :**
- *Problème :* Modèle trop lent pour l'automatisation en temps réel.
  *Solution :* Utilisez des versions quantifiées (`model:q4`).

---

### Conseils finaux
- `ollama list` pour voir les modèles téléchargés.
- `ollama rm <model>` pour libérer de l'espace.
- Exécutez Ollama comme service en arrière-plan pour plus de fiabilité.

---

### 🛠️ GUIDE D'INSTALLATION — Windows | macOS | Linux

**Étape 1 : Installer Ollama**
- Allez sur **https://ollama.com/download**
- Cliquez sur le bouton pour votre OS (Windows/macOS/Linux).
- **Windows :** lancez `OllamaSetup.exe`, gardez tout par défaut, Next/Install — rien à changer.
- **macOS :** ouvrez le `.dmg`, glissez Ollama dans Applications, ouvrez-le une fois (autorisez dans Sécurité si bloqué).
- **Linux :** dans le terminal : `curl -fsSL https://ollama.com/install.sh | sh`
- Vérifiez : `ollama --version`

**Étape 2 : Télécharger un modèle**
```
ollama pull phi3
```
(Avec 16GB+ RAM, vous pouvez utiliser `llama3` pour une meilleure qualité — ne téléchargez pas les deux au début.)

**Étape 3 : Installer Node.js (nécessaire pour n8n)**
- Allez sur **https://nodejs.org**
- Téléchargez la version **LTS** (pas "Current")
- Windows/Mac : lancez l'installateur, gardez tout par défaut, Next → Next → Install
- Linux : via votre gestionnaire de paquets, ex. `sudo apt install nodejs npm`, ou nvm pour LTS
- Vérifiez : `node --version`

**Étape 4 : Lancer n8n**
```
npx n8n
```
- La première fois, des paquets se téléchargent (2-3 min) — normal, patientez
- Si une question sur la télémétrie apparaît, répondez `n` (optionnel, confidentialité)
- Quand vous voyez `Editor is now accessible via: http://localhost:5678`, ouvrez ce lien dans votre navigateur
- Créez un compte local (nom/email/mot de passe) — reste sur votre machine

**Étape 5 : Connecter n8n à Ollama (identique sur tous les OS)**
- Ajoutez un nœud **HTTP Request** → Method : `POST` → URL : `http://localhost:11434/api/generate`
- Activez **Send Body** → Body Content Type : `JSON`
- Collez :
```json
{ "model": "phi3", "prompt": "Hello!", "stream": false }
```
- Cliquez sur "Test step" — vous verrez un champ `response` dans la sortie

**Liste de vérification (tous les OS) :**
| Réglage | À faire |
|---|---|
| Installateur Ollama | Tout par défaut |
| Version Node.js | LTS uniquement |
| Question télémétrie n8n | `n` (optionnel) |
| HTTP Request → Method | GET → POST |
| HTTP Request → Send Body | Activer |
| Body Content Type | JSON |

---
---

## 🇸🇦 العربية (Arabic)

### لماذا هذا الدليل؟
معظم أدلة n8n + AI توجهك لاستخدام واجهة OpenAI API التي تتطلب الدفع مقابل كل طلب. يوضح هذا الدليل كيفية ربط **n8n بـ Ollama (نماذج محلية مثل Llama 3 و Mistral و Phi-3)** لبناء تدفقات أتمتة **مجانية بالكامل** تعمل على جهازك الخاص.

---

### الخطوة 1: تثبيت Ollama
- Windows/Mac: نزّل من الموقع الرسمي وشغّل المثبت.
- Linux: شغّل سكريبت التثبيت في الطرفية (terminal).

**مستوى الصعوبة:**
- 🟢 سهل (8GB+ RAM): النماذج الصغيرة (Phi-3, Gemma 2B) تعمل بسلاسة.
- 🟡 متوسط (4-8GB RAM): استخدم فقط أصغر النماذج (TinyLlama).
- 🔴 صعب (أقل من 4GB RAM): قد يفشل تحميل النماذج.

**المشاكل والحلول:**
- *المشكلة:* "Command not found" بعد التثبيت (Windows).
  *الحل:* أعد تشغيل الطرفية/الجهاز؛ أعد التثبيت إذا لزم الأمر.
- *المشكلة:* برنامج الحماية يحظر المثبت.
  *الحل:* نزّل فقط من المصدر الرسمي واسمح به مؤقتاً.

---

### الخطوة 2: تحميل نموذج (Pull)
شغّل أمراً في الطرفية لتحميل نموذج (مثل `llama3` أو `phi3`).

**مستوى الصعوبة:**
- 🟢 سهل: إنترنت جيد + 8GB RAM → `llama3`
- 🟡 متوسط: ذاكرة محدودة → `phi3` أو `gemma:2b`
- 🔴 صعب: الإنترنت البطيء قد يفشل التحميل.

**المشاكل والحلول:**
- *المشكلة:* التحميل يتوقف في المنتصف.
  *الحل:* أعد تشغيل نفس الأمر — يستكمل Ollama من حيث توقف.
- *المشكلة:* خطأ "Out of memory".
  *الحل:* انتقل إلى نموذج أصغر (من `llama3` إلى `phi3`).

---

### الخطوة 3: اختبار Ollama محلياً
شغّل النموذج مباشرة في الطرفية للتأكد من استجابته.

**مستوى الصعوبة:**
- 🟢 سهل: إذا نجحت الخطوة 2.

**المشاكل والحلول:**
- *المشكلة:* النموذج يستجيب ببطء شديد.
  *الحل:* أغلق التطبيقات الثقيلة الأخرى لتحرير الذاكرة — أمر طبيعي على الأجهزة الضعيفة.

---

### الخطوة 4: تعريض Ollama كواجهة API
يشغّل Ollama تلقائياً خادم API محلي (`http://localhost:11434`).

**مستوى الصعوبة:**
- 🟢 سهل: الإعداد الافتراضي يعمل على جميع أنظمة التشغيل.

**المشاكل والحلول:**
- *المشكلة:* n8n داخل Docker لا يستطيع الوصول إلى `localhost:11434`.
  *الحل:* استخدم `http://host.docker.internal:11434` بدلاً من `localhost` (Windows/Mac). على Linux استخدم عنوان IP للجهاز المضيف أو `--network=host`.

---

### الخطوة 5: إعداد n8n
ثبّت n8n عبر npm أو Docker أو تطبيق سطح المكتب.

**مستوى الصعوبة:**
- 🟢 سهل: مع Node.js، يعمل `npx n8n`.
- 🟡 متوسط: Docker يحتاج معرفة أساسية.

**المشاكل والحلول:**
- *المشكلة:* "Port 5678 already in use".
  *الحل:* أوقف العملية التي تستخدم هذا المنفذ، أو استخدم `N8N_PORT` لمنفذ آخر.
- *المشكلة:* فشل npm install بسبب إصدار Node.
  *الحل:* استخدم Node.js LTS (18 أو 20).

---

### الخطوة 6: ربط n8n بـ Ollama عبر HTTP Request Node
- الطريقة: POST
- الرابط: `http://localhost:11434/api/generate` (أو `host.docker.internal` في Docker)
- النص (JSON): يتضمن `model` و `prompt`
- اضبط "Response Format" على JSON

**مستوى الصعوبة:**
- 🟢 سهل: يعمل إذا كانت الخطوة 4 صحيحة.

**المشاكل والحلول:**
- *المشكلة:* خطأ "Connection refused".
  *الحل:* تأكد أن Ollama يعمل (`ollama serve`) وأن الرابط/المنفذ متطابقان.
- *المشكلة:* الاستجابة تأتي فارغة أو تنسيق streaming يربك n8n.
  *الحل:* أضف `"stream": false` في JSON body.

---

### الخطوة 7: بناء أول تدفق عمل
مثال: Trigger (يدوي/Webhook) → HTTP Request (Ollama) → معالجة الإخراج (Set/Code) → إجراء (إرسال بريد، حفظ في Google Sheets)

**مستوى الصعوبة:**
- 🟢 سهل: على أي جهاز بعد إكمال الخطوات السابقة.
- 🟡 متوسط: النماذج الثقيلة قد تستغرق 5-30 ثانية — زد الـ timeout.

**المشاكل والحلول:**
- *المشكلة:* التدفق ينتهي بسبب انتظار الاستجابة (timeout).
  *الحل:* زد قيمة "Timeout" في عقدة HTTP Request (60000ms أو أكثر).
- *المشكلة:* خطأ في تحليل JSON بالإخراج.
  *الحل:* استخدم عقدة "Set"/"Code" لاستخراج حقل `response`.

---

### الخطوة 8: التحسين حسب جهازك

| نوع الجهاز | النموذج المقترح | ملاحظات |
|---|---|---|
| 16GB+ RAM, GPU | llama3 8B, mistral | سريع، جودة عالية |
| 8GB RAM, بدون GPU | phi3, gemma:2b | متوازن |
| 4GB RAM | tinyllama, phi3:mini | مهام بسيطة فقط |

**المشاكل والحلول:**
- *المشكلة:* النموذج بطيء جداً للأتمتة في الوقت الفعلي.
  *الحل:* استخدم نسخاً مكمّمة (`model:q4`).

---

### نصائح أخيرة
- `ollama list` لرؤية النماذج المحملة.
- `ollama rm <model>` لتحرير المساحة.
- شغّل Ollama كخدمة في الخلفية لمزيد من الاستقرار.

---

### 🛠️ دليل التثبيت — Windows | macOS | Linux

**الخطوة 1: تثبيت Ollama**
- اذهب إلى **https://ollama.com/download**
- اضغط على الزر الخاص بنظامك (Windows/macOS/Linux).
- **Windows:** شغّل `OllamaSetup.exe`، اترك كل شيء افتراضي، Next/Install — لا تغيير.
- **macOS:** افتح `.dmg`، اسحب Ollama إلى Applications، افتحه مرة واحدة (اسمح به في إعدادات Security إذا تم حظره).
- **Linux:** في الطرفية: `curl -fsSL https://ollama.com/install.sh | sh`
- تحقق: `ollama --version`

**الخطوة 2: تحميل نموذج**
```
ollama pull phi3
```
(مع 16GB+ RAM يمكنك استخدام `llama3` لجودة أفضل — لا تحمّل الاثنين في البداية.)

**الخطوة 3: تثبيت Node.js (مطلوب لـ n8n)**
- اذهب إلى **https://nodejs.org**
- نزّل نسخة **LTS** (وليس "Current")
- Windows/Mac: شغّل المثبت، اترك كل شيء افتراضي، Next → Next → Install
- Linux: عبر مدير الحزم، مثل `sudo apt install nodejs npm`، أو nvm لتثبيت LTS
- تحقق: `node --version`

**الخطوة 4: تشغيل n8n**
```
npx n8n
```
- أول مرة سيتم تحميل بعض الحزم (2-3 دقائق) — طبيعي، انتظر
- إذا ظهر سؤال عن telemetry، اختر `n` (اختياري، للخصوصية)
- عند ظهور `Editor is now accessible via: http://localhost:5678`، افتح الرابط في المتصفح
- أنشئ حساباً محلياً (اسم/بريد/كلمة مرور) — يبقى على جهازك فقط

**الخطوة 5: ربط n8n بـ Ollama (نفس الطريقة في جميع الأنظمة)**
- أضف عقدة **HTTP Request** ← Method: `POST` ← URL: `http://localhost:11434/api/generate`
- فعّل **Send Body** ← Body Content Type: `JSON`
- ألصق:
```json
{ "model": "phi3", "prompt": "Hello!", "stream": false }
```
- اضغط "Test step" — سترى حقل `response` في الإخراج

**قائمة تحقق (لجميع الأنظمة):**
| الإعداد | ماذا تفعل |
|---|---|
| مثبت Ollama | كل شيء افتراضي |
| إصدار Node.js | LTS فقط |
| سؤال telemetry في n8n | `n` (اختياري) |
| HTTP Request → Method | من GET إلى POST |
| HTTP Request → Send Body | فعّله |
