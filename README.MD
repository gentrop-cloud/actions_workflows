![logo](https://i.imgur.com/0ubZWC7.png)

# Dev Workflows 
This repository contains all our worflows, and allows them to be reused in projects

## Checkout our available Workflows:



### Lint 
This workflow help us to detect Linting issues with annotations in the PR, and it fixes other issues auto. \
Docs [here](./docs/lint.md).


### Test 
To help to run our suit tests, and writing the workflow only once, we could use this one. 
It should be extensible, accepting parameters like `use_emulators` and specify which script should be run. 

If the test scenario is too different, we can always create a new one from the present configuration and specify it on our project.


### Deploy

Ref: https://cloud.google.com/blog/products/identity-security/enabling-keyless-authentication-from-github-actions
GCP Training Video: https://www.youtube.com/watch?v=ZgVhU5qvK1M

## Setup

export PROJECT_ID=<YOUR_PROJECT_ID>
export PROJECT_NUMBER=<YOUR_PROJECT_NUMBER>
export REPOSITORY_PATH=<YOUR_REPOSITORY_PATH>
export POOL_NAME=<POOL_NAME>
export PROVIDER_NAME=<PROVIDER_NAME>
export SA_NAME=<YOUR_SA_NAME>
export SA_EMAIL=$SA_NAME@$PROJECT_ID.iam.gserviceaccount.com

gcloud iam workload-identity-pools create $POOL_NAME --location="global" --project $PROJECT_ID

gcloud iam workload-identity-pools providers create-oidc $PROVIDER_NAME \
--location="global" --workload-identity-pool="${POOL_NAME}"  \
--issuer-uri="https://token.actions.githubusercontent.com" \
--attribute-mapping="attribute.actor=assertion.actor,google.subject=assertion.sub,attribute.repository=assertion.repository" \
--project $PROJECT_ID

gcloud iam service-accounts create $SA_NAME \
--display-name="Service account used by Deployer action" \
--project $PROJECT_ID


gcloud projects add-iam-policy-binding $PROJECT_ID \
--member="serviceAccount:${SA_EMAIL}" \
--role="roles/editor"



gcloud iam service-accounts add-iam-policy-binding "${SA_EMAIL}" \
  --project="${PROJECT_ID}" \
  --role="roles/iam.workloadIdentityUser" \
  --member="principalSet://iam.googleapis.com/projects/${PROJECT_NUMBER}/locations/global/workloadIdentityPools/${POOL_NAME}/attribute.repository/${REPOSITORY_PATH}"
  
  
  