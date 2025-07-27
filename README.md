# automation-login-bot
# 🕸️ Selenium Automation: Candidate Portal Login Bot

This is a Python automation project built using Selenium. The script automates the login process to the [PCET Nagpur Candidate Portal](https://mis.pcenagpur.edu.in/modules/Candidate/candidateLogin.php) and navigates through specific sections like the **Personal** tab and **Next** buttons.

---

## 🚀 Features

- Automates login with username and password
- Clicks on the "Personal" tab after login
- Repeatedly clicks "Next" buttons to go through form steps
- Waits and verifies element visibility using `WebDriverWait`
- Logs clickable elements for debugging

---

## 🛠️ Tech Stack

- 🐍 Python 3
- 🧪 Selenium
- 🌐 Chrome WebDriver (via `webdriver-manager`)

---

## 📦 Installation

1. **Clone the repository**:

   ```bash
   git clone https://github.com/your-username/automation-login-bot.git
   cd automation-login-bot
   Install dependencies:

2.Make sure you have Python installed. Then install the required libraries:

bash
Copy
Edit
pip install -r requirements.txt
3.If requirements.txt is not there, you can install manually:

bash
Copy
Edit
pip install selenium webdriver-manager
4.▶️ How to Run
Just run the script using Python:

bash
Copy
Edit
python login_bot.py
