---
subcategory: "Cloud Trace Service (CTS)"
---

# huaweicloud_cts_tracker

Manages CTS **system** tracker resource within HuaweiCloud.

## Example Usage

```hcl
variable "bucket_name" {}

resource "huaweicloud_cts_tracker" "tracker" {
  bucket_name = var.bucket_name
  file_prefix = "cts"
  lts_enabled = true
}
```

## Argument Reference

The following arguments are supported:

* `region` - (Optional, String, ForceNew) Specifies the region in which to manage the CTS system tracker resource.
  If omitted, the provider-level region will be used. Changing this creates a new resource.

* `bucket_name` - (Optional, String) Specifies the OBS bucket to which traces will be transferred.

* `file_prefix` - (Optional, String) Specifies the file name prefix to mark trace files that need to be stored
  in an OBS bucket. The value contains 0 to 64 characters. Only letters, numbers, hyphens (-), underscores (_),
  and periods (.) are allowed.

* `lts_enabled` - (Optional, Bool) Specifies whether trace analysis is enabled.

* `organization_enabled` - (Optional, Bool) Specifies whether to apply the tracker configuration to the organization.
  If the value is set to **true**, the audit logs of all members in the organization in the current region will be
  transferred to the OBS bucket or LTS log stream configured for the management tracker.

* `validate_file` - (Optional, Bool) Specifies whether trace file verification is enabled during trace transfer.

* `kms_id` - (Optional, String) Specifies the ID of KMS key used for trace file encryption.

* `enabled` - (Optional, Bool) Specifies whether tracker is enabled.

## Attribute Reference

In addition to all arguments above, the following attributes are exported:

* `id` - The resource ID in UUID format.
* `name` - The tracker name, only **system** is available.
* `type` - The tracker type, only **system** is available.
* `transfer_enabled` - Whether traces will be transferred.
* `status` - The tracker status, the value can be **enabled**, **disabled** or **error**.

## Timeouts

This resource provides the following timeouts configuration options:

* `create` - Default is 5 minutes.
* `delete` - Default is 5 minutes.

## Import

CTS tracker can be imported using `name`, only **system** is available. e.g.

```
$ terraform import huaweicloud_cts_tracker.tracker system
```
