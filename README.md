# UN Careers Job Extraction Bot

An Automation Anywhere (A360) bot that goes through the job listings on the UN Careers portal, collects the details of each job, filters them by job network and duty station, and saves everything into an Excel file with a text log of the run.

This is an independent learning project built during my AI & Intelligent Process Automation internship at Orion Valley Academy. It is not affiliated with or endorsed by the United Nations. The bot only reads publicly listed job postings.

## What the bot does

Searching the portal by hand means opening every page, reading every card, and copying details into a spreadsheet. The bot does that loop automatically:

1. Opens the UN Careers job search page with the filters already applied in the URL.
2. Goes through each page of results until there is no next page.
3. For every job card on a page, it captures the details below.
4. Sorts each job into one of two Excel sheets depending on the rules below.
5. Writes a line to a log file for every job it processes.
6. Cleans up, saves the workbook, and shows a summary when it is done.

## Data collected per job

- Job Name
- Job ID
- Job Link
- Job Network
- Duty Station
- Date Posted
- Deadline

## Filtering logic

Each job goes through these checks in order:

1. Is the job ID the same as the previous one? If yes, it is skipped, which stops duplicates being written.
2. Is the job network "Information and Telecommunication Technology" or "Logistics, Transportation and Supply Chain"? If not, the job goes to the Excluded Jobs sheet.
3. If it is one of those two networks, is the duty station Kabul or Nairobi? If yes, it goes to the Excluded Jobs sheet.
4. Everything else goes to the Main Jobs sheet.

## How it works, step by step

1. Setup. Records the start time, creates the log file, creates an Excel workbook with two sheets (Main Jobs and Excluded Jobs) and writes the headers on both.
2. Launch. Opens the filtered UN Careers search in the browser, maximizes the window and waits for the page to load.
3. Outer loop. Keeps going while a next page exists.
4. Inner loop. Goes through the 10 job cards on the current page. For each one it captures the title, link, network, duty station, date posted and deadline using the Recorder.
5. Cleaning. Removes the label text from the captured values (for example "Job ID :" or "Deadline :") so only the actual value is left in the cell.
6. Routing and logging. Applies the filtering logic above, writes the row to the correct sheet, and logs the job to the text file.
7. Pagination. If the next page button exists it clicks it and waits. If not, the loop stops.
8. Finish. Autofits the columns on both sheets, saves and closes Excel, closes the browser tab, calculates how long the run took, shows a summary message box, and logs the final stats.

## Technical notes and limitations

- The Job ID is not captured directly. It is extracted from the job link by taking the text between `jobSearchDescription/` and `?`. If the UN Careers site changes its URL structure, this will break and need updating.
- The job name is taken from the HTML title property of the job link, so it also depends on the page structure staying the same.
- The inner loop assumes 10 jobs per page. A page with a different number of results would need that value changed.
- The last page is detected through a Try/Catch block. When the bot tries to reach a next page that does not exist, the error is caught on purpose and used to end the loop cleanly.
- The Excel and log file paths are set for my own machine. Change them in the bot before running it somewhere else.

## Tools used

- Automation Anywhere A360 (Recorder, Excel advanced, String, Loop, If, Error handler, Logging actions)
- Microsoft Excel
- Google Chrome

## Repository contents

- `UN Careers Job Extraction Bot/`: the exported bot package from Control Room
- `README.md`: this file

## Running the bot

1. In Control Room, import the exported package.
2. Open the bot and update the Excel and log file paths to ones on your machine.
3. Run the bot and let it go through all the pages.
4. When it finishes, open the Excel file to see the Main Jobs and Excluded Jobs sheets, and check the log file for the run details.

## Author

Marwan Karam
Computer Engineering student
LinkedIn: https://www.linkedin.com/in/mkaram209/
