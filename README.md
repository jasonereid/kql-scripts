# kql-scripts

## Find all Azure storage accounts with public accessible set to TRUE, add TLS version also
## Run this from the Azure Resource Graph Explorer

    resources
    | where type == "microsoft.storage/storageaccounts"
    | project
    name,
    properties.allowBlobPublicAccess,
    publicAccess= properties['publicNetworkAccess'] ,
    properties.minimumTlsVersion

