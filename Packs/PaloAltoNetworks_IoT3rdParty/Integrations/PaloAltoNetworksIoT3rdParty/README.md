Use Palo Alto Networks IoT 3rd Party to integrate Palo Alto Networks IoT solution with 3rd party products such as SIEM, Cisco ISE, Service Now etc.
## Configure Palo Alto Networks IoT 3rd Party on Cortex XSOAR

1. Navigate to **Settings** &gt; **Integrations** &gt; **Servers & Services**.
2. Search for Palo Alto Networks IoT 3rd Party.
3. Click **Add instance** to create and configure a new integration instance.

| **Parameter** | **Description** | **Required** |
| --- | --- | --- |
| URL | Palo Alto Networks IoT Security Portal URL \(e.g. https://example.iot.paloaltonetworks.com\) | True |
| Customer ID | Tenant ID | True |
| Key ID | Access Key ID | True |
| Access Key | Secret Access Key | True |
| isFetch | Fetch incidents | False |

4. Click **Test** to validate the URLs, token, and connection.
## Commands
You can execute these commands from the Demisto CLI, as part of an automation, or in a playbook.
After you successfully execute a command, a DBot message appears in the War Room with the command details.
### panw-iot-3rd-party-report-status-to-panw
***
PANW IoT 3rd Party Report Status to PANW command - Sends a status message back to PANW IOT cloud.

#### Base Command

`panw-iot-3rd-party-report-status-to-panw`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| status | Message to be sent to PANW IoT Cloud | Required |
| message | Message to be sent to PANW IoT Cloud | Required |
| integrationName | Name of PANW IoT 3rd Party Integration | Required |
| playbookName | Name of the playbook | Required |
| type | Type of asset associated with the status report | Required |


#### Context Output

There is no context output for this command.

#### Command Example
```!panw-iot-3rd-party-report-status-to-panw status=success message="successfully updated 100 devices" integrationName=ise playbookName="Increment Export to Cisco ISE - PANW IoT 3rd Party Integration" type=device```

#### Human Readable Output
### Reporting Status:
|||
| --- | --- |
| integration_name | ise |
| iot_cloud_response | received: yes |
| message | successfully updated 100 devices |
| playbook_name | Increment Export to Cisco ISE - PANW IoT 3rd Party Integration |
| status | success |
| timestamp | 1606106283993 |
| type | device |


### panw-iot-3rd-party-get-single-asset
***
PANW IoT 3rd Party get single Asset - For a given a asset ID (alert-id, vulnerability-id or mac-address) returns the asset details.


#### Base Command

`panw-iot-3rd-party-get-single-asset`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| assetType | Type of Asset | Required |
| assetID | Asset ID. MacAddress for device, zb_ticketid for alert and vulnerability | Required |


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PanwIot3rdParty.SingleAsset | unknown | Asset Details |


#### Command Example
```!panw-iot-3rd-party-get-single-asset assetType="Device" assetID="00:e0:4c:68:09:16"```

#### Human Readable Output
### Successfully pulled Device (00:e0:4c:68:09:16) from PANW IoT Cloud


### panw-iot-3rd-party-get-asset-list
***
PANW IoT 3rd Party get asset list - Returns a list of assets for the specified asset type.


#### Base Command

`panw-iot-3rd-party-get-asset-list`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| assetType | Type of Asset | Required |
| incrementTime | Increment time in minutes. Example: Increment Time = 15 mins will return input type assets modified or discovered within the last 15 minutes. A Null value will return full inventory (1000 MAX) | Optional |
| offset | Offset for paging: Null value will accumulate all results by default | Optional |
| pageLength | Page size for paging: Null value will accumulate all results by default | Optional |


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PanwIot3rdParty.Devices | unknown | List of Devices |
| PanwIot3rdParty.Alerts | unknown | List of Alerts |
| PanwIot3rdParty.Vulnerabilities | unknown | List of Vulnerabilities |


#### Command Example
```!panw-iot-3rd-party-get-asset-list assetType="Device" incrementTime="2"```

#### Human Readable Output
### Asset import summary:
|||
| --- | --- |
| asset type | Device |
| assets pulled | 11 |



### panw-iot-3rd-party-convert-assets-to-external-format
***
PANW IoT 3rd Party convert assets to external foramt - For a given asset (alert, device, vuln) converts it to 3rd party format.


#### Base Command

`panw-iot-3rd-party-convert-assets-to-external-format`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| assetType | Input asset type | Required |
| outputFormat | Desired output format | Required |
| assetList | List of input assets | Required |
| metadata | Example: ServiceNow ID and deviceid mapping | Optional |
| metadata1 | Example: incident triggered by PANW IoT cloud API | Optional |


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PanwIot3rdParty.VulnerabilityCEFSyslogs | unknown | List of CEF formatted vulnerability syslogs for SIEM |
| PanwIot3rdParty.AlertCEFSyslogs | unknown | List of CEF formatted alert syslogs for SIEM |
| PanwIot3rdParty.DeviceCEFSyslogs | unknown | List of CEF formatted device syslogs for SIEM |
| PanwIot3rdParty.CiscoISEAttributes | unknown | List of Cisco ISE attribute dicts/maps |
| PanwIot3rdParty.AlertServiceNow | unknown | Single SN formatted alert string |
| PanwIot3rdParty.VulnerabilityServiceNow | unknown | Single SN formatted vulnerability string |
| PanwIot3rdParty.DeviceServiceNow | unknown | List of upsert ready formatted device for SN |


#### Command Example
```!panw-iot-3rd-party-convert-assets-to-external-format assetType=Device outputFormat=siem assetList=[a list of 221 device maps]```

#### Human Readable Output
### Converted 221 Device to SIEM
