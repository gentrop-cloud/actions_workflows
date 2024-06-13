## Setup
```
export PROJECT_ID=<YOUR_PROJECT_ID>
export PROJECT_NUMBER=<YOUR_PROJECT_NUMBER>
export REPOSITORY_PATH=<YOUR_REPOSITORY_PATH>
export POOL_NAME=<POOL_NAME>
export PROVIDER_NAME=<PROVIDER_NAME>
export SA_NAME=<YOUR_SA_NAME>
export SA_EMAIL=$SA_NAME@$PROJECT_ID.iam.gserviceaccount.com
```

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
