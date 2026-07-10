<h1 align="center">💼 Job Showcase Bot</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-blue?style=flat&logo=python" alt="Python">
  <img src="https://img.shields.io/badge/Automation-n8n-orange?style=flat&logo=n8n" alt="n8n">
  <img src="https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-2088FF?style=flat&logo=github-actions" alt="GitHub Actions">
  <img src="https://img.shields.io/badge/Integration-Telegram-2CA5E0?style=flat&logo=telegram" alt="Telegram">
</p>

---

### 📖 Overview

An automated **Job Showcase Bot** designed to streamline job searches by fetching, processing, and delivering relevant career opportunities. This project leverages AI-driven workflows to parse job data and broadcasts curated updates seamlessly.

### ✨ Features

* **Automated Aggregation:** Continuously monitors and fetches job postings from targeted platforms and feeds.
* **AI-Powered Processing:** Utilizes Google AI Studio to intelligently parse, summarize, and categorize job descriptions.
* **Workflow Automation:** Managed through n8n and scheduled via GitHub Actions for reliable, hands-off execution.
* **Instant Notifications:** Dispatches formatted job alerts directly to Telegram channels utilizing Google Apps Script and the Telegram API.

---

### 🛠️ Tech Stack

* **Core & AI:** Python, Google AI Studio
* **Automation & CI/CD:** n8n, GitHub Actions
* **Integration:** Google Apps Script, Telegram API
* **Environment:** Linux (WSL)

---

### 🚀 Getting Started

#### Prerequisites
* Python 3.8+
* [n8n](https://n8n.io/) installed or cloud instance access
* Telegram Bot Token and Chat ID
* Google AI Studio API Key

#### Installation

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/YOUR_USERNAME/jobs.git](https://github.com/YOUR_USERNAME/jobs.git)
   cd jobs
