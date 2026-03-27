# `gspread`

## Google Sheets API - Setup

### 1. Install the required libraries

```bash
pip install gspread google-auth
```

### 2. Authenticate with your service account and open the Google Sheet

```python
import gspread
from google.oauth2.service_account import Credentials

# Define the scope
scope = ["https://spreadsheets.google.com/feeds", "https://www.googleapis.com/auth/drive"]

# Path to your service account JSON key
json_keyfile_path = "/Users/your_name/your_repo_name/you_auth_folder/your-service-account.json"

# Authenticate using service account credentials
credentials = Credentials.from_service_account_file(json_keyfile_path, scopes=scope)
client = gspread.authorize(credentials)

# Open the Google Spreadsheet by key
spreadsheet = client.open_by_key("your_spreadsheet_key")

# Select the first worksheet by its ID
worksheet = spreadsheet.get_worksheet_by_id(0)
```
