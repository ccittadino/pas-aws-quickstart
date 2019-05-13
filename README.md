# pks-gcp-quickstart

This repo contains a concourse pipeline and tasks to automatically deploy PKS on GCP, including paving the environment using Terraform. This is meant for POCs and getting a minimal platform up quickly in a self contained way. this should not be used for production.
It is using [terraforming-gcp](https://github.com/pivotal-cf/terraforming-gcp) and [Platform Automation](http://docs-platform-automation.cfapps.io/platform-automation/v2.0/index.html) to do so.

# Features

* the pipeline can be deployed multiple times with different values for `env_name`
  * for each pipeline there will be a dedicated subdomain created in gcp: `env_name.dns_suffix`
* letsencrypt certificates are generated for PKS and Ops Manager and Harbor.

# Reqirements

* GCP account
* Pivotal Network account
* Private Git Repository
* three private GCS Buckets
* concourse
* a (sub-)domain hosted on GCP

# Credentials

To keep it simple and easy deployable on any concourse installation, the pipeline currently gets most of its credentials and customization fron a `credentials.yml` file.
Copy the `credentials-template.yml` file to `credentials.yml` and modify the appropriate items.

# Deploy Pipline

```
fly login -t env -c https://concourse.domain.com -n team
fly -t env set-pipeline -p pcf-platform-automation -c pipeline.yml -l credentials.yml --verbose
fly -t env unpause-pipeline -p pcf-platform-automation
```