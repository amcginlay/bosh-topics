# bosh-topics

## Credentials

- `export BOSH_CLIENT=ops_manager`
- `export BOSH_CLIENT_SECRET=XXXXXXXXXXXXXX`
- `export BOSH_CA_CERT=/var/tempest/workspaces/default/root_ca_certificate`
- `export BOSH_ENVIRONMENT=10.0.0.10`

## Non-Destructive Commands

- `bosh --help`
- `bosh env`
- `bosh tasks --recent=10`
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
