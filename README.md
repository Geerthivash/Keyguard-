# KeyGuard – Python-Based Content Filtering & Access Control Tool

KeyGuard is a lightweight content-filtering application built in Python to help enforce safe browsing practices on desktop systems.  
It detects and blocks restricted or inappropriate content using rule-based pattern matching on URLs, keywords, and user inputs.

This project demonstrates practical skills in automation, access control, and Python scripting — relevant for IT support, compliance enforcement, and endpoint security operations.

---

## 🚀 Features

- **URL & Keyword Filtering:**  
  Blocks access to restricted categories based on configurable patterns.

- **Automated Content Inspection:**  
  Scans user-entered URLs or search terms to prevent exposure to inappropriate material.

- **Customizable Ruleset:**  
  Filtering patterns can be easily modified to suit school, home, or corporate environments.

- **Lightweight & Offline:**  
  No external API calls; works entirely through local pattern matching.

---

## 🛠️ Tech Stack

- **Python 3**
- Regular Expressions (Regex)
- File Handling & Pattern Lists
- Basic Access Control Logic

---

## 📦 How It Works

1. The user attempts to access a URL or search query.
2. KeyGuard checks the string against a list of blocked patterns.
3. If matched:
   - Access is denied.
   - A warning is displayed.
4. If clean:
   - The request is allowed.

---

## 📁 Project Structure

