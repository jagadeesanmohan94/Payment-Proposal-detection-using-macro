# Payment-Proposal-detection-using-macro

Macro file link 

https://capgemini-my.sharepoint.com/personal/jagadeesan_mohan_capgemini_com/Documents/Desktop/Learning!!!/CG%20Macro/Module%20for%20PP%20macro.xlsm?web=1

Output file link

https://capgemini-my.sharepoint.com/personal/jagadeesan_mohan_capgemini_com/Documents/PP%20Learnings%20(002).xlsx?web=1


![Image](https://github.com/user-attachments/assets/4034c0fe-f99b-4050-848c-d15374f20779)

![Image](https://github.com/user-attachments/assets/7f02f3cc-ab89-4927-ba0a-8c1ece3c24c2)

![Image](https://github.com/user-attachments/assets/74c4b0b8-d268-4dc5-9462-f38741483f7e)

![Image](https://github.com/user-attachments/assets/5f43db3a-7a5a-4819-83d8-25bcd20520a1)

![Image](https://github.com/user-attachments/assets/f8de00b9-46c8-417b-b4b2-dc90fb69e5d3)


1) Purpose
Create a single duplicate-capture report across three business units—Hiab, Kalmar, MacGregor—each with three regional master files (EMEA, APAC, US). The macro:

Reads the 9 master files and an optional PP Status file.
Consolidates all records.
Flags possible duplicates using well-defined rules.
Populates the Output sheet with:

Master ID (e.g., 5400000214_CITA_385.52)
New Consolidate DP (duplicate key) → yellow highlight when found
New consolidate Clarification (Sheet) (where else the match exists)
Audited Date / PP Status (if provided)




2) Files & UI (Macro‑PP Sheet)

Master files (9 total):

Hiab → EMEA, APAC, US
Kalmar → EMEA, APAC, US
MacGregor → EMEA, APAC, US


PP Status file (optional): to show status/audit info in the Output.

On the Macro‑PP sheet (as shown in your screenshots):

Use the Browse buttons beside each region to select the corresponding file.
Make sure you’ve selected all relevant files before running.
Click the main button: Run Consolidation & Duplicate Check.


The macro uses whatever files you’ve selected in the UI (no Power Query).


3) Required/Recommended Columns in Master Files
To ensure consistent matching and reporting, each master file should contain:

Core identifiers:
Document No, CoCode, Type, Invoice No, Invoice Date, Vendor Code, Vendor Name
Amounts & currency:
Amount in DC (rounded to two decimals for matching), Crcy
(Optional but helpful: Amount in LC, LCRcy)
Dates & context:
Posting Date, Net due date, Username (e.g., BASWARE)


If column names differ slightly (e.g., “Vendor” vs “Vendor Code”), keep the names consistent across the 9 files or document the mapping for the macro.


4) How duplicates are identified (Macro logic—concept, not code)
The macro checks duplicates in three levels. You can choose to use only Strict or include Medium/Loose as fallbacks.
A) Strict (primary rule – recommended)
Key = Invoice No + Vendor Code + Amount in DC (2 decimals) + Currency

Best precision.
Flags when the same invoice number appears for the same vendor with the same amount & currency across any unit/region.

B) Medium (optional fallback)
Key = Invoice No + Vendor Code + Amount in DC (2 decimals)

Ignores currency; useful if currency is always the same or not reliable in some files.

C) Loose (optional fallback)
Key = Vendor Code + Amount in DC (2 decimals) + Invoice Date within ±N days

Catches near-duplicates when invoice number is slightly different or missing.


When a row matches under Strict, you get “Possible Duplicate (Strict)”; if not, the macro tries Medium, then Loose.


5) Output Sheet — Columns & Meaning
Your Output sheet (like the first screenshot) will have:


Master
A human-friendly ID combining key fields (e.g., Document No_Invoice No_Amount → 5400000214_CITA_385.52).


Document No / CoCode / Type / Invoice No / Invoice Date
The original SAP identifiers and dates.


Vendor Code / Vendor Name


Amount in DC / Crcy / Posting Date / Net due date / Username
Financial and process context.


Amount in LC / LCRcy (optional)


Unit / Region
The source business unit and region (Hiab/Kalmar/MacGregor and EMEA/APAC/US).


StrictKey / MediumKey / LooseKey (can be hidden)
The internal match keys used by the macro for duplicate detection.


New Consolidate DP

Shows the key that matched a duplicate (e.g., 5400000214_CITA_385.52).
Yellow highlight when a duplicate is found (like your screenshot).
If no duplicate is found, displays “Not Found”.



Match Reason

Explains which rule matched:
Possible Duplicate (Strict) / Possible Duplicate (Medium) / Possible Duplicate (Loose)
Otherwise: Unique.



New consolidate Clarification (Sheet)

Lists where else the same key appears, with Unit–Region references, for quick follow‑up.
Example: Kalmar-EMEA (Doc 5400002060), Hiab-US (Doc 5400001603)
Shows “Not Found” if there are no matches.



PP Status / Audited Date (optional)

Populated from the PP Status file using Document No (if provided).




6) What you see in practice (based on your screenshots)

When a duplicate is detected, “New Consolidate DP” shows the matching key in yellow (bold), and “Match Reason” says “Possible Duplicate (Strict)”.
“New consolidate Clarification (Sheet)” will list the other files/regions/units where the same invoice appears, so you can quickly notify the team.
If nothing matches, both New Consolidate DP and Clarification show “Not Found”.


7) Operating steps (Month‑on‑Month)

Open the macro workbook.
On Macro‑PP:

Click Browse and assign the 9 master files (Hiab/Kalmar/MacGregor → EMEA/APAC/US).
Optional: select the PP Status file.


Click Run Consolidation & Duplicate Check.
Review:

Consolidated tab → raw combined records with Unit/Region.
Output tab → check New Consolidate DP and Match Reason; duplicates will be yellow.


Filter Output to Match Reason = Possible Duplicate (Strict) for emailing.


8) Email-ready summary (like your second screenshot)
For any duplicates you want to report:

Filter the Output to “Possible Duplicate (Strict)”.
Copy columns (e.g., Document No, CoCode, Type, Invoice No, Invoice Date, Vendor Code, Vendor Name, Amount in DC, Crcy, Posting Date, Net due date, Username) into your email table.
Add a short note:

“Hi Team,
There is possible duplicate in the below payment proposal.”


Paste the table and send.


9) Quality/Control notes

Rounding: Amount in DC is normalized to two decimals for reliable matching.
Invoice No: Keep consistent formatting (avoid accidental spaces or leading zeros changes).
Currency: Included in Strict rule to prevent false matches across currencies.
Performance: Macro processes in memory; suitable for large row counts across 9 files.
Auditability: Match Reason + Clarification make it easy to trace why a row was flagged.


10) What I can tailor for you (still content, not code)

If you want Strict-only detection (no Medium/Loose): set policy to Strict only in the documentation and ignore other keys.
If you prefer a different Master ID format, we’ll document the new convention (e.g., Document No – Invoice No – Amount – Currency).
If some regions have slightly different column names, we’ll add a column mapping table in the documentation so the macro team aligns them before running.

