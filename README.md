# Ticket System Simple Automation (n8n)

## 📌 Overview
This workflow automates IT ticket handling using **Telegram**, **AI classification (Ollama)**, **Google Sheets**, and **Gmail**.  
It receives IT support requests via Telegram, classifies them into problem types, routes them to the correct IT helper, logs them in Google Sheets, and notifies the responsible team via email.

---

## ⚙️ Workflow Steps

1. **Telegram Trigger**  
   - Captures incoming messages from users.  
   - Each message is passed into the workflow for classification.

2. **Message a Model (Ollama)**  
   - Uses the `llama3.2:latest` model to classify the problem.  
   - Categories include:  
     - Hardware problem  
     - Network problem  
     - Software problem  
     - Account/Access problem  
     - Peripheral problem  
     - Email/Communication problem  
     - Security problem  
     - Other problem  
   - Returns **only the category name**.

3. **Code in JavaScript**  
   - Maps the problem type to the correct IT helper email.  
   - Formats the current date/time in **dd/mm/yyyy hh:mm (GMT+2 Cairo)**.  
   - Adds a default status of `"pending"`.  
   - Returns a JSON array with:  
     ```json
     [
       {
         "problemType": "Network problem",
         "assignedHelper": "network_team@company.com",
         "ticketMessage": "User message here",
         "date": "29/03/2026 06:00",
         "status": "pending"
       }
     ]
     ```

4. **Append Row in Google Sheets**  
   - Logs the ticket into the **Tickets System** spreadsheet.  
   - Columns:  
     - Message  
     - Problem Type  
     - IT Desk  
     - Date Submitted  
     - Status  

5. **Send Message and Wait for Response (Gmail)**  
   - Sends the ticket details to the assigned IT helper.  
   - Includes a custom form for scheduling resolution (date + time options).  
   - Example email body:  
     ```
     Message : User message here
     Date Submitted : 29/03/2026 06:00
     Status : Pending
     ```

6. **Send a Text Message (Telegram)**  
   - Sends confirmation back to the user with the scheduled **date and time** chosen by IT helper.

---

## 📊 Data Flow
- **Input:** Telegram message from user.  
- **Processing:** AI classification → JS mapping → JSON formatting.  
- **Output:**  
  - Logged in Google Sheets.  
  - Sent to IT helper via Gmail.  
  - Confirmation sent back to user via Telegram.

---

## ✅ Current Features
- Automatic classification of IT problems.  
- Routing tickets to correct IT helper.  
- Logging tickets in Google Sheets.  
- Email notification with scheduling form.  
- Telegram confirmation to user.  

---

## 🚀 Next Steps (Future Enhancements)
- Add **status updates** (e.g., resolved, in-progress).  
- Track **Date ended** when ticket is closed.  
- Enable **batch processing** for multiple tickets and tracking using calender. 
