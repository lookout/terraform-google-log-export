# Log Export: Storage destination submodule

This submodule allows you to configure a Google Cloud Storage bucket destination that
can be used by the log export created in the root module.

## Usage

Consider the following
example that will configure a storage bucket destination and a log export at the project level:

```hcl
module "log_export" {
  source                 = "<path-to-log-export-module>"
  destination_uri        = "${module.destination.destination_uri}"
  filter                 = "severity >= ERROR"
  log_sink_name          = "storage_example_logsink"
  parent_resource_id     = "sample-project"
  parent_resource_type   = "project"
  unique_writer_identity = true
}

module "destination" {
  source                   = "<path-to-log-export-module>/modules/storage"
  project_id               = "sample-project"
  storage_bucket_name      = "sample_storage_bucket"
  log_sink_writer_identity = "${module.log_export.writer_identity}"
}
```

At first glance that example seems like a circular dependency as each module declaration is
using an output from the other, however Terraform is able to collect and order all the resources
so that all dependencies are met.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| lifecycle\_rules | List of lifecycle rules to configure. Format is the same as described in provider documentation https://www.terraform.io/docs/providers/google/r/storage_bucket.html#lifecycle_rule except condition.matches_storage_class should be a comma delimited string. | <pre>set(object({<br>    # Object with keys:<br>    # - type - The type of the action of this Lifecycle Rule. Supported values: Delete and SetStorageClass.<br>    # - storage_class - (Required if action type is SetStorageClass) The target Storage Class of objects affected by this Lifecycle Rule.<br>    action = map(string)<br><br>    # Object with keys:<br>    # - age - (Optional) Minimum age of an object in days to satisfy this condition.<br>    # - created_before - (Optional) Creation date of an object in RFC 3339 (e.g. 2017-06-13) to satisfy this condition.<br>    # - with_state - (Optional) Match to live and/or archived objects. Supported values include: "LIVE", "ARCHIVED", "ANY".<br>    # - matches_storage_class - (Optional) Comma delimited string for storage class of objects to satisfy this condition. Supported values include: MULTI_REGIONAL, REGIONAL, NEARLINE, COLDLINE, STANDARD, DURABLE_REDUCED_AVAILABILITY.<br>    # - num_newer_versions - (Optional) Relevant only for versioned objects. The number of newer versions of an object to satisfy this condition.<br>    condition = map(string)<br>  }))</pre> | `[]` | no |
| force\_destroy | When deleting a bucket, this boolean option will delete all contained objects. | `bool` | `false` | no |
| location | The location of the storage bucket. | `string` | `"US"` | no |
| log\_sink\_writer\_identity | The service account that logging uses to write log entries to the destination. (This is available as an output coming from the root module). | `string` | n/a | yes |
| project\_id | The ID of the project in which the storage bucket will be created. | `string` | n/a | yes |
| retention\_policy | Configuration of the bucket's data retention policy for how long objects in the bucket should be retained. | <pre>object({<br>    is_locked             = bool<br>    retention_period_days = number<br>  })</pre> | `null` | no |
| storage\_bucket\_name | The name of the storage bucket to be created and used for log entries matching the filter. | `string` | n/a | yes |
| storage\_class | The storage class of the storage bucket. | `string` | `"STANDARD"` | no |
| uniform\_bucket\_level\_access | Enables Uniform bucket-level access to a bucket. | `bool` | `true` | no |
| versioning | Toggles bucket versioning, ability to retain a non-current object version when the live object version gets replaced or deleted. | `bool` | `false` | no |

## Outputs

| Name | Description |
|------|-------------|
| console\_link | The console link to the destination storage bucket |
| destination\_uri | The destination URI for the storage bucket. |
| project | The project in which the storage bucket was created. |
| resource\_id | The resource id for the destination storage bucket |
| resource\_name | The resource name for the destination storage bucket |
| self\_link | The self\_link URI for the destination storage bucket |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
