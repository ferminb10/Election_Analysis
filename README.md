# Election_Analysis
Performing analysis on Election data to uncover trends.
## Overview of Project
This weeks challenge helps us build upon our skills learned in the python module. For deliverables, the client wanted aditional information analyzed from the election results.  To complete the audit, for loops, conditional statements with memberships and logical operators were used to investigate parameters such as percentage vote by county, voter and county turnout.

## Results
The analysis had few tasks. First, a solution code was refractored to loop through all stock data one time in order to collect the same information that you did in this module. Then, it was determined if that our refactored code successfully performed better than the original script, the results shown below.


Using a bulleted list, address the following election outcomes. Use images or examples of your code as support where necessary.

How many votes were cast in this congressional election?
Provide a breakdown of the number of votes and the percentage of total votes for each county in the precinct.
Which county had the largest number of votes?
Provide a breakdown of the number of votes and the percentage of the total votes each candidate received.
Which candidate won the election, what was their vote count, and what was their percentage of the total votes?
There is a bulleted list where each election outcome is addressed. (7 pt)



### Election Results
![election_results](https://user-images.githubusercontent.com/107658895/176082445-ede641f1-8329-431c-8e59-a02f2ff13f01.png)

#### The figure above shows the election results.

# Code:

#Add our dependencies.

import csv
import os

#Add a variable to load a file from a path.

file_to_load = os.path.join( "Resources", "election_results.csv")
#Add a variable to save the file to a path.
file_to_save = os.path.join("analysis", "election_analysis.txt")

#Initialize a total vote counter.

total_votes = 0

#Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

#1: Create a county list and county votes dictionary.
county_list = []
county_votes = {}

#Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0

#2: Track the largest county and county voter turnout.
largest_county_turnout = ""
largest_county_turnout_count = 0
largest_county_percentage = 0

#Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    # Read the file object with the reader function.
    reader = csv.reader(election_data)

    #Read the header
    header = next(reader)

    #For each row in the CSV file.
    for row in reader:

        #Add to the total vote count (either work)
        #total_votes = total_votes + 1
        total_votes += 1

        #Get the candidate name from each row.
        candidate_name = row[2]

        #3: Extract the county name from each row.
        county_name = row[1]

        #If the candidate does not match any existing candidate add it to the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        #Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        #4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_list:

            #4b: Add the existing county to the list of counties.
            county_list.append(county_name)

            #4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0

        #5: Add a vote to that county's vote count.

        county_votes[county_name] += 1

#Save the results to our text file.
with open(file_to_save, "w") as txt_file:

    #Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n\n"
        f"County Votes:\n"
    )
    
    #Print election results to the terminal.
    print(election_results, end="")
   
    #Saves the final vote counts to the .txt
    txt_file.write(election_results)

    #6a: Write a for loop to get the county from the county dictionary.
    for county in county_list:
        
        #6b: Retrieve the county vote count.
        county_vote = county_votes.get(county)
        
        #6c: Calculate the percentage of votes for the county.
        county_vote_percentage = float(county_vote) / float(total_votes) * 100

         #6d: Print the county results to the terminal.
        county_results = f"{county}: {county_vote_percentage:.1f}% ({county_vote:,})\n"
        
        #Print county votes results to the terminal.
        print(county_results)

         # 6e: Save the county votes to a .txt file.
        txt_file.write(county_results)
         
         #6f: Write an if statement to determine the winning county and get its vote count.
        if (county_vote > largest_county_turnout_count) and (
            county_vote_percentage > largest_county_percentage
        ):
            #True
            largest_county_turnout_count = county_vote
            largest_county_percentage = county_vote_percentage
            largest_county_turnout = county

    #7: Print the county with the largest turnout to the terminal.
    winning_county_print = (
        f"-------------------------\n"
        f"Largest County Turnout: {largest_county_turnout}\n"
        f"-------------------------\n"
    )

    #Print winning county print with the largest county turnout to the terminal.
    print(winning_county_print)

    #8: Save the county with the largest county turnout to a .txt file.
    txt_file.write(winning_county_print)

    #Save the final candidate vote count to the .txt file.
    for candidate_name in candidate_votes:

        #Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n"

        #Print candidate results in terminal
        print(candidate_results)

        #Save the candidate results to our .txt file.
        txt_file.write(candidate_results)

        #Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    #Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n"
    )
    
    #Print winning candidate summary in terminal
    print(winning_candidate_summary)

    #Save the winning candidate's name to the .txt file
    txt_file.write(winning_candidate_summary)


## Summary
Based on the results of this test the refractered code was a more efficient version of the original script. This is one of the benefits of refractoring code. You can take code and try to optimize. The limitations of this dataset is that it only provides a small dataset. This script doesn't do a good job of testing its limits. I was curious to know how long it would take to run the S&P 500. One of the disadvantages of refractoring code for the particular application of analyzing thousands of stocks is that the program may take a long time to execute. You can easily make this to your advantage if you're well educated in economics and have it tailored to a specific set that you want. The refractored script proved to be a much more efficient than the original based on the executed times. The faster it can be the more applicible it can be for a wider range of datasets.



In a summary statement, provide a business proposal to the election commission on how this script can be used—with some modifications—for any election. Give at least two examples of how this script can be modified to be used for other elections.
There is a statement to the election commission that explores how this script can be used for any election, with two examples for modifying the script. (4 pt)


