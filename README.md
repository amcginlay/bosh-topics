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
- `bosh deployments`
- `bosh instances --ps`
- `bosh cloud-config`
- `bosh manifest` <- download cf manifest and collapse YAML sections using a good text editor
- `bosh errands`
- `bosh ssh`
- `bosh logs`
- `bosh variables`

## Local File Copy Syntax

- `gcloud compute scp ubuntu@jumpbox:~/workspace/pcf-operator-course/${CF_DEPLOYMENT}.yml .`
