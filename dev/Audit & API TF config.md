https://stackoverflow.com/questions/71531832/is-it-possible-track-the-number-of-docker-pulls-in-google-artifact-registry

https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/google_project_iam#google_project_iam_audit_config

https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/logging_metric

https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/monitoring_monitored_project

https://registry.terraform.io/modules/lacework/audit-log/gcp/latest ??

### quota
https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/service_usage_consumer_quota_override

### slurm/ persistent VM management
https://docs.ansible.com/ansible/latest/collections/ansible/builtin/script_module.html


ChátGPT suggestion, fix and critique:
```
provider "google" {
  project = "<your-project-id>"
  region  = "us-central1" # or your preferred region
}

resource "google_monitoring_metric_descriptor" "artifact_registry_pulls" {
  project = "<your-project-id>"
  display_name = "Artifact Registry Pulls"
  type = "custom.googleapis.com/artifact_registry/pulls"
  metric_kind = "GAUGE"
  value_type = "INT64"
  labels {
    key = "repository"
    value_type = "STRING"
    description = "The name of the Artifact Registry repository"
  }
}

resource "google_monitoring_alert_policy" "pull_alert_policy" {
  display_name = "Artifact Registry Pulls Alert"
  project      = "<your-project-id>"

  conditions {
    display_name = "Pulls Alert Condition"
    condition_threshold {
      filter          = "metric.type=\"custom.googleapis.com/artifact_registry/pulls\" resource.type=\"gce_instance\" resource.labels.repository=\"<your-repository-name>\""
      comparison      = "COMPARISON_GT"
      threshold_value = 10 # Adjust this threshold as needed
      duration        = "60s"
      aggregations {
        alignment_period  = "60s"
        per_series_aligner = "ALIGN_RATE"
        cross_series_reducer = "REDUCE_SUM"
      }
    }
    display_name = "Pulls Alert Condition"
    description = "Alert when the number of pulls exceeds a threshold"
    condition_threshold {
      comparison = "COMPARISON_GT"
      threshold_value = 10 # Adjust this threshold as needed
      duration = "60s"
      filter = "metric.type=\"custom.googleapis.com/artifact_registry/pulls\" resource.type=\"gce_instance\" resource.labels.repository=\"<your-repository-name>\""
      aggregations {
        alignment_period = "60s"
        per_series_aligner = "ALIGN_RATE"
        cross_series_reducer = "REDUCE_SUM"
      }
    }
  }

  notification_channels = ["<your-notification-channel-id>"] # Define your notification channels here
}
```