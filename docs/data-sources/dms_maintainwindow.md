---
subcategory: "Distributed Message Service (DMS)"
---

# huaweicloud_dms_maintainwindow

Use this data source to get the ID of an available HuaweiCloud dms maintainwindow.

## Example Usage

```hcl
data "huaweicloud_dms_maintainwindow" "maintainwindow1" {
  seq = 1
}
```

## Argument Reference

* `region` - (Optional, String) The region in which to obtain the dms maintainwindows. If omitted, the provider-level
  region will be used.

* `seq` - (Optional, Int) Indicates the sequential number of a maintenance time window.

* `begin` - (Optional, String) Indicates the time at which a maintenance time window starts.

* `end` - (Optional, String) Indicates the time at which a maintenance time window ends.

* `default` - (Optional, Bool) Indicates whether a maintenance time window is set to the default time segment.

## Attribute Reference

In addition to all arguments above, the following attributes are exported:

* `id` - Specifies a data source ID in UUID format.
