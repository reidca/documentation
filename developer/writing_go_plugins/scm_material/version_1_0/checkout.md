## Message: Checkout SCM
 
### Request - From the server

***Request name***: `checkout`

***Request parameters***: empty

***Request headers***: empty

***Request body***: This contains information about the SCM configuration provided by the user. The keys in the maps correspond to the keys provided by the plugin, as a part of the response to ["SCM Configuration"](scm_configuration.md) message.

***Example request***:

```json
{
    "scm-configuration": {
        "SCM_URL": {
            "value": "http://localhost.com"
        },
        "USERNAME": {
            "value": "user"
        },
        "PASSWORD": {
            "value": "password"
        }
    },
    "destination-folder": "/var/lib/go-agent/pipelines/pipeline-name/destination",
    "revision": {
        "revision": "revision-1",
        "timestamp": "2011-07-14T19:43:37.100Z",
        "data": {
          "dataKeyOne": "data-value-one",
          "dataKeyTwo": "data-value-two"
        }
    }   
}
```

### Response - From the plugin

***Expected response body***: The plugin is expected to send a response, which contains a status ("success" or "failure"), and a list of error messages. This represents whether a checkout was successfully made, to the SCM specified in the request.

***Example response***:

```json
{
    "status": "success",
    "messages": [
        "Successfully checked out to SCM revision provided"
    ]
}
```

### Schema information

***[JSON schema](http://json-schema.org) of request from the server***:

```json
{
    "title": "Checkout SCM request schema",
    "description": "Schema for checkout SCM connection request Json",
    "type": "object",
    "properties": {
        "scm-configuration": {
             "type": "object",
             "patternProperties": {
                 "^[a-zA-Z0-9_-]+$": {
                     "type": [
                         "object",
                         "null"
                     ],
                     "properties": {
                         "value": {
                             "type": "string",
                             "pattern": "^[a-zA-Z0-9_-]+$"
                         }
                     },
                     "additionalProperties": false
                 }
             },
             "additionalProperties": false
         },
         "destination-folder" : {
            "type": "string",
            "required": true
        },
        "revision": {
            "type": "object",
            "properties": {
                "revision": {
                    "type": "string"
                },
                "timestamp": {
                    "type": "string"
                },
                "data": {
                    "type": "object",
                    "patternProperties": {
                        "^[a-zA-Z0-9_-]+$": {
                            "type": "string"
                        },
                        "additionalProperties": false
                    }
                }
            }
        },
        "additionalProperties": false
    }
}
```

***[JSON schema](http://json-schema.org) of expected response***:

```json
{
    "title": "Checkout SCM connection response schema",
    "description": "Schema for checkout SCM connection response Json",
    "type": "object",
    "required": true,
    "properties": {
        "messages": {
            "required": false,
            "type": "array",
            "items": {
                "type": "string",
                "required": false
            },
            "minItems": 0,
            "uniqueItems": true
        },
        "status": {
            "type": "string",
            "required": true
        }
    }
}
```
