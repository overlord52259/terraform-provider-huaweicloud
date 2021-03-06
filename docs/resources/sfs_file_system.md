---
subcategory: "Scalable File Service (SFS)"
---

# huaweicloud\_sfs\_file\_system

Provides an Shared File System (SFS) resource.
This is an alternative to `huaweicloud_sfs_file_system_v2`

## Example Usage

```hcl
variable "share_name" { }
variable "share_description" { }
variable "vpc_id" { }

resource "huaweicloud_sfs_file_system" "share-file" {
  name         = var.share_name
  size         = 100
  share_proto  = "NFS"
  access_level = "rw"
  access_to    = var.vpc_id
  description  = var.share_description

  tags {
    key = "value"
  }
}
```

## Argument Reference
The following arguments are supported:

* `region` - (Optional) The region in which to obtain the sfs resource. If omitted, the provider-level region will work as default. Changing this creates a new sfs resource.

* `size` - (Required) The size (GB) of the shared file system.

* `share_proto` - (Optional) The protocol for sharing file systems. The default value is NFS.

* `name` - (Optional) The name of the shared file system.

* `description` - (Optional) Describes the shared file system.

* `is_public` - (Optional) The level of visibility for the shared file system.

* `metadata` - (Optional) Metadata key and value pairs as a dictionary of strings. Changing this will create a new resource.

* `availability_zone` - (Optional) The availability zone name. Changing this parameter will create a new resource.

* `enterprise_project_id` - (Optional) The enterprise project id of the shared file system. Changing this creates a new resource.

* `tags` - (Optional) The key/value pairs to associate with the shared file system.

* `access_level` - (Optional) Specifies the access level of the shared file system. Possible values are *ro* (read-only)
    and *rw* (read-write). The default value is *rw* (read/write). Changing this will create a new access rule.

* `access_type` - (Optional) Specifies the type of the share access rule. The default value is *cert*.
    Changing this will create a new access rule.

* `access_to` - (Optional) Specifies the value that defines the access rule. The value contains 1 to 255 characters.
    Changing this will create a new access rule. The value varies according to the scenario:
    - Set the VPC ID in VPC authorization scenarios.
    - Set this parameter in IP address authorization scenario.

        For an NFS shared file system, the value in the format of *VPC_ID#IP_address#priority#user_permission*.
        For example, 0157b53f-4974-4e80-91c9-098532bcaf00#2.2.2.2/16#100#all_squash,root_squash.

        For a CIFS shared file system, the value in the format of *VPC_ID#IP_address#priority*.
        For example, 0157b53f-4974-4e80-91c9-098532bcaf00#2.2.2.2/16#0.

-> **NOTE:** If you want to create more access rules, please using [huaweicloud_sfs_access_rule](https://www.terraform.io/docs/providers/huaweicloud/r/sfs_access_rule.html).


## Attributes Reference
In addition to all arguments above, the following attributes are exported:

* `id` - The UUID of the shared file system.

* `status` - The status of the shared file system.

* `share_type` - The storage service type assigned for the shared file system, such as high-performance
    storage (composed of SSDs) and large-capacity storage (composed of SATA disks).

* `export_location` - The address for accessing the shared file system.

* `host` - The host name of the shared file system.

* `share_access_id` - The UUID of the share access rule.

* `access_rule_status` - The status of the share access rule.

* `access_rules` - All access rules of the shared file system. The object includes the following:
    - `access_rule_id` - The UUID of the share access rule.
    - `access_level` - The access level of the shared file system
    - `access_type` - The type of the share access rule.
    - `access_to` - The value that defines the access rule.
    - `status` - The status of the share access rule.

## Import

SFS can be imported using the `id`, e.g.

```
$ terraform import huaweicloud_sfs_file_system 4779ab1c-7c1a-44b1-a02e-93dfc361b32d
```
