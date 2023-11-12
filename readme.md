<!-- Add the following script to your README.md file -->

```html
<script>
  // Function to fetch all repositories for a user using GitHub API
  async function fetchUserRepos(username) {
    const response = await fetch(`https://api.github.com/users/${RafaelRodMar}/repos`);
    const data = await response.json();
    return data.map(repo => repo.name);
  }

  // Function to fetch repository languages using GitHub API
  async function fetchRepoLanguages(username, repo) {
    const response = await fetch(`https://api.github.com/repos/${RafaelRodMar}/${repo}/languages`);
    const data = await response.json();
    return data;
  }

  // Replace 'YourUsername' with your GitHub username
  const username = 'RafaelRodMar';

  // Fetch all repositories for the user
  fetchUserRepos(username)
    .then(async repos => {
      const allLanguages = {};

      // Iterate through each repository and accumulate language data
      for (const repo of repos) {
        const languages = await fetchRepoLanguages(username, repo);
        
        // Add languages to the overall count
        for (const lang in languages) {
          if (allLanguages[lang]) {
            allLanguages[lang] += languages[lang];
          } else {
            allLanguages[lang] = languages[lang];
          }
        }
      }

      // Calculate the total lines of code
      const totalLines = Object.values(allLanguages).reduce((acc, val) => acc + val, 0);

      // Calculate the percentage of each language
      const percentages = {};
      for (const lang in allLanguages) {
        percentages[lang] = ((allLanguages[lang] / totalLines) * 100).toFixed(2);
      }

      // Display the language percentages in your README.md
      const languageList = Object.keys(percentages)
        .map(lang => `${lang}: ${percentages[lang]}%`)
        .join(', ');

      document.getElementById('repo-languages').innerHTML = languageList;
    })
    .catch(error => console.error('Error fetching repository languages:', error));
</script>

<!-- Add a placeholder where the language percentages will be displayed -->
Languages Percentage: <span id="repo-languages"></span>
