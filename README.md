# bosh-topics

## Credentials

- `export BOSH_CLIENT=ops_manager`
- `export BOSH_CLIENT_SECRET=XXXXXXXXXXXXXX`
- `export BOSH_CA_CERT=/var/tempest/workspaces/default/root_ca_certificate`
- `export BOSH_ENVIRONMENT=10.0.0.10`

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

- `gcloud compute scp ubuntu@jumpbox:~/workspace/pcf-operator-course/gcp_credentials.json .`
