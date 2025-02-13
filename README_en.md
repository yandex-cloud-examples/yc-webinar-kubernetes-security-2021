# Hands-on Kubernetes webinar setup

The setup video will be available when published on YouTube.
The setup enables you to independently configure everything shown in the webinar, in particular:

1. Role-based management model for different container environments.
2. Pod deployment policies in the created cluster.


## Prerequisites

- Bash
- Terraform
- jq
- [CLI](https://cloud.yandex.ru/docs/cli/operations/install-cli) initiated in the default profile of your user (either `admin` or `editor` at the cloud level)
- Two test folders (you will need their IDs in the next steps)
- helm v3

## Preparing the environment

The setup will include two folders and two users: `devops` and `developer`. 


Write down the IDs of the folders for our job:

```
export STAGING_FOLDER_ID=<staging_demo_folder_ID>
export PROD_FOLDER_ID=<prod_demo_folder_ID>
```

Create service accounts that will emulate users:

```
$ yc iam service-account create --name devops-user1 --folder-id=$STAGING_FOLDER_ID
$ yc iam service-account create --name developer-user1 --folder-id=$STAGING_FOLDER_ID
```
Create two CLI profiles: one will emulate `devops` user, the other, `developer`:
```
$ yc iam key create --service-account-name devops-user1 --folder-id=$STAGING_FOLDER_ID --output devops.json
$ yc iam key create --service-account-name developer-user1 --folder-id=$STAGING_FOLDER_ID --output developer.json

$ yc config profile create demo-devops-user1
$ yc config set service-account-key devops.json

$ yc config profile create demo-developer-user1
$ yc config set service-account-key developer.json
```
Check that no one has any roles in the folders for the job:
```
$yc resource-manager folder list-access-bindings --id=$STAGING_FOLDER_ID --profile=default

+---------+--------------+------------+
| ROLE ID | SUBJECT TYPE | SUBJECT ID |
+---------+--------------+------------+
+---------+--------------+------------+

$ yc resource-manager folder list-access-bindings --id=$PROD_FOLDER_ID --profile=default

+---------+--------------+------------+
| ROLE ID | SUBJECT TYPE | SUBJECT ID |
+---------+--------------+------------+
+---------+--------------+------------+
```

Let’s proceed to the lab.

#### Part one: Configuring role-based access 

```
$ cd ./terraform/iam
```

Check out the readme file for [this section](./terraform/iam/).

#### Part two: Configuring policies

(It requires you to complete _Part 1_ or use a previously created Kubernetes cluster.)

```
$ cd ./kubernetes/
```

Check out the readme file for [this section](./kubernetes/).

#### Part three: Deleting the setup

```
$ cd ./end
```

Check out the readme file for [this section](./end/).

