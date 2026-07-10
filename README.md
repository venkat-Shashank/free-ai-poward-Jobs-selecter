# 🤖 Job Scraper Bot

> An automated AI-assisted job search bot I built for my own fresher/entry-level job hunt. It searches job boards, removes duplicates, reads job descriptions, filters only relevant Python Developer opportunities, writes the results to Google Sheets, and sends Telegram alerts with match scores and optional AI-drafted cover letters.

---

## 💡 Why I Built This

I am a developer with hands-on skills in **Python, SQL, REST APIs, Git/GitHub, SQLite/MySQL, Flask, debugging, and basic backend development exposure** from my internship.

During my job search, I noticed one repeated problem: most job portals show a lot of irrelevant roles such as senior roles, manager roles, repeated listings, expired links, or jobs that do not match my profile. I wanted one simple automation that could run every hour and show only the jobs that are actually useful for me.

This project started as a small script, but I slowly improved it after facing real issues like:

- search APIs giving duplicate or irrelevant links
- DuckDuckGo sometimes hanging
- RSS feeds taking too long to respond
- AI models returning broken JSON instead of clean output
- GitHub Actions running for too long if a request got stuck
- Google Sheets service-account permission errors
- Telegram buttons needing a separate callback handler
- keeping all API keys safe using GitHub Secrets instead of hardcoding them

So this repository is not just a scraper. It is a complete automation workflow using Python, APIs, AI filtering, Google Sheets, Telegram, and GitHub Actions.

---

## ⚙️ What This Bot Does

The bot automatically:

1. Searches job listings from multiple sources using:
   - Google Custom Search API
   - DuckDuckGo Search through `ddgs`
   - Jobicy RSS feed for remote security jobs
2. Collects job links from platforms like:
   - Instahyre
   - CutShort
   - Hirist
   - Naukri
   - Foundit
   - LinkedIn Jobs
   - Wellfound
   - Hiring Cafe
   - We Work Remotely
   - Greenhouse
   - Lever
   - Jobicy
3. Scrapes job-page text using `aiohttp` and `BeautifulSoup`.
4. Rejects irrelevant jobs such as:
   - senior roles
   - manager/head/lead roles
   - non-python roles
   - generic search-result pages
5. Uses Gemini or Groq AI to score each job.
6. Saves matched jobs into Google Sheets.
7. Sends Telegram alerts with:
   - job title
   - job link
   - match percentage
   - suitability percentage
8. Drafts a cover letter automatically when the match score is high enough.
9. Runs automatically every hour using GitHub Actions.

---

## 🎯 Resume-Based Target Profile

This bot is customized for my current resume and early-career profile.

### 💪 My Current Strengths

- Python programming
- Object-Oriented Programming
- REST API integration
- SQL, MySQL, SQLite
- Flask basics
- Git and GitHub
- Postman API testing
- Debugging and unit testing basics
- Data Structures and Algorithms
- Cybersecurity internship experience
- Basic computer networking knowledge
- Agile/team collaboration experience

### 🔍 Roles This Bot Is Mainly Designed To Find

- Python Developer Intern
- Backend Developer Intern
- Software Engineer Intern
- Python Automation Intern
- Junior Python Developer
- Entry-Level Software Engineer

### 🚫 Roles This Bot Intentionally Rejects

- Senior Software Engineer
- Engineering Manager
- Head of Engineering
- Lead Developer
- Architect-level roles
- Pure sales/marketing roles
- Non-technical roles
- Jobs unrelated to Python development, backend engineering, or software automation

---

## 🛠️ Tech Stack

| Area | Tools Used |
|---|---|
| Language | Python 3.10 |
| Async HTTP | `aiohttp`, `asyncio` |
| Web parsing | `BeautifulSoup`, `feedparser` |
| Search | Google Custom Search API, DuckDuckGo Search, Jobicy RSS |
| AI filtering | Gemini API, Groq API |
| Storage | Google Sheets |
| Notifications | Telegram Bot API |
| Automation | GitHub Actions cron workflow |
| Auth | Google Service Account |
| Optional callbacks | Google Apps Script |

---

## 📁 Project Structure

```text
job-scraper-bot/
├── main.py
├── README.md
├── requirements.txt
└── .github/
    └── workflows/
        └── job-scraper.yml
```

---

## ⚠️ Important Note Before Copying The Code

If you copied `main.py` from a chat, make sure the code is clean Python.

Sometimes Markdown converts code into broken text like:

```python
[asyncio.run](http://asyncio.run)(main())
[re.search](http://re.search)(...)
if **name** == "__main__":
```

It must be changed back to valid Python:

```python
asyncio.run(main())
re.search(...)
if __name__ == "__main__":
```

Also replace HTML entities if they appear:

```text
&lt;  ->  <
&gt;  ->  >
&amp; ->  &
```

---

## 🔑 Required Access / APIs

You need the following accounts and API access to run the complete project.

| Requirement | Needed? | Purpose |
|---|---:|---|
| GitHub account | Required | To host the repo and run GitHub Actions |
| Google Cloud project | Required | To create service account and enable APIs |
| Google Sheets API | Required | To write job results into Google Sheets |
| Google Drive API | Required | To allow service account to open the sheet |
| Google service account JSON | Required | Used as `GOOGLE_SHEET_CREDS` secret |
| Google Programmable Search Engine | Recommended | Gives `CXAPI` search engine ID |
| Google Custom Search API key | Recommended | Used as `SAPI` |
| Gemini API key | Required or Groq fallback | AI job filtering and cover-letter drafting |
| Groq API key | Optional fallback | Backup if Gemini fails or quota is over |
| Telegram bot token | Required for alerts | Sends job notifications |
| Telegram chat ID | Required for alerts | Tells the bot where to send messages |
| Google Apps Script | Optional | Needed only if you want Apply/Trash buttons to update Google Sheet |

The bot can still collect some results using DuckDuckGo and Jobicy without Google Search API, but the complete setup works best when all required secrets are configured.

---

## 📊 Google Sheet Setup

Create a Google Sheet with this exact name unless you modify the code:

```text
Job_Search_Master
```

> Important: The current code defines `SHEET_NAME`, but it opens the sheet using `client.open("Job_Search_Master")`. So either create the sheet with this exact name or update the code to use `client.open(SHEET_NAME)`.

Create these columns in the first row:

| Column | Header | Used For |
|---|---|---|
| A | Status | New / Applied / Trash |
| B | Source | AI Match |
| C | Job Title | Job title from search result |
| D | Company | Currently saved as `Unknown` |
| E | Link | Job URL, used for duplicate checking |
| F | Found At | Timestamp |
| G | Match % | AI skill-match score |
| H | Suitability % | AI fresher/intern suitability score |
| I | AI Cover Letter | Auto-drafted email for strong matches |

The script checks duplicates using **Column E**, so do not move the Link column unless you also update this line in `main.py`:

```python
existing_links = set(sheet.col_values(5)[1:])
```

---

## 🔐 Google Service Account Setup

1. Go to Google Cloud Console.
2. Create a new project or use an existing project.
3. Enable these APIs:
   - Google Sheets API
   - Google Drive API
4. Create a Service Account.
5. Create and download a JSON key for that service account.
6. Open your Google Sheet.
7. Share the sheet with the service account email.
   - The email usually looks like:

```text
something@project-name.iam.gserviceaccount.com
```

8. Give it **Editor** access.
9. Add the full JSON content as a GitHub secret named:

```text
GOOGLE_SHEET_CREDS
```

Recommended way to make the JSON one line before adding it to GitHub Secrets:

```bash
cat service-account.json | jq -c .
```

Then copy the output and paste it into the GitHub secret.

Never commit the service-account JSON file to GitHub.

---

## 🔎 Google Custom Search Setup

This bot uses Google Custom Search for more stable search results.

You need two values:

```text
SAPI   = Google Custom Search API key
CXAPI  = Programmable Search Engine ID
```

Steps:

1. Create or open a Google Cloud project.
2. Enable **Custom Search JSON API**.
3. Create an API key and save it as `SAPI`.
4. Create a Programmable Search Engine.
5. Configure it to search the whole web or your selected job sites.
6. Copy the Search Engine ID and save it as `CXAPI`.

The code searches with:

```python
params = {
    "key": cred["key"],
    "cx": cred["cx"],
    "q": query,
    "start": start_page,
    "dateRestrict": "m1"
}
```

`dateRestrict: "m1"` keeps results focused on roughly the last month.

---

## 🧠 Gemini API Setup

Gemini is used as the main AI filter.

The code supports up to 6 Gemini keys:

```text
GAPI1
GAPI2
GAPI3
GAPI4
GAPI5
GAPI6
```

You need at least one Gemini key if you want Gemini filtering.

The script randomly chooses one available Gemini key:

```python
GEMINI_KEYS = [os.getenv(f"GAPI{i}") for i in range(1, 7) if os.getenv(f"GAPI{i}")]
```

Why multiple keys?

- Helps avoid one key hitting quota too quickly.
- Gives backup if one key fails.
- Useful when the workflow runs many times per day.

---

## ⚡ Groq API Setup

Groq is used as a fallback AI provider when Gemini does not return a result.

The code supports up to 6 Groq keys:

```text
GRAPI1
GRAPI2
GRAPI3
GRAPI4
GRAPI5
GRAPI6
```

Groq uses this model in the current code:

```python
"model": "llama3-8b-8192"
```

You can replace it with another supported Groq model if needed.

---

## 📱 Telegram Bot Setup

Telegram is used to receive alerts instantly.

### 1. Create a Bot

1. Open Telegram.
2. Search for `@BotFather`.
3. Run:

```text
/newbot
```

4. Give your bot a name and username.
5. Copy the token.
6. Save it as this GitHub secret:

```text
TOK
```

### 2. Get Your Chat ID

Send one message to your bot first, then open this URL in a browser:

```text
https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates
```

Find your chat ID in the JSON response.

Save it as this GitHub secret:

```text
ID
```

The bot sends messages like this:

```text
🤖 JOB ALERT

💼 Job Title
🎯 Match: 90%
📈 Suitability: 88%

🔗 Job Link
```

---

## 🤫 GitHub Secrets Required

Go to:

```text
GitHub repo -> Settings -> Secrets and variables -> Actions -> New repository secret
```

Add these secrets:

| Secret Name | Required? | Description |
|---|---:|---|
| `GOOGLE_SHEET_CREDS` | Required | Full Google service-account JSON |
| `SAPI` | Recommended | Google Custom Search API key |
| `CXAPI` | Recommended | Google Programmable Search Engine ID |
| `GAPI1` | Required if using Gemini | Gemini API key |
| `GAPI2` to `GAPI6` | Optional | Extra Gemini keys for fallback/quota rotation |
| `GRAPI1` | Optional but useful | Groq API key |
| `GRAPI2` to `GRAPI6` | Optional | Extra Groq keys |
| `TOK` | Required for Telegram | Telegram bot token |
| `ID` | Required for Telegram | Telegram chat ID |
| `USER_PROFILE_INFO` | Required for good AI matching | Resume/profile text used by AI |
| `SHEET_NAME` | Optional | Currently not fully used unless code is updated |

---

## 📝 Recommended `USER_PROFILE_INFO` For Your Resume

Add this as the `USER_PROFILE_INFO` GitHub secret. This is what helps the AI understand my profile and select suitable jobs.

```text
[Your Name] is a software engineering student targeting entry-level software engineering and python developer roles. [He/She/They] has strong foundations in Python, Java, SQL, Object-Oriented Programming, REST API integration, file handling, modules, debugging, unit testing basics, Data Structures and Algorithms, and computer networking basics.

Technical skills include Python, Java, SQL, MySQL, SQLite, Flask basics, Git, GitHub, VS Code, PyCharm, Postman, and Jupyter Notebook. He has built projects such as an Expense Tracker Application using Python, SQLite and Tkinter, a URL Shortener REST API using Python, Flask and SQLite, and a Student Result Management System using Python and MySQL.

Completed a Cybersecurity Internship at [Company Name] from [Date] to [Date], working in an Agile environment, assisting in debugging software issues, documenting findings, working with SQL databases, and improving analytical problem-solving skills.

Preferred roles: Python Developer Intern, Backend Developer Intern, Software Engineer Intern, Junior Python Developer, and entry-level roles where Python, SQL, APIs, networking basics and backend engineering interest are useful.

Avoid senior, manager, lead, head, architect, sales, marketing, and non-technical roles. Prefer fresher, internship, trainee, junior, graduate, associate, and entry-level openings in India, Bengaluru, remote, or hybrid locations.
```

You can update this text whenever your resume improves.

---

## 📦 `requirements.txt`

Create a `requirements.txt` file:

```txt
aiohttp
gspread
feedparser
google-auth
google-auth-oauthlib
google-auth-httplib2
google-api-python-client
requests
beautifulsoup4
ddgs
```

Install locally with:

```bash
pip install -r requirements.txt
```

---

## 🔄 GitHub Actions Workflow

Create this file:

```text
.github/workflows/job-scraper.yml
```

Workflow:

```yaml
name: Job Scraper Bot

on:
  schedule:
    - cron: '0 * * * *'
  workflow_dispatch:

jobs:
  run-scraper:
    runs-on: ubuntu-latest
    timeout-minutes: 15

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.10'

      - name: Install Dependencies
        run: |
          python -m pip install --upgrade pip
          pip install aiohttp gspread feedparser google-auth google-auth-oauthlib google-auth-httplib2 google-api-python-client requests beautifulsoup4 ddgs

      - name: Run Job Scraper
        env:
          GOOGLE_SHEET_CREDS: ${{ secrets.GOOGLE_SHEET_CREDS }}
          SAPI: ${{ secrets.SAPI }}
          CXAPI: ${{ secrets.CXAPI }}
          GAPI1: ${{ secrets.GAPI1 }}
          GAPI2: ${{ secrets.GAPI2 }}
          GAPI3: ${{ secrets.GAPI3 }}
          GAPI4: ${{ secrets.GAPI4 }}
          GAPI5: ${{ secrets.GAPI5 }}
          GAPI6: ${{ secrets.GAPI6 }}
          GRAPI1: ${{ secrets.GRAPI1 }}
          GRAPI2: ${{ secrets.GRAPI2 }}
          GRAPI3: ${{ secrets.GRAPI3 }}
          GRAPI4: ${{ secrets.GRAPI4 }}
          GRAPI5: ${{ secrets.GRAPI5 }}
          GRAPI6: ${{ secrets.GRAPI6 }}
          TOK: ${{ secrets.TOK }}
          ID: ${{ secrets.ID }}
          USER_PROFILE_INFO: ${{ secrets.USER_PROFILE_INFO }}
          SHEET_NAME: ${{ secrets.SHEET_NAME }}
        run: python main.py
```

The workflow runs:

- automatically every hour
- manually using **Run workflow** from the GitHub Actions tab

The line below is very important:

```yaml
timeout-minutes: 15
```

I added it because scraping/search requests can sometimes hang. Without a timeout, the workflow can waste a lot of GitHub Actions time.

---

## 💻 Running Locally

### 1. Clone the Repo

```bash
git clone https://github.com/your-username/job-scraper-bot.git
cd job-scraper-bot
```

### 2. Create Virtual Environment

```bash
python -m venv .venv
source .venv/bin/activate
```

For Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Set Environment Variables

Linux/macOS example:

```bash
export GOOGLE_SHEET_CREDS='{"type":"service_account","project_id":"..."}'
export SAPI='your_google_custom_search_api_key'
export CXAPI='your_search_engine_id'
export GAPI1='your_gemini_api_key'
export GRAPI1='your_groq_api_key'
export TOK='your_telegram_bot_token'
export ID='your_telegram_chat_id'
export USER_PROFILE_INFO='your_resume_profile_text'
```

Windows PowerShell example:

```powershell
$env:GOOGLE_SHEET_CREDS='{"type":"service_account","project_id":"..."}'
$env:SAPI='your_google_custom_search_api_key'
$env:CXAPI='your_search_engine_id'
$env:GAPI1='your_gemini_api_key'
$env:GRAPI1='your_groq_api_key'
$env:TOK='your_telegram_bot_token'
$env:ID='your_telegram_chat_id'
$env:USER_PROFILE_INFO='your_resume_profile_text'
```

### 5. Run

```bash
python main.py
```

---

## 🤖 How The AI Filtering Works

The AI receives my resume/profile and a batch of job descriptions.

It returns JSON in this format:

```json
{
  "matches": [
    {
      "index": 0,
      "match_percent": 85,
      "suitability": 90
    }
  ]
}
```

### Meaning Of Scores

| Score | Meaning |
|---|---|
| `match_percent` | How well the job skills match my resume |
| `suitability` | How suitable the role is for a fresher/intern profile |

The bot drafts a cover letter only when:

```python
match_percent >= 85 and suitability >= 85
```

This avoids wasting AI calls on weak matches.

---

## 🔍 Search Queries Used

The current bot searches for python-developer-related fresher roles in India/Bengaluru/remote locations.

Example queries:

```python
QUERIES = [
    'site:instahyre.com ("Python Developer" OR "Python Backend" OR "Django") (Bangalore OR Remote)',
    'site:cutshort.io ("Python Developer" OR "Python Engineer") India',
    'site:hirist.tech ("Python Developer" OR "Python") India',
    'site:naukri.com ("Python Developer" OR "Python Engineer") Bangalore not:senior',
    'site:foundit.in ("Python Developer" OR "Python Engineer") Bangalore',
    'site:linkedin.com/jobs/view ("Python Developer" OR "Python Engineer") Bangalore',
    'site:wellfound.com/jobs ("Python Developer") India',
    'site:hiring.cafe ("Python" OR "Django") "India"',
    'site:weworkremotely.com ("Python") not:senior',
    'site:greenhouse.io/embed/job ("Python Developer") India',
    'site:jobs.lever.co ("Python") India'
]
```

To make it more suitable for generic software engineer roles, you can add queries like:

```python
'site:linkedin.com/jobs/view ("Software Engineer Intern") Bangalore',
'site:cutshort.io ("Backend Developer") Fresher India',
'site:wellfound.com/jobs ("Backend Intern") India'
```

---

## 🪝 Optional: Google Apps Script For Telegram Apply/Trash Buttons

The Python script sends Telegram inline buttons:

```text
✅ Apply
❌ Trash
```

But Telegram buttons need a callback handler. The Python script sends the buttons, but it does not listen for button clicks. If you want the buttons to update Google Sheets automatically, use Google Apps Script as a small webhook.

### What This GAS Webhook Does

When you click:

- `✅ Apply` -> Column A becomes `Applied`
- `❌ Trash` -> Column A becomes `Trash`

### Apps Script Setup

1. Open your Google Sheet.
2. Go to **Extensions -> Apps Script**.
3. Paste this code into `Code.gs`.
4. Add Script Properties:
   - `TELEGRAM_TOKEN`
   - `TELEGRAM_CHAT_ID`
   - `SHEET_ID`
   - `WEB_APP_URL` after deployment
5. Deploy as Web App:
   - Execute as: **Me**
   - Who has access: **Anyone**
6. Copy the Web App URL.
7. Add it as `WEB_APP_URL` in Script Properties.
8. Run `setWebhook()` once from Apps Script.

### Google Apps Script Code

```javascript
function doGet() {
  return ContentService.createTextOutput('Job Scraper Bot callback webhook is live.');
}

function doPost(e) {
  const props = PropertiesService.getScriptProperties();
  const token = props.getProperty('TELEGRAM_TOKEN');
  const allowedChatId = props.getProperty('TELEGRAM_CHAT_ID');
  const sheetId = props.getProperty('SHEET_ID');

  try {
    const update = JSON.parse(e.postData.contents);

    if (!update.callback_query) {
      return ContentService.createTextOutput('ok');
    }

    const callback = update.callback_query;
    const data = callback.data || '';
    const chatId = String(callback.message.chat.id);

    if (allowedChatId && chatId !== String(allowedChatId)) {
      answerCallback(token, callback.id, 'Unauthorized chat');
      return ContentService.createTextOutput('ok');
    }

    const parts = data.split('_');
    const action = parts[0];
    const rowNumber = Number(parts[1]);

    if (!rowNumber || !['apply', 'trash'].includes(action)) {
      answerCallback(token, callback.id, 'Invalid action');
      return ContentService.createTextOutput('ok');
    }

    const status = action === 'apply' ? 'Applied' : 'Trash';
    const spreadsheet = SpreadsheetApp.openById(sheetId);
    const sheet = spreadsheet.getSheets()[0];

    sheet.getRange(rowNumber, 1).setValue(status);

    answerCallback(token, callback.id, `Marked as ${status}`);
    return ContentService.createTextOutput('ok');

  } catch (err) {
    return ContentService.createTextOutput('error: ' + err.message);
  }
}

function answerCallback(token, callbackId, message) {
  const url = `https://api.telegram.org/bot${token}/answerCallbackQuery`;

  UrlFetchApp.fetch(url, {
    method: 'post',
    contentType: 'application/json',
    payload: JSON.stringify({
      callback_query_id: callbackId,
      text: message
    })
  });
}

function setWebhook() {
  const props = PropertiesService.getScriptProperties();
  const token = props.getProperty('TELEGRAM_TOKEN');
  const webAppUrl = props.getProperty('WEB_APP_URL');

  const url = `https://api.telegram.org/bot${token}/setWebhook?url=${encodeURIComponent(webAppUrl)}`;
  const response = UrlFetchApp.fetch(url);
  Logger.log(response.getContentText());
}
```

### Important Button Accuracy Fix

If multiple jobs are found in one run, make sure each Telegram button points to the correct future row.

A safer pattern inside `main.py` is:

```python
row_number = next_row

kb = {
    "inline_keyboard": [[
        {"text": "✅ Apply", "callback_data": f"apply_{row_number}"},
        {"text": "❌ Trash", "callback_data": f"trash_{row_number}"}
    ]]
}

next_row += 1
```

Without incrementing `next_row`, multiple messages may point to the same sheet row.

---

## 🛣️ Main Workflow Explanation

### 1. Search Phase

The bot runs Google and DuckDuckGo searches for every query.

```python
tasks = [search_google(session, q, p) for q in QUERIES for p in [1, 11]] + [search_ddg(q) for q in QUERIES]
results = await asyncio.gather(*tasks)
```

Google checks page starts `1` and `11`, while DuckDuckGo collects additional results.

### 2. Duplicate Removal

The bot stores already-seen links from Google Sheets:

```python
existing_links = set(sheet.col_values(5)[1:])
```

This prevents sending the same job repeatedly.

### 3. Deep Scraping

The bot opens each job link and extracts readable text:

```python
soup = BeautifulSoup(html, 'html.parser')
text = soup.get_text(separator=' ', strip=True)
```

### 4. AI Filtering

Jobs are analyzed in batches of 5:

```python
for i in range(0, len(jobs_buffer), 5):
    chunk = jobs_buffer[i:i+5]
```

Batching saves API calls and keeps prompts smaller.

### 5. Save And Notify

Matched jobs are saved to Google Sheets and sent to Telegram.

---

## 🐛 Common Problems And Fixes

### 1. Google Sheet Not Opening

Error example:

```text
Error Opening Sheet
```

Fix:

- Make sure the sheet name is exactly `Job_Search_Master`.
- Share the sheet with the service-account email.
- Enable Google Sheets API and Google Drive API.

---

### 2. `GOOGLE_SHEET_CREDS` JSON Error

Fix:

- Store the complete service-account JSON as one GitHub secret.
- Do not remove quotes or braces.
- Use compact JSON if possible:

```bash
cat service-account.json | jq -c .
```

---

### 3. No Jobs Found

Possible reasons:

- API quota finished.
- Search queries are too strict.
- Job boards blocked scraping.
- AI rejected most roles as senior/non-cyber.
- Resume profile text is too weak or too generic.

Fix:

- Add more search queries.
- Improve `USER_PROFILE_INFO`.
- Check GitHub Actions logs.
- Try running manually.

---

### 4. Telegram Message Not Coming

Fix:

- Check `TOK` secret.
- Check `ID` secret.
- Send one message to the bot first.
- Test this URL:

```text
https://api.telegram.org/bot<YOUR_BOT_TOKEN>/sendMessage?chat_id=<YOUR_CHAT_ID>&text=test
```

---

### 5. AI Returns Invalid JSON

This is why the code has `safe_parse_json()`.

Sometimes AI models wrap JSON inside text. The helper tries to extract JSON using regex.

---

### 6. GitHub Action Keeps Running Too Long

Fix:

```yaml
timeout-minutes: 15
```

The code also uses request timeouts:

```python
timeout = aiohttp.ClientTimeout(total=90)
```

DuckDuckGo is also wrapped with:

```python
asyncio.wait_for(..., timeout=20.0)
```

---

### 7. Telegram Apply/Trash Buttons Do Nothing

That is expected unless you set up the Google Apps Script webhook.

The Python script only sends the buttons. Google Apps Script or another server must handle the callback.

---

## 🔒 Security Notes

- Never commit API keys.
- Never commit Google service-account JSON.
- Use GitHub Secrets for all credentials.
- Keep the Google Sheet shared only with trusted accounts.
- Rotate API keys if they are accidentally exposed.
- Avoid scraping private pages or pages behind login.
- Respect job-site terms and keep request volume low.

---

## 🚧 Limitations

This project is useful, but it is not perfect.

- Some job sites block scraping.
- Some search results are not direct job pages.
- AI scoring is helpful but not always 100% accurate.
- Company name extraction is currently basic and saved as `Unknown`.
- Telegram buttons need Google Apps Script or another webhook to work.
- Free API quotas can run out.
- The bot depends on job-page text being accessible without login.

---

## 🚀 Future Improvements

Things I want to improve later:

- Extract company name automatically.
- Add location and salary columns.
- Add better duplicate detection using normalized URLs.
- Add direct support for software engineer fresher roles.
- Save rejected jobs separately for analysis.
- Add a dashboard using Streamlit or Flask.
- Add email alerts in addition to Telegram.
- Improve cover letters with company-specific context.
- Add tests for JSON parsing and link cleaning.
- Add better callback handling directly in Python.

---

## 🎓 What I Learned

This project helped me practice real-world software engineering skills:

- async Python programming
- API integration
- Google Sheets automation
- Telegram bot integration
- AI prompt engineering
- JSON parsing and error handling
- GitHub Actions automation
- secret management
- debugging timeouts
- building something useful for my own career

It also connects directly with my resume because it uses Python, APIs, SQL-related storage thinking, debugging, automation, and python developer job filtering.

---

## ⚖️ Disclaimer

This project is for personal job-search automation and learning. It should be used responsibly with low request volume. It does not guarantee job selection, interviews, or perfect job matching. Final verification and application should always be done manually.

---

## 👨‍💻 Author

**[G venkata shashank]**  
[Python developer]  
Interested in Python, Software Engineering, Backend Development, Django, APIs, and automation.

GitHub: [[venkat-Shashank]](https://github.com/venkat-Shashank)
