---
subcategory: "Distributed Cache Service"
---

# huaweicloud\_dcs\_maintainwindow

Use this data source to get the ID of an available Huaweicloud dcs maintainwindow.
This is an alternative to `huaweicloud_dcs_maintainwindow_v1`

## Example Usage

```hcl

data "huaweicloud_dcs_maintainwindow" "maintainwindow1" {
  seq = 1
}

```

## Argument Reference

* `region` - (Optional) The region in which to obtain the dcs maintainwindows. If omitted, the provider-level region will work as default.

* `seq` - (Required) Indicates the sequential number of a maintenance time window.

* `begin` - (Optional) Indicates the time at which a maintenance time window starts.

* `end` - (Required) Indicates the time at which a maintenance time window ends.

* `default` - (Required) Indicates whether a maintenance time window is set to the default time segment.

## Attributes Reference

`id` is set to the ID of the found maintainwindow. In addition, the following attributes
are exported:

* `begin` - See Argument Reference above.
* `end` - See Argument Reference above.
* `default` - See Argument Reference above.
