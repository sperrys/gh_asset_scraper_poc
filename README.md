# gh_asset_scraper_poc

### Proof of Concept Repo for scraping GitHub asset urls to local files. 


### Problem 

For a couple of reasons, github asset urls (eg. https://github.com/user-attachments/assets/guid), embedded in markdown files, don't render on GitHub pages in the same way they do when visiting a repo on GitHub.com. While this pattern had previously worked for GitHub Pages, it's not supported. 

### Solution 

Following best practices, repositories meant for GitHub pages sites should instead use a dedicated  `assets` or `media` directory in their repository as a backing store. Markdown files can then use relative links to properly display relevant images without worrying about availability of the image or needing to make a network call. 

Migrating to this pattern can be cumbersome if a user had previously followed the unsupported pattern. 


To help automate the migration process, this repo demos a custom GitHub action [workflow](./.github/workflows/scrape_assets.yml) that:
 - Scraping relevant GitHub asset urls from a repository's markdown files
 - Downloading the underlying files to an `assets` directory
 - Replacing the http links with relative links to the assets directory
 - Doing this ^ in a dedicated branch, and creating a pull request for review upon script completion. 


### Considerations 
- The scraper will match asset urls that take the form `{github_instance_url}/user-attachments/assets/{guid}` since it is the default canonical pattern. The script can be adapted to consider other url patterns. 


### Running in A Repo 

In order for the workflow to run there's a few minor setup steps. 

- `scrape_assets.yml` should be placed in the `.github/workflows` directory like it is in this repo, or would be for  any repo that uses [workflows](https://docs.github.com/en/actions/writing-workflows/about-workflows).
- In order for the workflow to be able to create a branch and pull request, the user should make a dedicated [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#about-personal-access-tokens)


![meow-nod-fast](https://github.com/user-attachments/assets/01eca72e-4bc0-4b64-9e1a-c43eb0af5bae)
![test_image](https://github.com/user-attachments/assets/05f2aee1-a99d-4883-9d30-b9c17271f0d3)



