# vault-github-secrets-sync

This is a project to Automate to proccess of retrieving secret/env-variables from the Hashicorp-Vault into the Github Secrets Environment.


Vault Configured to run on the local-host

![image](https://github.com/user-attachments/assets/ced136cb-5e5c-47e9-8f15-b62dfaf1ed7f)



Github actions self-hosted runner configured.

Reasons:
-My Vault is runing on the localhost and the runner hosted by github can't communicate with the localhost address(127.0.0.1)

<img width="953" alt="image" src="https://github.com/user-attachments/assets/cdb3ea72-dd7b-45af-88e1-30c2af2dc87f">



Sample Of Successful Secrets Retrieval and Update

<img width="746" alt="image" src="https://github.com/user-attachments/assets/9d4f1ac2-bd46-4a4e-8faa-f53ac8a6363a">



Steps Involved in my workflow.

1. Install jq
Purpose: jq is a lightweight command-line JSON processor. It is used to parse, filter, and format JSON data efficiently.

Why It's Needed:
Vault responses often return data in JSON format.
jq is used to extract specific fields from the JSON responses returned by Vault commands.

2. Install GitHub CLI
Purpose: The GitHub CLI (gh) allows you to interact with GitHub from the command line.

Why It's Needed:
The workflow uses gh secret set to update repository secrets securely.
gh is essential for managing GitHub secrets programmatically within the workflow.

3. Authenticate GitHub CLI
Purpose: Ensures that the GitHub CLI is authenticated with the necessary permissions to perform actions like updating secrets.

Why It's Needed:
GitHub requires authentication for actions that modify repository settings, including updating secrets.
Uses the GH_PAT (Personal Access Token) to authenticate.

4. Verify Vault Token
Purpose: Validates the Vault token to ensure that the workflow can access Vault and retrieve secrets.

Why It's Needed:
Vault uses tokens to authenticate and authorize requests.
This step checks if the provided Vault token (VAULT_TOKEN) is valid.

5. Cache Vault Data Hashes
Purpose: Caches the hashes of the current Vault data for efficient change detection across workflow runs.

Why It's Needed:
To avoid redundant secret updates when there are no changes in the Vault data.
Caching saves time and resources by skipping unnecessary updates.

6. Check Vault for Changes
Purpose: Compares the current Vault data with the cached data to detect changes.

Why It's Needed:
Detects whether any secrets in Vault have been modified.
Ensures that secrets are updated only when changes are detected, reducing unnecessary updates.

7. Fetch and Merge Secrets from Vault
Purpose: Fetches updated secrets from Vault and merges them into a format suitable for GitHub secrets.

Why It's Needed:
Secrets need to be fetched and formatted before they can be updated in GitHub.
This ensures the latest data from Vault is prepared for the GitHub repository.

8. List Merged Secrets Files
Purpose: Lists the merged secrets files created in the previous step for verification and debugging.

Why It's Needed:
Ensures that the merged secrets files were created successfully.
Provides visibility into which secrets are prepared for update.

9. Update GitHub Secrets
Purpose: Updates the GitHub repository secrets with the merged secrets from Vault.

Why It's Needed:
Ensures that the latest secrets from Vault are securely stored in the GitHub repository.
These secrets can then be accessed by other workflows or applications.

10. Post Cache Vault Data Hashes
Purpose: Updates the cached Vault data hashes to reflect the latest state after secrets are updated.

Why It's Needed:
Ensures that the cached hashes are updated so that future workflow runs can accurately detect changes.
Prevents redundant updates in subsequent workflow runs.

11. Complete Job
Purpose: Marks the end of the workflow job.

Why It's Needed:
Indicates that all steps in the workflow have been successfully executed or skipped based on conditions.
Ensures proper cleanup and final status reporting.
