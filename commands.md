
### git

#### login + clone
```bash
git config --global credential.helper store



```

### Docker
```bash
docker system prune -a --volumes
```


### BASH

When to use

```bash
if

while

function

array

```

sed

```bash
# sed to replace : with =
cat my_file.txt | sed 's/: /=/' | my_new_file.txt
cat config.yaml | sed 1d | sed 's/: /=/' | sed 's/^/$/' >| vars.txt
example:
###
---
region: europe-west2
###
End:
###
$region=europe-west2
### 
```

find and replace text with sed: [https://stackoverflow.com/questions/5410757/how-to-delete-from-a-text-file-all-lines-that-contain-a-specific-string](https://stackoverflow.com/questions/5410757/how-to-delete-from-a-text-file-all-lines-that-contain-a-specific-string)

awk

#### process tree:

```bash
ps -aef --forest

```

#### check if directory contains folder

```bash
if [[ $(find ${dir} -type f  | wc -l) -gt 0 ]]; then echo "ok"; fi
-gt = greater than
essentially if word count of directory is greater than 0
```

Standard bash confirm input:

```bash
echo "You entered" "'$SA_DESCRIPTION'"
read -p "Continue? (Y/N): " confirm && [[ $confirm == [yY] || $confirm == [yY][eE][sS] ]] || exit 1
```

Remove file with funny characters

```bash
rm -i -- *
```

#### FIND

#### Text manipulation in bash
##### adding a line between 2 existing lines

Test this: https://stackoverflow.com/questions/13696674/using-awk-or-send-to-find-a-string-and-insert-another-complex-string-before-it

```
# grep for the string and find the line, when you have the line cat the string


```

### tmux

```bash
ctr + b + [ #scroll

q # to stop scrolling

tmux new -s new-window
```

### Docker

```bash
docker logs --follow <ID>

# running images
docker ps

# all images list
docker images ls

#run images interactively - from a non-running container
docker run -it --entrypoint /bin/bash $image_id

# poke around a running container
docker exec -it $image_id sh

# docker pull, tag and push
docker pull {repository}:{tag}

docker tag {image_id} {new_repo}:{tag}

docker push {new_repo}:{tag}
```

### SLURM

```bash
squeue # jobs in the queue

sinfo # info on node states

sudo scontrol update nodename=$nodename state=resume
```

### GIT

```bash
git diff SS-2263/optimise-local-profile-in-nextflow-config 

# save credentials
git config --global credential.helper store

git config --global user.email "nathan@cegx.co.uk"
git config --global user.name "Nathan Williams"
```

## GitHub

```bash
# How to get the API # of workflow to curl

```

### Python

find and replace text with python:

editing google buckets:

## Gcloud

```bash
gcloud projects get-iam-policy cegx-nextflow  \\
--flatten="bindings[].members" \\
--format='table(bindings.role)' \\
--filter="bindings.members:cegx-pypi-access@cegx-nextflow.iam.gserviceaccount.com"

gcloud projects get-iam-policy cegx-releases  \\
--flatten="bindings[].members" \\
--format='table(bindings.role)' \\
--filter="bindings.members:artifact-reg@cegx-releases.iam.gserviceaccount.com"
```

```bash

gcloud iam service-accounts add-iam-policy-binding cegx-docker-registry-access@cegx-nextflow.iam.gserviceaccount.com \
  --project=cegx-nextflow \
 --role=roles/iam.workloadIdentityUser \
 --member=principalSet://iam.googleapis.com/projects/432023135736/locations/global/workloadIdentityPools/github/attribute.repository/cegx-ds/modality

gcloud iam service-accounts create $SA_ACCOUNT \\
 --display-name=$SA_ACCOUNT \\
--project=$PROJECT

gcloud iam service-accounts add-iam-policy-binding cegx-docker-registry-access@cegx-nextflow.iam.gserviceaccount.com \\
--member=serviceAccount:cegx-docker-registry-access@cegx-nextflow.iam.gserviceaccount.com \\
--role='roles/editor'

gcloud projects get-iam-policy $PROJECT_ID  \\
--flatten="bindings[].members" \\
--format='table(bindings.role)' \\
--filter="bindings.members:$SA_ACCOUNT"

REPO=prelude
PROJECT=cegx-nextflow
PROJECT_ID=cegx-nextflow
SA_ACCOUNT=cegx-docker-registry-access@cegx-nextflow.iam.gserviceaccount.com
PROJECT_NUMBER=432023135736
POOL=github

cegx-cli-terraform@cegx-cli-terraform.iam.gserviceaccount.com
REPO=biomodal-cli
PROJECT=cegx-cli-terraform
SA_ACCOUNT=cegx-cli-terraform
PROJECT_NUMBER=687848260311

gcloud iam workload-identity-pools describe $POOL \\
  --project="${PROJECT_ID}" \\
  --location="global" \\
  --format="value(name)"

```

### placement policy
https://cloud.google.com/compute/docs/instances/manage-placement-policies#:~:text=You%20can%20delete%20a%20placement%20policy%20using%20the%20gcloud%20CLI%20and%20REST.&text=To%20delete%20a%20placement%20policy%2C%20use%20the,compute%20resource%2Dpolicies%20delete%20command.&text=%2D%2Dregion%3D%20REGION-,Replace%20the%20following%3A,of%20an%20existing%20placement%20policy.
```

```


## Web

### nginx - CSP - uses $host for docker container

```bash
# in the nginx
add_header Content-Security-Policy "default-src 'self' 'unsafe-inline' 'unsafe-eval' https://*.$host <https://www.google.com> <https://fonts.googleapis.com> <https://fonts.gstatic.com> <https://www.gstatic.com>; img-src 'self' https://*.$host https://*.gravatar.com https://*.githubusercontent.com blob: data:; worker-src blob:; child-src 'self' <https://www.google.com> https://*.$host blob:" always;

# When curled this translates to:
curl -sD - <https://testing.cegx.co.uk/> -o /dev/null | grep Content-Security-Policy
Content-Security-Policy: default-src 'self' 'unsafe-inline' 'unsafe-eval' https://*.testing.cegx.co.uk <https://www.google.com> <https://fonts.googleapis.com> <https://fonts.gstatic.com> <https://www.gstatic.com>; img-src 'self' https://*.testing.cegx.co.uk https://*.gravatar.com https://*.githubusercontent.com blob: data:; worker-src blob:; child-src 'self' <https://www.google.com> https://*.testing.cegx.co.uk blob:
```

### NEXTFLOW

```bash
NXF_VER=22.11.1-edge
NXF_TRACE=nextflow
NXF_DEBUG=3

```

Folder paths quick explainer:

```bash
# A top-level path in which input files should be organised and to which output files will be written
data_path : gs://my-bucket
# this assume a directory called nf-input and will create one called nf-results, so:
data_path : gs://my-bucket = input_path : gs://my-bucket/nf-input + output_path : gs://my-bucket/nf-results + meta_file : gs://gs://my-bucket/CEGX_Runmeta.csv
# and all parts of the data_paths can be used as independent params, if you couldn't move files around

```

### Debian/UNIX

```bash
# add user
sudo adduser [newuser]
# list all users
getent passwd
# list all local users
cut -d: -f1 /etc/passwd
```

### Kubernetes (GKE)
```
https://github.com/seqeralabs/nf-tower-k8s/blob/master/cluster-preparation.md https://help.tower.nf/21.10/compute-envs/gke/#overview https://help.tower.nf/23.1/enterprise/configuration/compute_environments/ https://help.tower.nf/23.1/enterprise/kubernetes/#configure-container-registry-credentials