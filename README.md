Command to get the commit-messages and changed files: 
git log --name-only --pretty=format:"%ad - %an: %s" --after="2023-01-01" --until="2025-11-11" > git_log_output.txt  
>> write to git_log_output.txt  