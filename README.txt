==== WARNING and CONSIDERATIONS

LLM is notorious for making mistakes. It can generate a lot of stuff and its up to the human to verify to make sure no mistakes

Repeated runs of same data and prompt to LLM often produce different results. Sometimes stylistic. Sometime different summaries. Sometimes even different resulting answers! Every run can even takes different process and use different tools. Mainly depends on how much complexity and ambiguity is in the data set and prompts.

An effective solution is just ask LLM to generate initial results and prompt it to fix obvious bugs followed by manual adjustment for final result. Trying to use LLM to step by step generate the final result reliably may not be time efficient as LLM can fix one bug while introduce a different one with each new run.

LLM will make some mistakes but so will a human doing this work. Archeological info isn’t perfect and we don’t have a minute by minute accounting of every day. These files contain suggestions for the LLM to make estimates. And also some obvious info that an LLM should already understand. These are added and represent "hard rules" to minimize LLM errors. Personally I think doing by hand will take a long time and the person will be tired and confused by all the tax rules in their head when completed. This will likely result in mental mistake and typos.

Generated result is a spreadsheet but it’s really just tables of numbers and no formulas. Not sure it’s possible to construct the complex spreadsheet formulas to account for all the requirements. Through repeated testing, I can see the complexity and many edge cases

- Every working date range need to eliminate weekends, holidays, sick days, travel time
- These days can over lap. For example sick day on a weekend, holiday, or vacation.
- Holidays can occur on weekends and employers often bump to subsequent Monday.
- On travel days with potential NYS working hours, travel time need to be estimated to determine if any time remain to work in NYS location.

As noted above doing all this correctly while applying the list of tax law requirements probably drives someone insane after a couple of days :)

CPAs (Much less doing non-resident RSUs and probably charge $$$) might assign a junior person that would section out date blocks and work through them slowly. Can't imagine it's cheap to achieve precision. Foreign expat tax analysis/prep service providers might have developed a set of tools to step by step churn through the data to arrive at the figures.

==== Files

All the files are pretty self explanatory

- work travel is a list of dates traveling for work
- holidays are company holidays
- work from home are dates working from home according to NYS tax code (only WFH days when assigned to NYS work office) Do not entire WFH when assigned to outside NYS work office.
- vacation are vacation dates
- domicile work includes dates and where domiciled and worked
- travel rules provides some guidelines on travel schedule and airports
- NYS tax rules include key work day location tax rules
- basic rules are obvious rules serving as a reminder to the LLM
- RSUs are list of RSU blocks
- spreadsheet spec is the construction guidance
- workday agent is sequence of prompt to the LLM asking it to count work days and non work days
- README is this file
- Results folder are my LLM run results
	Note in the chat I had to correct the LLM multiple times (know this data well after working with it for a few days) despite having all the rules in place. The reason is LLM is generating code to generate the spreadsheet with errors in the code.
	For each RSU block total days, weekend, holiday, NYS workday non-NYS workday WFH vacation sick #s are directly entered into IT-203F Sched B form. Form then compute numerator for the workday fraction using these numbers. 

==== LLM

- Info from (2/16/26)
- Claude free tier Claude Sonnet 4.5
- Free tier chatGPT can not upload more than 3 files. Tried combining files together and still didn't work. Seems to require more precise data formats.

==== How to Run

- start new chat session in LLM (setup a free account if don’t have one)
- upload all files except README and the Results folder
- type “run workday agent” and wait for LLM to build the spreadsheet :)

==== Results

In Results Folder

- AI produced spreadsheet
- AI chat log

==== Travel classification

One of the most difficult and require a lot of context to get correct. Amongst these

For business trip, need to understand where location is the business trip. This gets challenging in trips combining other personal activities and > 2 legs.

NYS tax rule require any amount of time in a day spent working in NYS be marked as NYS workday. Adding more guidance to LLM to estimate with travel windows and time zones can also increase ambiguity and complexity causing errors (seen in my correction prompts to Claude in Results folder). The easiest solution maybe for mark travel as non-NYS workday for any travel window > 4-5 hours. However, this maybe wrong in late day westward flights from NYC/NYS.

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