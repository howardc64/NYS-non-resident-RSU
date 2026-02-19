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
- work from home are dates working from home
- vacation are vacation dates
- domicile work includes dates and where domiciled and worked
- travel rules provides some guidelines on travel schedule and airports
- NYS tax rules include key work day location tax rules
- RSUs are list of RSU blocks
- workday agent is sequence of prompt to the LLM asking it to count work days and non work days
- README is this file
- Results are my LLM run results

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

==== Interesting Observations

- Had a year type on a travel trip. Started in xx/xx/25 and ended in xx/xx/24. Claude flagged and corrected
- On date windows without info, LLM tried to determine the purpose and was correct in one case I verified

==== Development Info

- All work done on macOS (Sonoma/Sequoia) in case any strange text file characters on other OSes

- Prompt ordering probably matters. workday agent starts by building a travel list with estimated travel windows. This knowledge is necessary for later prompts to determine if any time is left in NYS on a normal work day schedule to consider NYS work day.

- NYS rule require any amount of time worked in NYS (or work from home with convenience of employer rule) is a full NYS work day. Flight itinerary without timestamps and trying to determine if possible to work in NYS is challenging. Trying to guess travel window + timezones creates ample opportunity for AI to get lost. For 3+ hour travels with likely time zone losses as well, maybe easier just say no NYS work day.
