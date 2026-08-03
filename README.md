# EU POLITICIANS’ HABIT OF SKIPPING WORK ON THURSDAYS MAY HAVE COST EUROPEANS THEIR PRIVACY -- Arvid Grange -- The Lede Program at Columbia University

## Project purpose - Testing if low attendance patterns in the EU Parliament may have affected the outcome of the July 9th vote on extending controversial privacy law "Chat Control 1.0"
By analyzing years of voting records from the EU Parliament -- both second reading votes and regular votes -- we should be able to see if there is a pattern of voting results are affected by low attendance in the plenary chamber.

This metric is important for second reading votes, which require an absolute majority, meaning more than half of the maximum number of seats in parliament (720), regardless of how many MEPs are present during voting.

The vote on extending the ePrivacy Directive, or Chat Control 1.0 as it is called colloquially, was such a second reading vote.

## Findings
The findings show that second reading votes are very rare. Even rarer are instances when the EU Parliament gets the absolute majority required to overturn the EU Council's decision at a second reading.

Only 5 of 96 second reading votes between 2014 - 2026 (excluding 2020-2022) overturned the Council position. Another 5 of them reached a simple majority, the requirement for winning a first reading vote. All of these were votes on Chat Control, indicating that this might be a more polarizing issue.

More findings showed the pattern of absences in the plenary chamber during voting weeks in Strasbourg. The data supports anecdotal evidence of MEPs skipping work on Thursdays in order to take earlier trains back to Brussles. Attendance peaks on Tuesdays and drops substantially from there to Thursday afternoons.

I also found that of the 50 worst attended plenary days between 2023 - 2026 almost half, 23 of 50, were Thursdays.

Since the data sample of second reading votes was so small, no confident conclusions could be drawn from it. Therefore I also analyzed voting results of regular votes, per weekday, over the past 2.5 years, and compared what percentage of votes in favour of a law reached absolute majority numbers (>360) during regular votes.

This is meant as a proxy metric for understanding what effect lower attendance could have on second reading hearings, despite there not being sufficient data among those readings to analyze.

This finding showed that the percentage of votes reaching absolute majority numbers also drops substantially during the voting week, with 82% on Tuesdays to 61% of votes on Thursdays.

## Data collection process
The data was collected using the European Parliament's API: https://data.europarl.europa.eu/en/developer-corner/opendata-api

Important note: No data between 2020 - 2022 was used, since voting during the covid-19 pandemic was done remotely. Using that data would have skewed the results, since we are interested in absence effects.

Getting the second reading votes proved difficult and had to be done in several steps, first getting all plenary days since 2014, then the voting sessions (VOT-items) and then specific votes held during those sessions (DEC-items).

Some challenges were that the results made clear that the EU Parliament changed the structure of this data several times during the years analyzed. They also changed the API from "v1" to "v2", without clearly documenting this. These diagnostics took a long time and required several spot checks to see if I was missing data, which I was. To fix this I designed a loop searching for the following keywords to find second readings: ["***ii", "*** ii", "second reading", "deuxième lecture", "deuxieme lecture", "deuxi\u00e8me lecture"].

Another loop was implemented to download a vote item's xml file of the meeting minutes, if a DEC-item was missing in the json, and analyze this file separately to find voting outcomes.

The data collection of attendance numbers was instead done with data from 2023 - 2026 since the API data for this parameter was reliable during those years.

## Data analysis
Analysis was split in two categories: second reading votes and attendance numbers.

Second reading votes:
Finding the rate of second reading votes resulting in the EU Council's position being overturned.

This required the creation of a new column, "Council win or loss". Keywords like "rejected" was not sufficient since some votes labeled as rejected meant the Council's position was rejected, while other votes labeled as rejected meant the vote to reject the Council's position was rejected. This required a fair bit of manual parsing of the second reading votes.

Further analysis was to find what percentage of the 2nd readings that were votes held on July 9th 2026, i.e. votes held on the extension of the ePrivacy Directive, as well as how many 2nd reading votes reached simple majority numbers.

The attendance for each day was calculated using simple averages grouped by weekday. To find the difference between attendance on Thursday mornings to Thursday afternoons analysis was done on DEC-items (decisions, or individual votes), rather than PL-items, which record the attendance for a given day. DEC items provide more detailed time stamps paired with attendance.

Afternoon attendance was found by analyzing votes held after 12.00 on Thursdays.

Finding percentages of regular votes reaching absolute majority numbers was done by gathering all DEC-items 2023-2026 (1822 items) and filtering away all items without recorded attendance numbers before analysis.

## New skills and approaches
Diagnosing API errors and undocumented changes in API structure. Using nested loops to search for different information sources (keywords in API:s -- if missing pivoting to links to xml documents, downloading those and searching for other keywords and numbers -- then merging these to one data frame.)