# postcard-party-deduping
## Problem 
Over the summer of 2018, I helped with the James Smith campaign’s Postcard Party Initiative. The staff received almost 100,000 names and addresses of registered voters who had voted for Democrats in the past but had not voted recently. This demographic was being targeted by the campaign’s GOTV efforts, and volunteers who wanted to hold Postcard Parties to send postcards to hundreds of these voters were assigned a couple hundred names at a time. Because the campaign was already at a fundraising disadvantage, the campaign wanted to ensure that all unique voters received a postcard before duplicates were sent out to make the most of donor money. 
This project contains sensitive information (voter names and addresses), so the files themselves cannot be shared. However, I will explain the methodology of how I deduplicated these spreadsheets.
The information came in three different formats:

A.	Master document format. This was the file of addresses that needed to be assigned to volunteers, and this spreadsheet contained the 97,000 names and addresses in the following format. 

| Name | City | State | Five Digit Zip | Four Digit Zip | Address |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| John M. Doe   | Greenville  | SC | 29208 | 3725 | 16 Main St |
| Jane Roberts | Mauldin | SC | 29206 | 3145	| PO Box #7274 |
| John R. Doe | Simpsonville	| SC	| 29201	|3266	| PO Box #7274 |

This file was given to the new staff members by the campaign manager who had previously been handling data in one of the two following formats.

B.	Before the postcard party initiative was handled by the staff members that I was working with, the campaign manager sent out names in individual Excel documents to volunteers in two different ways. The following method is very similar to the format of the master document, with the main difference being the presence of a unique VANID identifier. 

| VANID	| Name | City | State | Five Digit Zip | Four Digit Zip | Address |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
|11111	| John M. Doe	| Greenville |	SC |	29208	| 3725	| 16 Main St |
|22222	| Jane Roberts | Mauldin |	SC	| 29206 |	3145 |	PO Box #7274 |
|33333	| Dorothy Adams |	Simpsonville | SC	| 29201	| 3266	| PO Box #7274 |

C.	The campaign manager also previously sent names to volunteers in the following way:

| First Name	| Last Name	| City	| State |	Five Digit Zip |	Address |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
|John | 	Doe	| Greenville	| SC	|29208	| 16 Main St |
|Jane |	Roberts	| Mauldin	| SC	|29206	| PO Box #7274 |
|John	| Doe	| Simpsonville	| SC	|29201	| PO Box #7274 |

All the names in file formats B and C have already been sent to volunteers, so the end goal was to determine whether or not a person in the master file could be located in one of those two types of spreadsheets. 

An additional issue arose when I later realized that some of the data in the formats did not in fact follow the formats listed above at all, but these names were still valid addresses that needed to be deduplicated.

| Name | City | State | Five Digit Zip | Four Digit Zip | Address |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| John M. Doe   | Greenville  | SC | 29208 | 3725 | 16 Main St |
| The Doe Family  | Greenville  | SC | 29208 | 3725 | 16 Main St |


| Name | City | State | Five Digit Zip | Four Digit Zip | Address |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| Jane Roberts and Cecilia Smith | Mauldin | SC | 29206 | 3145	| PO Box #7274 |
| Jane Roberts | Mauldin | SC | 29206 | 3145	| PO Box #7274 |

When trying to compile these three separate files into one document, it becomes clear that each unique person needs to have an identifying key to determine whether or not the person is in the master file. However, problems arise when trying to create an identifier:
1)	We cannot use name alone as an identifier because cases like John M. Doe vs John R. Doe indicate two different people, but would appear to be the same in spreadsheets in format C. 
2)	We cannot use address alone as an identifier because cases like Jane Roberts’s address vs John Doe’s address, where the difference lies in a zipcode or city, would not be handled correctly. 
3)	We cannot use name AND address alone because cases like John M. Doe at 16 Main St vs John Doe at 16 Main St would not be flagged as the same person. 

## Solution
In order to account for all of these conditions, I created two unique IDs, one based on name and address, and one based on address and zipcode. The first identifier caught all of the cases where the names were exact matches, and the second identifier caught cases where the names were not exact (typically a middle initial difference or incorrect formatting) or where two addresses were the same but had different zipcodes. 

To do so, I parsed each separate word in the name column in Microsoft Excel  to separate first, middle, and last names, and to check for cases where appropriate formatting was not followed. I checked that formatting was followed by checking the number of columns a name took up (if it had more than 4 words, it was typically incorrectly formatted). I also created a new name field called Name1 for Format C, where first and last name were separated, by concatenating first and last name. 

I combined all the separate spreadsheets in formats B and C into two tables in Microsoft Access and imported the master spreadsheet as a third table. 

To catch all of the exact matches, I ran queries 1 and 2 in the query sheet. These queries alone caught 3000 duplicates between the master file and the addresses already sent to volunteers. Following this, I used the address-zipcode identifier to find other duplicates (queries 3 and 4). This method was repeated with the Format C names. New tables of the matches were created from these two queries. Over 5000 duplicates were found through these queries. 

After these queries were constructed to remove as many duplicates as possible, the information was then put back into a spreadsheet for staff to use. I used color formatting and filters, and I taught the staff how to use the filters to extract the information they wanted from the spreadsheet. My efforts prevented approximately 2000 names from being sent out as duplicates.

