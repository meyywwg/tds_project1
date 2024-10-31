# GitHub User and Repository Data Scraper

This project involves scraping user and repository data from GitHub using the GitHub API. The primary goal is to gather data on developers in Mumbai who have a minimum of 50 followers. The data is stored in two CSV files: `users.csv` and `repositories.csv`.

## Overview of the Scraping Process

1. **GitHub API Token**: 
   - The script requires a GitHub API token for authentication. This token is included in the request headers to ensure authorized access to the API.

2. **Fetch Users**:
   - The `fetch_users` function queries the GitHub API for users based in a specific location (default: Mumbai) who have more than a specified number of followers (default: 50).
   - The API endpoint used is:
     ```
     https://api.github.com/search/users?q=location:Mumbai+followers:>50&page={page}&per_page=100
     ```
   - The function retrieves paginated results until there are no more users to fetch, appending each user’s data to a list. Important fields collected include:
     - `login`: GitHub username
     - `name`: User’s name
     - `company`: User’s company (if provided)
     - `location`: User’s location
     - `email`: User’s email address (if provided)
     - `hireable`: Hireable status
     - `bio`: User’s biography
     - `public_repos`: Number of public repositories
     - `followers`: Number of followers
     - `following`: Number of users the user is following
     - `created_at`: Account creation date

3. **Fetch User Repositories**:
   - For each user retrieved, the `fetch_repositories` function is called to collect their repositories.
   - The API endpoint for fetching repositories is:
     ```
     https://api.github.com/users/{user_login}/repos?per_page=100&page={page}
     ```
   - Similar to the user fetching process, this function also retrieves paginated results, collecting repository details such as:
     - `login`: GitHub username
     - `full_name`: Full name of the repository
     - `created_at`: Creation date of the repository
     - `stargazers_count`: Number of stars received
     - `watchers_count`: Number of watchers
     - `language`: Programming language used
     - `has_projects`: Indicates if the repository has projects enabled
     - `has_wiki`: Indicates if the repository has a wiki enabled
     - `license_name`: Type of license for the repository (if any)

4. **Data Saving**:
   - The collected user and repository data are saved to CSV files using the `save_users_to_csv` and `save_repositories_to_csv` functions, respectively. Each function writes the data into a specified CSV file with appropriate headers.

5. **Rate Limiting**:
   - To avoid hitting API rate limits, a `time.sleep(1)` call is included after each request. This ensures that there is a one-second pause between requests.


## Interesting Findings

After analyzing the data, one of the most surprising findings was that the programming language with highest number of stars per repository is TSQL.

Alos there exists a very weak positive correlation between users' **number of repositories** and their **number of followers**. 

There also exists a negative correlation between the  word count in the **bio** and the number of **followers**. Contrary to common belief that a longer, more detailed bio would attract more followers, the analysis revealed that users with shorter bios  actually garnered a higher average number of followers compared to those with extensive bios. 

Additionally, we observed that developers who indicated they were **hireable** were more likely to share their email addresses, hinting at a possible trend where more open communication facilitates networking and professional opportunities.


