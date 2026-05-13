AGRI-COOPERATIVE RESOURCE PLANNING (ACRP) SYSTEM

Developer: Group 9 - HUMORLESS

University: Davao Oriental State University (DOrSU)

Course/Year: BSIT - 1st Year
==========================================================================

1. PROJECT OVERVIEW

Project ACRP is a specialized resource planning tool for agricultural 
cooperatives. It enforces a strict fairness limit (3 sacks per hectare) 
and provides a critical 'Emergency Priority List' for low-stock farmers.

2. NEW FEATURES ADDED

- Update Hectares: Administrators can now modify the farm size of existing 
  farmer records without deleting them.
- Persistent Login Loop: Users can now attempt a retry or exit the system 
  directly from the login page without the program crashing.
- Navigation: Added a '[B] Back' option in CRUD operations to prevent 
  the user from getting stuck in a menu.

3. SYSTEM HIGHLIGHTS

- Security: Caesar Cipher (+3 Shift) encryption for stored passwords.
- Privacy: Silent console masking with asterisk feedback reveal.
- Validation: Regex verification for numeric IDs and PH Mobile Numbers.
- Role-Based Access: Specific menus for Admin and Staff roles.

4. TEST CREDENTIALS

ADMIN: admin1 / HUmorLESS@1234

STAFF: staff1 / HUMORless@4312

5. HOW TO RUN
1. Compile: javac ACRPSystem.java
2. Run: java ACRPSystem (Must run in Command Prompt for password masking)
