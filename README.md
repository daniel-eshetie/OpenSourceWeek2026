# OpenSourceWeek2026

# UN System Data Commons | Getting Started

# Hackathon environments

#### Custom DC

[https://un-sdg-customdc-datacommons-web-service-777379434764.us-central1.run.app/](https://un-sdg-customdc-datacommons-web-service-777379434764.us-central1.run.app/)

#### DC Platform

[https://dc-un-dev-dc-datacommons-service-72utkhfqvq-uc.a.run.app/](https://dc-un-dev-dc-datacommons-service-72utkhfqvq-uc.a.run.app/)

# Connecting to MCP endpoints

For more detailed information, follow the instructions available on [datacommons.org](http://datacommons.org), starting with [Configure the MCP server \- Docs \- Data Commons](https://docs.datacommons.org/custom_dc/mcp.html).

First identify the {{dc\_base\_url}} for the environment to be used. The MCP endpoint is available from the path `{{dc_base_url}}/mcp` (for example, [`https://dc-un-dev-dc-datacommons-service-72utkhfqvq-uc.a.run.app/mcp`](https://dc-un-dev-dc-datacommons-service-72utkhfqvq-uc.a.run.app/mcp))

## Connecting with Gemini

*Determine whether you prefer to directly modify the settings.json for your gemini installation, or potentially use the cli to directly add the MCP service.*

*To install Gemini, refer to the documentation : [https://geminicli.com/docs/](https://geminicli.com/docs/)*

#### Option a: Modify the gemini settings.json

```
$ open .gemini/settings.json

# Ensure the contents include:
{
  ... // Note there may be additional settings preceding the mcpServers
  "mcpServers": {
    "undata-cdc": {
      "httpUrl": "https://dc-un-dev-dc-datacommons-service-72utkhfqvq-uc.a.run.app/mcp"
    }
  }
}
```

#### Option b: Associate the UN MCP server via cli

*Note that the cli has the same effect by modifying the settings.json above*

```
$ gemini mcp add undata {{dc_base_url}}/mcp

e.g.
$ gemini mcp add --transport http undata https://dc-un-dev-dc-datacommons-service-72utkhfqvq-uc.a.run.app/
```

### Post connection

**Open the gemini console**

```
$ gemini
```

**Confirm that the connection is valid** 

```
# Simple discovery
$ /mcp

...
Note: First startup may take longer. Tool availability will update automatically.

Configured MCP servers:

🟢 undata - Ready (2 tools)
  Tools:
  - mcp_undata_get_observations
  - mcp_undata_search_indicators

# And for more details on the MCP tools
$ /mcp desc undata
```

If you do not see a green `Ready` for the mcp instance, reach out. This is a	potential cause for concern.

**Using the MCP**   
Listing the tools can provide initial gateway to the types of queries to interface with via MCP.

```
$ /mcp list

$ What variables are available from undta for describing climate change?

...
Data Commons provides a wide range of variables for describing climate change, which are primarily organized under
  Greenhouse Gas / CO₂ Emissions and UN Sustainable Development Goal 13 (SDG 13: Climate Action).

  Here is a structured breakdown of the key variables available, along with their unique identifier (DCID):

  ---

  1. Greenhouse Gas (GHG) and Carbon Dioxide (CO₂) Emissions
  These variables track actual emission amounts overall, per capita, or by specific sectors.

  Total & Per Capita Emissions
   * Total Greenhouse Gas Emissions: Annual_Emissions_GreenhouseGas
   * Non-Biogenic Greenhouse Gas Emissions: Annual_Emissions_GreenhouseGas_NonBiogenic
   * CO₂ Emissions (Annual): eia/INTL.4008-8-MMTCD.A
   * CO₂ Emissions Per Capita: Amount_Emissions_CarbonDioxide_PerCapita

  Sector-Specific Emissions (CO₂ vs. GHG)
...
```

# API Interactions with Postman

While it's possible to use curl to issue all of your API requests for development and testing, [Postman](https://www.getpostman.com/) is available to make API exploration easier.

The attached files comprise a postman collection of API endpoints and the relevant environment variables. The work with both Custom Data Commons and [datacommons.org](http://datacommons.org) by changing the `base_url` in the environment variables.

### Files

* \`datacommons\_rest\_apis.postman\_collection.json\`: Postman collection.  
* \`datacommons\_rest\_apis.postman\_environment.json\`: Postman environment with configurable \`base\_url\` and \`api\_key\`.

### Referenced APIs

The collection currently includes REST V2 sample requests for:

* \`/v2/node\`: fetch node property labels and linked values.  
* \`/v2/observation\`: fetch statistical observations, available variables, facets, and filtered observations.  
* \`/v2/resolve\`: find DCIDs before using Node or Observation.

The requests use POST bodies from the REST V2 examples so relation expressions do not need to be URL-encoded by hand.

### How to configure

Import both JSON files into Postman, select the \`Data Commons REST APIs Environment\`, then update the values as needed for:

* \`base\_url\`: API base URL without a trailing slash and without the API version.  
  * Custom DC pattern: \`https://\<CUSTOM\_HOST\>/core/api\` (e.g. https://dc-un-dev-dc-datacommons-service-72utkhfqvq-uc.a.run.app/core/api)  
  * datacommons.org pattern: \`[https://api.datacommons.org](https://api.datacommons.org)\`  
* \`api\_key\`: Leave blank for Custom Data Commons. Set only when calling datacommons.org.

The collection adds the \`X-API-Key\` header only when \`api\_key\` is non-empty.

Keeping \`base\_url\` at \`/core/api\` instead of \`/core/api/v2\` leaves room to add other API families later.

#### Loading in Postman

1. Open Postman and choose the workspace you want to use. Note that you do not need to create an account or to log into Postman in order to use the ‘thin’ local client.  
2. Click \`Import\` in the sidebar.  
3. Select both JSON files from this folder.  
4. Click \`Import\`.  
5. Select the \`Data Commons REST APIs Environment\`.  
6. Set \`base\_url\` to the Data Commons endpoint, for example:

   \`\`\`text  
   https://\<CUSTOM\_HOST\>/core/api  
   \`\`\`

7. Keep \`api\_key\` blank for Custom Data Commons.  
8. Open the collection and run the v2 Node / Observation / Resolve requests.

