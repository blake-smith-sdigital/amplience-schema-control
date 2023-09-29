This workflow is designed to automate the process of updating Amplience schemas and synchronizing associated content types when changes occur in the specified directory.

Get Event Information: Retrieves information about the triggered event and sets environment variables accordingly, including the event name, pull request type, current branch, destination branch, and whether the pull request was merged.

Check for Amplience File/Schema Changes: Checks for changes in Amplience schemas based on the commit/pull request type and branch. If the pull request is closed and merged, it uses the GitHub API to retrieve the changed files. Otherwise, it uses Git commands to compare the files between the base branch and the current branch.

Check for Hub to update: If there are changed files, determine the Amplience hub to update based on the commit/pull request type and destination branch. The hub to update is set to one of three environments: Amplience B2C Sandbox, Amplience B2C QA2 (Staging), or Amplience B2C DEV2 (Prod).

Install DC-CLI: Installs and configures the Amplience Dynamic Content CLI (dc-cli) tool using npm. The installation is performed only if there is a hub to update.

Process files: (Performed only if there is a hub to update.) Processes the changed files by exporting them locally and importing them into the specified Amplience hub. The files are exported to a local folder, and the dc-cli tool is used to configure and import the files into the hub. For each schema ID, it searches for a matching content type based on the schema ID.

If a matching content type is found, its ID is extracted and used to sync the content type with the updated schema.

Uploads dc-cli log files as an artifact (only if there are changed files).

=============================

Hub Mappings:

| Source                     | Destination  | PR type      | Hub to update     |
|----------------------------|--------------|--------------|-------------------|
| Feature or fix branch XYZ  | Develop      | Opened       | B2C Sandbox       |
| Feature or fix branch XYZ  | Develop      | Merged       | B2C QA2 (Staging) |
| Any                        | Main         | Commit/Merge | B2C DEV2 (Prod)   |

=============================

Secrets:

API_ACCESS_TOKEN

AMPLIENCE_SECRET_SANDBOX

AMPLIENCE_SECRET_QA2

AMPLIENCE_SECRET_DEV2

Create Github Personal Access Token - To access the Git API
- Save token value in secrets.API_ACCESS_TOKEN

Variables:

AMPLIENCE_CLIENT_ID_SANDBOX

AMPLIENCE_CLIENT_ID_QA2

AMPLIENCE_CLIENT_ID_DEV2

AMPLIENCE_HUB_ID_SANDBOX

AMPLIENCE_HUB_ID_QA2

AMPLIENCE_HUB_ID_DEV2
