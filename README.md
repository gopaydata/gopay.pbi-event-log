````md
# GoPay Power BI Event Logs Extractor

Extract Power BI activity event logs via Power BI Admin REST API.

---

# Description

This component downloads Power BI audit and activity logs using the Power BI Admin API endpoint:

```text
GET /v1.0/myorg/admin/activityevents
````

The extractor stores activity logs into Keboola output tables for further analysis, monitoring, auditing, and reporting.

---

# Functionality Notes

The component uses Azure AD Service Principal authentication (`client_credentials` OAuth2 flow).

Fetched data includes:

* User activity
* Report usage
* Dataset activity
* Workspace operations
* Sharing events
* Consumption metrics
* Distribution methods
* Success/failure information

The component downloads logs for the last 8 days.

---

# Prerequisites

Before using this extractor:

1. Create an Azure App Registration
2. Generate a Client Secret
3. Enable Power BI Service API permissions
4. Allow Service Principals in Power BI Admin Portal
5. Grant the application Power BI admin permissions if required

The Service Principal must have permission to access the Power BI Admin APIs.

Required configuration parameters:

| Parameter       | Description                   |
| --------------- | ----------------------------- |
| `client_id`     | Azure Application (Client) ID |
| `client_secret` | Azure Client Secret VALUE     |
| `tenant_id`     | Azure Directory (Tenant) ID   |
| `incremental`   | Enables incremental loading   |

---

# Authentication

The component authenticates using OAuth2 Client Credentials flow.

Token endpoint:

```text
https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token
```

Request body:

```text
grant_type=client_credentials
client_id=<client_id>
client_secret=<client_secret>
scope=https://analysis.windows.net/powerbi/api/.default
```

---

# Features

| Feature                 | Description                               |
| ----------------------- | ----------------------------------------- |
| OAuth2 Authentication   | Azure AD Service Principal authentication |
| Incremental Loading     | Supports incremental table loading        |
| Activity Log Extraction | Downloads Power BI activity events        |
| Pagination Support      | Uses continuation URI handling            |
| Multi-day Extraction    | Downloads data for the last 8 days        |
| CSV Output              | Stores data into Keboola output tables    |

---

# Output

Generated output tables:

```text
data/out/tables/pbi_event_logs.csv
```

Main columns:

| Column              | Description            |
| ------------------- | ---------------------- |
| `Id`                | Event ID               |
| `CreationTime`      | Event timestamp        |
| `Operation`         | Executed operation     |
| `UserId`            | User performing action |
| `Activity`          | Activity type          |
| `WorkspaceId`       | Workspace identifier   |
| `DatasetId`         | Dataset identifier     |
| `ReportId`          | Report identifier      |
| `ArtifactName`      | Artifact name          |
| `ArtifactKind`      | Artifact type          |
| `IsSuccess`         | Success flag           |
| `ConsumptionMethod` | Consumption method     |

---

# Used APIs

Authentication:

```text
https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token
```

Power BI REST API:

```text
GET /v1.0/myorg/admin/activityevents
```

---

# Development

If required, change local data folder path in `docker-compose.yml`:

```yaml
volumes:
  - ./:/code
  - ./CUSTOM_FOLDER:/data
```

Clone repository and run locally:

```bash
git clone https://github.com/gopaydata/gopay.pbi-event-log gopay.pbi-event-log
cd gopay.pbi-event-log
docker-compose build
docker-compose run --rm dev
```

Run tests:

```bash
docker-compose run --rm test
```

---

# Integration

For deployment and Keboola integration documentation:

https://developers.keboola.com/extend/component/deployment/

```
```
