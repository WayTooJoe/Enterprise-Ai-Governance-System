# Neural Risk Management 
The system bridges static compliance frameworks with live engineering operations, providing real-time visibility into AI models, datasets, and third-party vendors. It automates telemetry ingestion, schema validation, and evidence logging into an immutable vault, replacing manual processes.

          "id": "AOtLhuBZLSWkwE2T",
          "name": "Airtable Personal Access Token account"
        }
      }
    },
    {
      "parameters": {
        "text": "=={{ JSON.stringify($json.body) }}",
        "attributes": {
          "attributes": [
            {
              "name": "Incoming_Asset_ID",
              "description": "Extract the asset tag or model ID (e.g., \"AI-001\"). Look for 'model_id' or 'asset_tag'. Default to 'UNKNOWN_ASSET' if missing."
            },
            {
              "name": "Incoming_NIST_Control",
              "description": "Locate the compliance framework control code (e.g., \"MEAS-3.1\"). Look for 'target_control' or 'mapped_nist_control'. Default to 'UNKNOWN_CONTROL' if missing."
            },
            {
              "name": "Business_Unit",
              "description": "Infer the department based on keywords. If 'registry_uri' or 'model_id' contains 'customer-insights', 'alpha-trader', or 'ledger', output 'Finance'. If it contains 'contract' or 'regulatory', output 'Legal'. Otherwise, default to 'Corporate'."
            },
            {
              "name": "Deployment_Context_Key",
              "description": "Generate a composite string structured exactly as: Incoming_Asset_ID - Incoming_NIST_Control (Business_Unit)."
            },
            {
              "name": "Scan_Type",
              "description": "Determine the type of assessment run (e.g., \"Automated Bias Scan\") or default to \"Unknown\"."
            },
            {
              "name": "Scan_Status",
              "description": "Extract the execution status or default to \"Ingested\"."
            },
            {
              "name": "Raw_JSON_Payload",
              "description": "Capture the entire original, untouched input JSON object exactly as it was received and format it as a single string block."
            }
          ]
        },
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.informationExtractor",
      "typeVersion": 1.2,
      "position": [
        208,
        0
      ],
      "id": "b4c0781e-e8d0-47cb-b737-2c003d79f699",
      "name": "Information Extractor"
    },
    {
      "parameters": {
        "model": {
          "__rl": true,
          "mode": "list",
          "value": "gpt-5-mini"
        },
        "builtInTools": {},
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1.3,
      "position": [
        144,
        208
      ],
      "id": "f3645845-2894-4e90-a389-742d29f1996f",
      "name": "OpenAI Chat Model",
      "credentials": {
        "openAiApi": {
          "id": "HHYxDzypiuByLIt1",
          "name": "n8n free OpenAI API credits"
        }
      }
    }
