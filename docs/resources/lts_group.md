---
subcategory: "Log Tank Service (LTS)"
---

# huaweicloud_lts_group

Manages a log group resource within HuaweiCloud.

## Example Usage

```hcl
resource "huaweicloud_lts_group" "log_group1" {
  group_name  = "log_group1"
  ttl_in_days = 30
}
```

## Argument Reference

The following arguments are supported:

* `region` - (Optional, String, ForceNew) Specifies the region in which to create the log group resource. If omitted, the
  provider-level region will be used. Changing this creates a new log group resource.

* `group_name` - (Required, String, ForceNew) Specifies the log group name. Changing this parameter will create a new
  resource.

* `ttl_in_days` - (Required, Int) Specifies the log expiration time(days), value range: 1-365.

* `tags` - (Optional, Map) Specifies the key/value pairs to associate with the log group.

  -> Currently, the key in tags can not be removed.

## Attribute Reference

In addition to all arguments above, the following attributes are exported:

* `id` - The log group ID.

* `created_at` - The creation time of the log group.

## Import

The log group can be imported using the `id`, e.g.

```
$ terraform import huaweicloud_lts_group.group_1 7117d38e-4c8f-4624-a505-bd96b97d024c
```
