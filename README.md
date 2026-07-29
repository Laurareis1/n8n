{
  "name": "My workflow 2",
  "nodes": [
    {
      "parameters": {
        "httpMethod": "POST",
        "path": "loom-house-order",
        "options": {}
      },
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2.1,
      "position": [
        -752,
        -64
      ],
      "id": "9f8ee696-bb5c-4548-9d0c-b2b13c18049f",
      "name": "Webhook",
      "webhookId": "977e5707-6ec6-4e10-98fc-45b73f37f9d7"
    },
    {
      "parameters": {
        "fieldToSplitOut": "body.items",
        "include": "allOtherFields",
        "options": {}
      },
      "type": "n8n-nodes-base.splitOut",
      "typeVersion": 1,
      "position": [
        -544,
        -176
      ],
      "id": "d949539f-4597-46bf-82cb-45e8ed70fe6c",
      "name": "Split Out"
    },
    {
      "parameters": {
        "url": "=https://api.restcountries.com/countries/v5?q={{ $json.body.shippingCountry}}&pretty=1\\\n-H \"Authorization: Bearer rc_live_cceb3d8cfd3d48ac9ec9e056c198ed08\"",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "Authorization",
              "value": "rc_live_cceb3d8cfd3d48ac9ec9e056c198ed08"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.4,
      "position": [
        -560,
        48
      ],
      "id": "6ca703de-89e5-4041-ab5b-bf333f2814ca",
      "name": "HTTP Request",
      "retryOnFail": false,
      "onError": "continueRegularOutput"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "e16999fc-e402-40be-9b6f-9a9a21009e25",
              "leftValue": "={{ $json.data.objects[0].codes.alpha_2 }}",
              "rightValue": "PT",
              "operator": {
                "type": "string",
                "operation": "notEquals"
              }
            }
          ],
          "combinator": "or"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        -352,
        48
      ],
      "id": "51893289-d833-4bf5-8cdb-30ae7f0e6fcc",
      "name": "If"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "42dbf886-1a00-427c-aa70-a2c69e232daa",
              "name": "customsText",
              "value": "INTERNATIONAL - print customs form",
              "type": "string"
            },
            {
              "id": "28851c86-5f5e-4219-9a3b-c839011819c1",
              "name": "",
              "value": "",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        -144,
        -48
      ],
      "id": "8d63045b-079b-44ef-bf30-279fb7b2e89e",
      "name": "Edit Fields"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "5be6df4d-8762-4fd4-a01a-5f449001a3c3",
              "name": "customsText",
              "value": "DOMESTIC - no customs form needed",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        -128,
        128
      ],
      "id": "ad566fb0-2f8f-4a8b-971a-1dfb166a8350",
      "name": "Edit Fields1"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://discord.com/api/webhooks/1531286834259431464/ZZMdfjdW0TC9FpTboW1_Z7l2CD34UpnBXqotHonyw1AQIWFIWkDWRn1wkJp5hiuuuS44",
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            {
              "name": "content",
              "value": "={{ \"New order: \" + $('Webhook').item.json.body.orderId + \"\\nCustomer: \" + $('Webhook').item.json.body.customerName + \"\\nDestination: \" + $('Webhook').item.json.body.shippingCountry + \"\\n\" + $json.customsText + \"\\n\\nItems:\\n\" + $('Webhook').item.json.body.items.map(i => \"- \" + i.name + \" (x\" + i.quantity + \") - bin \" + i.binNumber).join(\"\\n\") }}"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.4,
      "position": [
        144,
        16
      ],
      "id": "1d215407-4416-447b-bacf-3f47763d909d",
      "name": "HTTP Request1"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "18b5oWk29tQqDPpNEylaqmZ2UWK8ijuvc5YkPtkTLLwk",
          "mode": "list",
          "cachedResultName": "teste",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/18b5oWk29tQqDPpNEylaqmZ2UWK8ijuvc5YkPtkTLLwk/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Folha1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/18b5oWk29tQqDPpNEylaqmZ2UWK8ijuvc5YkPtkTLLwk/edit#gid=0"
        },
        "columns": {
          "mappingMode": "autoMapInputData",
          "value": {
            "headers": "=orderId → {{ $json.body.orderId }}",
            "params": "=customerName → {{ $json.body.customerName }}",
            "query": "=sku → {{ $json.items.sku }}",
            "body": "="
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "headers",
              "displayName": "headers",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "params",
              "displayName": "params",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "query",
              "displayName": "query",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "body",
              "displayName": "body",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "webhookUrl",
              "displayName": "webhookUrl",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "executionMode",
              "displayName": "executionMode",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "body.items",
              "displayName": "body.items",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -336,
        -176
      ],
      "id": "c7c03a6e-ac9d-4ee7-9a96-748b6e765cdc",
      "name": "Append row in sheet",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "416RGIZpLFXdN5R4",
          "name": "Google Sheets account"
        }
      }
    }
  ],
  "pinData": {},
  "connections": {
    "Webhook": {
      "main": [
        [
          {
            "node": "Split Out",
            "type": "main",
            "index": 0
          },
          {
            "node": "HTTP Request",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Split Out": {
      "main": [
        [
          {
            "node": "Append row in sheet",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HTTP Request": {
      "main": [
        [
          {
            "node": "If",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If": {
      "main": [
        [
          {
            "node": "Edit Fields",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Edit Fields1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields": {
      "main": [
        [
          {
            "node": "HTTP Request1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields1": {
      "main": [
        [
          {
            "node": "HTTP Request1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate",
    "availableInMCP": false
  },
  "versionId": "4b889166-4393-4462-9540-614880fab4cc",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "889c3f7154be6c8e4001adc98a7c92b87e2353b48f3a4e53b83d38a5f932f13c"
  },
  "nodeGroups": [],
  "id": "XmXWyjGvRHxEAIoM",
  "tags": []
}
