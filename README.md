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
Follow the next section to establish BOSH cli connectivity

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

Now test BOSH cli connectivity with `bosh env`

## Open The Communications Channels (GCP)

This section is only required if your jumpbox is outside the BOSH director's network

```bash
gcloud compute networks peerings create default-to-${PCF_SUBDOMAIN_NAME}-pcf-network \
  --network=default \
  --peer-network=${PCF_SUBDOMAIN_NAME}-pcf-network \
  --auto-create-routes

gcloud compute networks peerings create ${PCF_SUBDOMAIN_NAME}-pcf-network-to-default \
  --network=${PCF_SUBDOMAIN_NAME}-pcf-network \
  --peer-network=default \
  --auto-create-routes
  
gcloud compute --project=${PCF_PROJECT_ID} firewall-rules create bosh \
 --direction=INGRESS \
 --priority=1000 \
 --network=${PCF_SUBDOMAIN_NAME}-pcf-network \
 --action=ALLOW \
 --rules=tcp:25555,tcp:8443,tcp:22 \
 --source-ranges=0.0.0.0/0
```

## Non-Destructive Commands

- `bosh --help`
- `bosh env`
- `bosh tasks`
- `bosh tasks --recent=20`
- `bosh tasks --recent=30 --all`
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

## SSH from jumpbox to BOSH director VM (GCP)

This technique uses `bbr_ssh_credentials`

```bash
KEYFILE=$(mktemp)
om \
  --skip-ssl-validation \
  --target ${PCF_OPSMAN_FQDN} \
  --username admin \
  --password ${PCF_OPSMAN_ADMIN_PASSWD} \
  curl \
    --silent \
    --path /api/v0/deployed/director/credentials/bbr_ssh_credentials | \
      jq --raw-output '.credential.value.private_key_pem' > ${KEYFILE}
ssh -i ${KEYFILE} bbr@10.0.0.10
```

When sifting thorugh JSON on BOSH VMs, consider `python -m json.tool` as a replacement for `jq`
