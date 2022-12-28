# kql-scripts

## Find all NSGs with 3389 and 22 open inbound

    resources 
    | where type startswith 'microsoft.network/networksecuritygroups' 
    | mv-expand rules=properties.securityRules
    | where rules.properties.destinationPortRange in ("3389", "22")
    | project 
        name    = rules.name, 
        access  = rules.properties.access, 
        rule    = rules.properties.destinationPortRange, 
        nsgname = name, 
        group   = resourceGroup

## Find all Azure storage accounts with public accessible set to TRUE, add TLS version also
### Run this from the Azure Resource Graph Explorer

    resources
    | where type == "microsoft.storage/storageaccounts"
    | project
    name,
    properties.allowBlobPublicAccess,
    publicAccess= properties['publicNetworkAccess'] ,
    properties.minimumTlsVersion

## Get failed requests from an Application Gateway (http requests > 399)
### Strip last two lines for detailed logs per request

        AzureDiagnostics
        | where ResourceType == "APPLICATIONGATEWAYS" and OperationName == "ApplicationGatewayAccess" and httpStatus_d > 399
        | summarize AggregatedValue = count() by bin(TimeGenerated, 1h), _ResourceId
        | render timechart
        
        
        
