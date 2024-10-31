# GitHub User and Repository Data Scraper

This project involves scraping user and repository data from GitHub using the GitHub API. The primary goal is to gather data on developers in a specific location (Mumbai) who have a minimum number of followers. The data is stored in two CSV files: `users.csv` and `repositories.csv`.

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

## Running the Script

To execute the scraping process, simply run the script. It will create two CSV files (`users.csv` and `repositories.csv`) containing the scraped data. Ensure that you have the required permissions for the GitHub API and a valid API token.

## Important Considerations

- **API Rate Limits**: Be aware of GitHub's API rate limits, especially if you are scraping large amounts of data. You may need to manage your token usage to avoid being temporarily blocked.
- **Data Quality**: The quality of the data depends on the completeness of the users' profiles on GitHub. Some fields may be missing, such as email addresses or bios.

## Conclusion

This script provides a simple and effective way to gather data on GitHub users and their repositories, focusing on developers in specific locations with a minimum follower count. It can be customized further for various parameters as needed.
