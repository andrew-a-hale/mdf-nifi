# NiFi Metadata Driven Framework

## Use Cases

1. Database Replication
1. File Ingestion
1. API

## Startup

```podman
podman run \
  --name nifi \
  -p 8443:8443 \
  -e SINGLE_USER_CREDENTIALS_USERNAME=admin \
  -e SINGLE_USER_CREDENTIALS_PASSWORD=passwordpassword \
  -v ./data:/opt/nifi/nifi-current/data:rw \
  -v ./queue:/opt/nifi/queue:rw \
  -v ./custom-scripts:/opt/nifi/custom-scripts:ro \
  -d \
  apache/nifi:latest
```

## Connector Types

- HTTP
- Local Network
- Cloud Storage
- Database (ODBC/JDBC)
- MDF PubSub

## Pipeline Architecture

1. Trigger to add message to queue
1. Read job from queue
1. Fetch Config for job from message data
1. Branch on source connector type
1. Finish source stage by producing NiFi Record
1. Branch on destination connector type
    - Handle Write Method
    - Handle Schema Changes
1. Load

## Questions

- Do we want to support any transformations?

## Ideas / Note

- Job Queues are prioritised with normal, high, urgent
- Version Controlled pipelines
- Internal State Management is cluster wide
- Optional Parameters block for each connector (defaults) with overrides in the configuration connector blocks
- Consider secret management
- Consider how to safely update configuration
- Consider change management around pipeline
- Set guidelines to prevent scope creep