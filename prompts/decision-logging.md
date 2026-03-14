Create a decision logging system. Every time I describe a decision I'm making, log it to decisions.csv with: date, decision, reasoning, expected outcome, and a 30-day review date.

Set up a cron job that checks daily if any decisions have hit their review date and appends a "REVIEW DUE" flag. Build a review.sh script that surfaces only those flagged items.
