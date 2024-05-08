# SAP BTP Tenant Upgrade GitHub Action

This GitHub Action upgrades tenant databases in SAP Business Technology Platform (SAP BTP) to the latest version. It's designed to handle multiple tenants and complex upgrade options.

## Features

- Upgrades multiple tenants simultaneously.
- Customizable upgrade options for flexibility.
- Integrated authentication with XSUAA for secure API access.
- Uses HTTP requests to interact with the MTX and XSUAA services.

## Inputs

Below are the inputs that you can configure for this action:

- `tenants`: **Required**. JSON array of tenant identifiers to upgrade. Example: `["tenant1", "tenant2"]`.
- `options`: JSON object describing upgrade options. Defaults to enabling trace and auto-undeploy for HDI deployments.
- `xsuaaClientId`: **Required**. Client ID for XSUAA authentication.
- `xsuaaClientSecret`: **Required**. Client Secret for XSUAA authentication.
- `xsuaaUrl`: **Required**. URL to XSUAA service for obtaining tokens.
- `mtxUrl`: **Required**. URL to the MTX service for upgrading tenants.
- `timeout`: Timeout for the HTTP request in milliseconds. Default is `600000` (10 minutes).

## Usage

To use this action, add the following steps to your `.github/workflows` YAML file:

```yaml
steps:
# -- Previous steps --

- name: Upgrade Tenant Databases
  uses: codeyogi911/btptenantupgrade@v1
  with:
    tenants: '["tenant1", "tenant2"]'
    options: '{ "_": { "hdi": { "deploy": { "trace": true, "auto_undeploy": true } } } }'
    xsuaaClientId: ${{ secrets.XSUAA_CLIENT_ID }}
    xsuaaClientSecret: ${{ secrets.XSUAA_CLIENT_SECRET }}
    xsuaaUrl: 'https://example-xsuaa-service/oauth/token'
    mtxUrl: 'https://example-mtx-service/-/cds/saas-provisioning/upgrade'
    timeout: 600000

# -- Next steps --
