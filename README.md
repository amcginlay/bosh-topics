# bosh-topics

## Export BOSH credentials

```bash
export $( \
  om \
    --skip-ssl-validation \
    --target ${PCF_OPSMAN_FQDN} \
    --username admin \
    --password ${PCF_OPSMAN_ADMIN_PASSWD} \
    curl \
      --silent \
      --path /api/v0/deployed/director/credentials/bosh_commandline_credentials | \
        jq --raw-output '.credential' \
)
```

Use `env | grep BOSH` to inspect the result of the above.

## Copy Root CA Certificate

```bash
sudo mkdir -p /var/tempest/workspaces/default

sudo sh -c \
  "om \
    --skip-ssl-validation \
    --target ${PCF_OPSMAN_FQDN} \
    --username admin \
    --password ${PCF_OPSMAN_ADMIN_PASSWD} \
    curl \
      --silent \
      --path "/api/v0/security/root_ca_certificate" |
        jq --raw-output '.root_ca_certificate_pem' \
          > /var/tempest/workspaces/default/root_ca_certificate"
```

## Non-Destructive Commands

- `bosh --help`
- `bosh env`
- `bosh tasks --recent=30`
- `bosh task --debug`
- `bosh stemcells`
- `bosh releases`
- `bosh inspect-release ${RELEASE}/${VERSION}`
- `bosh deployments`
- `bosh cloud-config`
- `bosh -d ${DEPLOYMENT_NAME} instances --ps`
- `bosh -d ${DEPLOYMENT_NAME} manifest > ${DEPLOYMENT_NAME}.yml`
- `bosh -d ${DEPLOYMENT_NAME} errands`
- `bosh -d ${DEPLOYMENT_NAME} ssh ${INSTANCE_TYPE}`
- `bosh -d ${DEPLOYMENT_NAME} logs`
- `bosh -d ${DEPLOYMENT_NAME} variables`

## Downloading BOSH manifests

After running this command on your jumpbox ...

- `bosh -d ${DEPLOYMENT_NAME} manifest > ${DEPLOYMENT_NAME}.yml`

... run this command from your local machine:

- `gcloud compute scp ubuntu@jumpbox:~/workspace/pcf-operator-course/${DEPLOYMENT_NAME}.yml .`

## GCP SSH to the BOSH director VM

- `gcloud compute ssh "vcap@${BOSH_DIRECTOR_VM_NAME}" --project "${PROJECT_ID}" --zone "${VM_ZONE}"`
