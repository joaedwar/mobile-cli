## Service Configuration

### Overview
The Service Config stores information about a mobile service and is backed by a secret. This information is then used to populate your mobile client's config.

This information could be anything but often is made up of values such as the URL of the service and perhaps some headers and configuration particular to that service.

### Retrieving Configurations
All service configs for a particular namespace can be retrieved with the following command:
```sh
mobile get clientconfig example_client_id --namespace=myproject
```

Which will produce output like the following:
```sh
+----------------+-------------------+----------------+-------------------------------------------------------+
|       ID       |      NAME         |      TYPE      |                          URL                          |
+----------------+-------------------+----------------+-------------------------------------------------------+
| Client ID      | example_client_id |                |                                                       |
| fh-sync-server | fh-sync-server    | fh-sync-server | https://fh-sync-server-myproject.192.168.64.74.nip.io |
| keycloak       | keycloak          | keycloak       | https://keycloak-myproject.192.168.64.74.nip.io       |
| prometheus     | prometheus        | prometheus     | https://prometheus-myproject.192.168.64.74.nip.io     |
+----------------+-------------------+----------------+-------------------------------------------------------+
```

To use the configuration with one of our SDKs, you can get a full JSON formatted version with the following command:
```sh
mobile get clientconfig example_client_id --namespace=myproject -o json
```

Which will produce output similar to the following (newlines and indentation have been added for readability):
```
{
  "version": 1,
  "clusterName": "https://192.168.64.86:8443",
  "namespace": "config",
  "clientId": "myapp-android",
  "services": [
    {
      "id": "keycloak-myapp-android-public",
      "name": "keycloak",
      "type": "keycloak",
      "url": "https://keycloak-config.192.168.64.86.nip.io/auth",
      "config": {
        "auth-server-url": "https://keycloak-config.192.168.64.86.nip.io/auth",
        "confidential-port": 0,
        "public-client": true,
        "realm": "config",
        "resource": "myapp-android-public",
        "ssl-required": "external"
      }
    },
    {
      "id": "metrics-myapp-android",
      "name": "metrics",
      "type": "metrics",
      "url": "https://aerogear-app-metrics-config.192.168.64.86.nip.io/metrics",
      "config": {}
    }
  ]
}
```

### Understanding the JSON format
Firstly, the parent object in the JSON output is described below:

#### version
The version of the JSON structure used in this response. If in the future the configuration changes in a non compatible way this version number would become 2 for example.

#### clusterName
An identifier of the cluster this config was retrieved from.

#### namespace
The namespace these configs were retrieved from.

#### clientId
The client id of the mobile application using these configs.

#### services
An array of configuration values for the provisioned mobile services in this namespace.

### The service object
The service object contains specific configuration values for each mobile service provisioned to a specific namespace.

Each service has a common set of values:
#### id
A unique identifier of this specific deployment of this service.

#### name
A human-readable identifier of this service.

#### type
A way of categorising services, e.g. Authentication, Storage, etc...

#### url
The canonical URL that this service can be reach at

#### config
The config is a loosely defined object where any extra details specific to a particular service that may be required to make proper use of this service will be stored.

For example, in the KeyCloak service in the snippet above, the config contains the name of the realm, clientID and other details that would be required to make use of KeyCloak from a client.
