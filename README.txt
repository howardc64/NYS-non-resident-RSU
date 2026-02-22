==== WARNING and CONSIDERATIONS

LLM is notorious for making mistakes. It can generate a lot of stuff and its up to the human to verify to make sure no mistakes

Repeated runs of same data and prompt to LLM often produce different results. Sometimes stylistic. Sometime different summaries. Sometimes even different resulting answers! Every run can even take different process and use different tools. Mainly depends on how much complexity and ambiguity is in the data set and prompts.

An effective solution is just ask LLM to generate initial results and prompt it to fix obvious bugs followed by manual adjustment for final result. Trying to use LLM to step by step generate the final result reliably may not be time efficient as LLM can fix one bug while introduce a different one with each new run.

LLM will make some mistakes but so will a human doing this work. Archeological data isn’t perfect and we don’t have a minute by minute accounting of every day. These files contain suggestions for the LLM to make estimates. And also some obvious info that an LLM should already understand. These are added and represent "hard rules" to minimize LLM errors. Personally I think doing by hand will take a long time and the person will be tired and confused by all the tax rules in their head during the process. This will likely result in mental mistake and typos.

Generated result from the LLM prompts is a spreadsheet like a daily timesheet with a 1 marked to the type of day it is. A summary sheet then sum up these day types for each RSU block to produce the numbers for NYS's RSU prorating tax form (IT-203F Sched B)

CPAs (Much fewer doing non-resident RSUs and probably charge $$) might assign a junior person that would tackle small sections of dates at a time and work through them slowly. Can't imagine it's cheap to achieve precision. Foreign expat / cross state tax analysis/prep service providers might have developed procedures and tools to step by step churn through the data to arrive at the figures. They probably mainly target companies with relocation needs as clients rather than consumers.

==== Files

All the files are pretty self explanatory

- work travel is a list of dates traveling for work
- holidays are company holidays
- work from home are dates working from home according to NYS tax code (only WFH days when assigned to NYS work office) Do not enter WFH when assigned to outside NYS work office.
- vacation are vacation dates
- sick days are list of sick days
- domicile work includes dates and where domiciled and worked
- travel rules provides some guidelines on travel schedule and airports
- NYS tax rules include key work day location tax rules
- basic rules are obvious rules serving as a guide rail to the LLM to minimize mistakes
- RSUs are list of RSU blocks
- spreadsheet spec is the spreadsheet construction rules
- workday agent is sequence of prompt to the LLM asking it to count work days and non work days
- README is this file
- Results folder are my LLM run results

On the LLM chat session (Results/Claude.pdf), note the LLM checking its work carefully. Finding mistakes and eventually got it correct. These likely occurred with the addition double check requests in the workday agent prompt sequence

I have had runs where LLM made mistakes with the slightest ambiguity

For each RSU block total days, weekend, holiday, NYS workday non-NYS workday WFH vacation sick Enter the #s directly into IT-203F Sched B form. Form directions then will compute the workday fraction using these numbers. 

==== LLM

- Info from (2/16/26)
- Claude free tier Claude Sonnet 4.5
- Free tier chatGPT could not upload more than 3 files. Tried combining files together and still didn't work. Seems to require more precise data formats.

==== How to Run

- start new chat session in LLM (setup a free account if don’t have one)
- upload all files except README and the Results folder
- type “run workday agent” and wait for LLM to build the spreadsheet :)

==== Results

In Results Folder

- AI produced spreadsheet
- AI chat log

==== Itinerary Formats

AI is asked to ignore everything to the right of XXX These are just notes for human readers

There are some ambiguities that will confused an LLM and its generated code. Here are some cases

First make the itinerary as precise and consistent as possible. My examples show

Date departure time arrival time (note)

This website can find all details if have departure and arrival airports with just 1 of the times. If no time stamp, It will give you all flights the took place on that date between the 2 airports.

https://www.flightera.net/en/search

Day trips

1. Date X departure A destination B
2. Date X departure B destination A

Because there are no time stamps, LLM can see 2 happen before 1 and get all confused. Can solve by adding some time hints like destination arrival for 1 in the morning and 2 in the evening.

Multi leg multi purpose trips

This can be hard to specify. First have to chop up different purposes into different segments and put in the correct file (vacations, WFH, work trip)

Also may be necessary to note staying in a location for a particular purpose rather than construct travel itineraries. For example

After travel itinerary, just add (vacation until date X)

==== Travel classification

One of the most difficult and require a lot of context to get correct and AI often gets it wrong Amongst these

For business trip, need to understand where location is the business trip. This gets challenging in trips combining other personal activities and > 2 legs.

NYS tax rule require any amount of time in a day spent working in NYS be marked as NYS workday. Adding more guidance to LLM to estimate with travel windows and time zones can also increase ambiguity and complexity causing errors (seen in my correction prompts to Claude in Results folder).

Easiest solution maybe for mark travel as non-NYS workday for any travel window > 4-5 hours. However, this maybe wrong in late day westward flights from NYC/NYS.

==== NYS Tax non-resident income tax Laws and Forms

The calculations for the forms are very convoluted requiring a daily log construction to avoid double counting. For example, if a day is vacation day and a sick day, IT-203F Sched B (and also IT-203B Sched A but this form doesn't handle RSUs) computation sequence require they only be counted once.

==== Interesting Observations

- Had a year type on a travel trip. Started in xx/xx/25 and ended in xx/xx/24. Claude flagged and corrected
- Had typo in airport code ERW and LLM correctly guess it was EWR based on other entires.
- On date windows without info, LLM tried to determine the purpose and was correct in one case I verified

==== Development Info

- All work done on macOS (Sonoma/Sequoia) in case any strange text file characters on other OSes

- Prompt ordering probably matters. workday agent starts by building a travel list with estimated travel windows. This knowledge is necessary for later prompts to determine if any time is left in NYS on a normal work day schedule to consider NYS work day.

- If travel heavy like this example, run the first prompt repeatedly to make sure get a good travel log while cleaning up data set and prompt. Correctly built travel log is key for the remaining tasks.