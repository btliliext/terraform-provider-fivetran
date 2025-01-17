---
page_title: "Resource: fivetran_connector_schedule"
---

# Resource: fivetran_connector_schedule

-This resource allows you to manage connectors schedule: pause/unpause connector, set daily_sync_time and sync_frequency.

## Example Usage

```hcl
resource "fivetran_connector_schedule" "my_connector_schedule" {
    connector_id = fivetran_connector.my_connector.id

    sync_frequency     = "1440"
    daily_sync_time    = "03:00"

    paused             = false
    pause_after_trial  = true

    schedule_type      = "auto"
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `connector_id` (String) The unique identifier for the connector

### Optional

- `daily_sync_time` (String) The optional parameter that defines the sync start time when the sync frequency is already set or being set by the current request to 1440. It can be specified in one hour increments starting from 00:00 to 23:00. If not specified, we will use [the baseline sync start time](https://fivetran.com/docs/getting-started/syncoverview#syncfrequencyandscheduling). This parameter has no effect on the [0 to 60 minutes offset](https://fivetran.com/docs/getting-started/syncoverview#syncstarttimesandoffsets) used to determine the actual sync start time
- `pause_after_trial` (String) Specifies whether the connector should be paused after the free trial period has ended
- `paused` (String) Specifies whether the connector is paused
- `schedule_type` (String) The connector schedule configuration type. Supported values: auto, manual
- `sync_frequency` (String) The connector sync frequency in minutes. Supported values: 5, 15, 30, 60, 120, 180, 360, 480, 720, 1440.

### Read-Only

- `id` (String) The unique resource identifier (equals to `connector_id`).

## Import

You don't need to import this resource as it is synthetic. 

To fetch schedule values from existing connector use `fivetran_connector` data source:
```hcl
data "fivetran_connector" "my_connector" {
    id = "my_connector_id"
}

# now you can use schedule values from this data_source:
#   sync_frequency = data.fivetran_connector.my_connector.sync_frequency
#   paused = data.fivetran_connector.my_connector.paused
```

This resource manages settings for already existing connector instance and doesn't create a new one.
If you already have an existing connector with id = `my_connector_id` just define `fivetran_connector_schedule` resource:

```hcl
resource "fivetran_connector_schedule" "my_connector_schedule" {
    connector_id = "my_connector_id"

    sync_frequency     = "360"
    paused             = false
    pause_after_trial  = true
    schedule_type      = "auto"
}
```

-> NOTE: You can't have several resources managing the same `connector_id`. They will be in conflict ater each `apply`.