uuid: 588a448b-c08d-4139-a746-b2b9f366e34b
name: Cloudflare Access Request
type: intake

## Overview

Cloudflare is a global network designed to make everything you connect to the Internet secure, private, fast, and reliable.

In this documentation, you will learn how to collect and send Cloudflare Access Request logs to SEKOIA.IO.

{!_shared_content/operations_center/detection/generated/suggested_rules_588a448b-c08d-4139-a746-b2b9f366e34b_do_not_edit_manually.md!}

{!_shared_content/operations_center/integrations/generated/588a448b-c08d-4139-a746-b2b9f366e34b.md!}

## Configuration

### Create the intake on SEKOIA.IO

Go to the [intake page](https://app.sekoia.io/operations/intakes) and create a new intake from the format Cloudflare.

### Configure events forwarding on Cloudflare

{!_shared_content/operations_center/integrations/cloudflare_necessary_info.md!}

#### Create a Logpush job

Configure a [Logpush job](https://developers.cloudflare.com/logs/reference/logpush-api-configuration/){:target="_blank"} with the following destination:

`https://intake.sekoia.io/plain/batch?header_X-SEKOIAIO-INTAKE-KEY=<YOUR_INTAKE_KEY>`


To do so, you can manage Logpush with cURL:

```bash
$ curl -X POST 'https://api.cloudflare.com/client/v4/accounts/<CLOUDFLARE_ACCOUNT_ID>/logpush/jobs' \
-H 'Authorization: Bearer <CLOUDFLARE_API_TOKEN>' \
-H "Content-Type: application/json" \
-d '{
    "dataset": "access_requests",    
    "enabled": true,     
    "max_upload_bytes": 5000000,     
    "max_upload_records": 1000,
    "logpull_options":"fields=Action,Allowed,AppDomain,AppUUID,Connection,Country,CreatedAt,Email,IPAddress,PurposeJustificationPrompt,PurposeJustificationResponse,RayID,TemporaryAccessApprovers,TemporaryAccessDuration,UserUID&timestamps=rfc3339",
    "destination_conf": "https://intake.sekoia.io/plain/batch?header_X-SEKOIAIO-INTAKE-KEY=<YOUR_INTAKE_KEY>"
    }' # (1)
```

1. will return
```json
{
  "errors": [],
  "messages": [],
  "result": {
    "id": "<ID>",
    "dataset": "access_requests",
    "frequency":"high",
    "kind":"", 
    "max_upload_bytes": 5000000,     
    "max_upload_records": 1000, 
    "enabled": true,
    "name": "<DOMAIN_NAME>",
    "logpull_options": "fields=<LIST_OF_FIELDS>",
    "destination_conf": "https://intake.sekoia.io/plain/batch?header_X-SEKOIAIO-INTAKE-KEY=<YOUR_INTAKE_KEY>",
    "last_complete": null,
    "last_error": null,
    "error_message": null,
    "time_created":"<TIMESTAMP>"
  },
  "success": true
}
```

!!! Important
    Replace :

    - `<YOUR_INTAKE_KEY>` with the Intake key you generated in the [Create the intake on SEKOIA.IO](#create-the-intake-on-sekoiaio) step.
    - `<CLOUDFLARE_ACCOUNT_ID>` with the ACCOUNT_ID found on the overview page
    - `<CLOUDFLARE_API_TOKEN>` with the API Token you generated

{!_shared_content/operations_center/integrations/cloudflare_useful_accounts_scoped_api_endpoints.md!}
