# MedAssist AI

**Make a lab report easier to discuss with your clinician.**

MedAssist Link : https://health-assist-c18ep49dm-hitanshu4.vercel.app/
Walkthrough : https://drive.google.com/file/d/1I9d3sNSY9S6BOAIA7ma6ao5vKq7O-MBf/view?usp=sharing 

MedAssist AI is a small Flask web application for exploring laboratory reports. It can organize report results into a readable dashboard, explain findings in plain language, and let you ask follow-up questions about a report.

> **For information only.** AI-generated explanations can be inaccurate and are not a diagnosis or a substitute for care from a qualified healthcare professional. Discuss results and treatment decisions with your clinician.

## What you can do

- Try one of three built-in sample reports: CBC, lipid panel, or thyroid and liver.
- Upload a PDF or an image (JPG, JPEG, PNG, WEBP, or BMP; up to 16 MB).
- Review a generated summary, risk indicator, structured test values, and recommendations.
- Ask follow-up questions about the analyzed report in the chat panel.
- Choose English, Urdu, or Roman Urdu for AI responses.
- Print the report view or save it as a PDF using your browser's print dialog.
- Switch between light and dark themes.

Report analysis and chat use the Google Gemini API. The dashboard and sample reports can load without an API key, but AI analysis and chat need one.

## Run it locally

### Requirements

- Python 3.10 or newer
- A Google Gemini API key for AI-powered analysis and chat

### 1. Get the project

```bash
git clone https://github.com/hitanshus-svg/Health-Assist.git
cd Health-Assist
```

### 2. Create and activate a virtual environment

**Windows PowerShell**

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**macOS / Linux**

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Python packages

```bash
python -m pip install -r requirements.txt
```

### 4. Configure Gemini

Copy the example environment file and add your key:

```powershell
Copy-Item .env.example .env
```

Open `.env` and set `GEMINI_API_KEY` to your key from [Google AI Studio](https://aistudio.google.com/app/apikey):

```dotenv
GEMINI_API_KEY=your_key_here
```

The application also accepts `GOOGLE_API_KEY`. Keep real keys in `.env` or your hosting provider's environment settings; never commit them to Git.

### 5. Start the server

```bash
python app.py
```

Open [http://127.0.0.1:5000](http://127.0.0.1:5000). The local server uses Flask's development mode; use a production WSGI server when hosting outside local development.

## Using the app

1. Choose a sample case or select a report file.
2. Select the response language.
3. Start the analysis and review the summary and individual test results.
4. Open the report chat to ask follow-up questions, or print the report view.

The sample data is illustrative. It is not real patient data and should not be used to make medical decisions.

## HTTP endpoints

| Method | Path | Purpose |
| --- | --- | --- |
| `GET` | `/` | Load the web application. |
| `GET` | `/api/health` | Check service status and whether an API key is configured. |
| `GET` | `/api/sample/<sample_id>` | Retrieve a sample report. IDs: `cbc`, `lipid`, `liver_thyroid`. |
| `POST` | `/api/analyze` | Analyze JSON report text or an uploaded PDF/image. |
| `POST` | `/api/chat` | Ask a question using report context and chat history. |

For text analysis, send JSON to `/api/analyze`:

```json
{
  "text": "Paste report text here",
  "language": "en"
}
```

For file analysis, send `multipart/form-data` with a `file` field and an optional `language` field. Supported language codes are `en`, `ur`, and `roman_ur`.

For chat, send JSON to `/api/chat` with `message`, `report_context`, `chat_history`, and `language`. The frontend constructs this context from the current report.

## Deploy on Vercel

The repository includes a Vercel Python entry point (`api/index.py`) and routing configuration (`vercel.json`).

1. Import this GitHub repository in the [Vercel dashboard](https://vercel.com/dashboard).
2. Add `GEMINI_API_KEY` (or `GOOGLE_API_KEY`) under the project's environment variables.
3. Deploy the project, then check `/api/health` on the deployment to confirm the service is responding and the key is configured.

Do not place API keys in frontend code, commit them, or expose them in public logs.

## Project layout

```text
.
├── api/
│   └── index.py                 # Vercel WSGI entry point
├── public/static/               # Static assets for deployment
├── static/
│   ├── css/                     # Local stylesheets
│   ├── js/app.js                # Browser interactions and API calls
│   └── medassist-icon.svg       # Application icon
├── templates/
│   └── index.html               # Main application page
├── app.py                       # Flask routes and Gemini integration
├── requirements.txt             # Python dependencies
├── vercel.json                  # Vercel build and routing configuration
└── .env.example                 # Environment variable template
```

## Data and privacy

The application does not include a user account or database layer. When you request AI analysis or chat, report content and the supplied chat context are sent to the configured Gemini API. Review Google's applicable terms and data policies before using the service with sensitive information. Avoid uploading information you are not authorized to share.

## License

This project is distributed under the MIT License. See [LICENSE](LICENSE).
