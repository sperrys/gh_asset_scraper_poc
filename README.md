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
- In order for the workflow to be able to create a branch and pull request, the user should make a dedicated [personal access token] (https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#about-personal-access-tokens) scoped to the repo, with the minimun permissions attached (see screenshot).

![image](assets/157f07d5-25b6-4975-851d-742682efbbe4)

- After creation, copy the generated value into an actions secret with the name `GH_SCRAPER_PAT`. Take consideration that any collaboraters can also now use this secret with this permission set in other worflow runs. Depending on circumstance, this might mean you want to remove the secret immediately after running the workflow script. 

![image](assets/581ba81b-e449-4a34-bb79-10267bae41aa)

- Setup should now be complete, and the script can be [triggered from the actions tab](https://docs.github.com/en/actions/managing-workflow-runs-and-deployments/managing-workflow-runs/manually-running-a-workflow).  

![image](assets/33410d6d-a7e3-4170-87b5-71d9fde6acf2)

