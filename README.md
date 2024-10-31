# Example : Serverless Search Portal with GitHub Action Ingest

This repository is an example of the [@globus/template-search-portal](https://github.com/globus/template-search-portal)

You can create your own portal with similar functionality by following the [**Creating Your Own Research Search Portal**](https://github.com/globus/template-search-portal?tab=readme-ov-file#creating-your-own-static-research-search-portal) section in the template repository and then referencing the sections below.


## Pre-Requisites

- A [Globus Search Index](https://docs.globus.org/api/search/) you administer.
- A GitHub repository you administer, created following [**Creating Your Own Research Search Portal**](https://github.com/globus/template-search-portal?tab=readme-ov-file#creating-your-own-static-research-search-portal).


## Enabling GitHub Action-based Ingest

1. Create a new Globus Service Account using the [Developers](https://app.globus.org/settings/developers) section in the Globus Web App.

2. Update your search index to include a `writer` role for the new service account.

```bash
# Example using the Globus CLI
globus search index role create "<INDEX_ID>" writer "<SERVICE_ACCOUNT>"
# globus search index role create "0d22571f-4126-4fa0-8796-be78617c366c" writer "51ccd7c2-4740-4033-8481-8579fb99f63a@clients.auth.globus.org"
```

3. Add the following [**secrets** to your repositories GitHub Actions secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions):
    - `GLOBUS_CLIENT_ID` - The client ID of the service account.
    - `GLOBUS_CLIENT_SECRET` - A client secret of the service account.


4. Add the GitHub Action workflow to your repository by copying the contents of the [`.github/workflows/ingest.yml`](.github/workflows/ingest.yml) file in this repository to your repository at `.github/workflows/ingest.yml`.
    - The workflow file in this repository is annotated with comments to help you understand how it works.

5. Add a file in your repository at `/data/ingest.json` matching one of the supported [Globus Search request schemas](https://docs.globus.org/api/search/reference/ingest/#request_schemas).
    - Most commonly, you will want this file to represent an `ingest_type: "GMetaList"`, allowing the repository to act as a management system for the records in your search index.