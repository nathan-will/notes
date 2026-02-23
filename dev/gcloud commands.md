```

gcloud projects get-iam-policy cegx-nextflow \ --flatten="bindings[].members" \ --format='table(bindings.role)' \ --filter="bindings.members:cegx-pypi-access@cegx-nextflow.iam.gserviceaccount.com"


gcloud projects get-iam-policy cegx-releases \ --flatten="bindings[].members" \ --format='table(bindings.role)' \ --filter="bindings.members:artifact-reg@cegx-releases.iam.gserviceaccount.com"

gcloud iam service-accounts add-iam-policy-binding $SA_ACCOUNT \
--project=$PROJECT_ID \
--role=roles/iam.workloadIdentityUser \ 
--member=principalSet://iam.googleapis.com/projects/$PROJECT_NUMBER/locations/global/workloadIdentityPools/github/attribute.repository/cegx-ds/$REPO

gcloud iam service-accounts add-iam-policy-binding $SA_ACCOUNT \
--project=$PROJECT_ID \
--role=roles/iam.workloadIdentityUser \
--member=principalSet://iam.googleapis.com/projects/$PROJECT_NUMBER/locations/global/workloadIdentityPools/github/attribute.repository/cegx-ds/$REPO

gcloud projects get-iam-policy $PROJECT_ID \
--flatten="bindings[].members" \
--format='table(bindings.role)' \
--filter="bindings.members:$SA_ACCOUNT" 

REPO=coda
PROJECT=cegx-nextflow
PROJECT_ID=cegx-nextflow
SA_ACCOUNT=cegx-pypi-access@cegx-nextflow.iam.gserviceaccount.com PROJECT_NUMBER=432023135736
POOL=github cegx-cli-terraform@cegx-cli-terraform.iam.gserviceaccount.com

`gcloud iam workload-identity-pools describe $POOL \
--project="${PROJECT_ID}" \
--location="global" \
--format="value(name)"`
```

