AGRI-COOPERATIVE RESOURCE PLANNING (ACRP) SYSTEM

Developer: Group 9 - HUMORLESS

University: Davao Oriental State University (DOrSU)

Course/Year: BSIT - 1st Year

Project Theme: Project ACRP (Agri-Resource Consistency & Record Preservation)
==========================================================================

1. PROJECT OVERVIEW
Project ACRP is a specialized resource planning tool designed for 
agricultural cooperatives in Davao Oriental. Its primary goal is to 
ensure resource consistency by enforcing a strict distribution limit 
(3 sacks per hectare) and maintaining digital record preservation 
for auditing and emergency support.

2. SYSTEM FEATURES
- Security: Caesar Cipher encryption (+3 Shift) for secure password storage.
- Validation: Advanced Regex patterns ensuring valid Numeric IDs and PH 
  standard mobile formats (09XXXXXXXXX).
- Role-Based Access Control (RBAC): 
    - ADMINISTRATOR: Full CRUD (Create, Read, Update, Delete) capabilities.
    - STAFF: View-only access, Emergency Priority List, and Audit Log viewing.
- Data Persistence: Real-time Auto-save feature synchronized with data.txt.

3. HOW TO RUN
- Ensure Java Development Kit (JDK) is installed on your machine.
- Place ACRPSystem.java, users.txt, and data.txt in the same directory.
- Open your Terminal or Command Prompt.
- Compile the code: javac ACRPSystem.java
- Execute the program: java ACRPSystem

4. TEST CREDENTIALS
For testing purposes, use the following pre-registered accounts:

ADMIN ACCOUNTS (Role ID: 0):
- Username: admin1 (up to admin5)
- Password: admin123

STAFF ACCOUNTS (Role ID: 1):
- Username: staff1 (up to staff10)
- Password: staff123

//For security, the 'users.txt' file stores passwords in an 
encrypted format (e.g., 'admin123' is saved as 'dgplq456').
==========================================================================
