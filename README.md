# armory-observability-plugin-releases

Spinnaker plugin for configuring and customizing observability.

## What is Observability?

In control theory, observability is a measure of how well internal states of a system can be inferred from knowledge of its external outputs. The observability and controllability of a linear system are mathematical duals. 

The concept of observability was introduced by Hungarian-American engineer Rudolf E. Kálmán for linear dynamic systems.

In software metrics, logging and tracing make up the core categories of observability

## What does this plugin do?

- Enables customizing and tweaking the Micrometer registry.
- Exposes an [OpenMetrics](https://openmetrics.io/) endpoint for the Micrometer/Spectator metrics
  - This allows tools such as Promethous or the New Relic OpenMetrics integration to work without the [Spinnaker Monitoring Daemon](https://github.com/spinnaker/spinnaker-monitoring/tree/master/spinnaker-monitoring-daemon).

## Potential Future Additions
- Enable distrubuted tracing, Slueth?
- Customize logging to filter noise or have custom appender for shipping important logs to log aggregator
- Customize Error handling? Can we enable something like [Backstopper](https://github.com/Nike-Inc/backstopper) in a plugin? I have no idea ¯\\_(ツ)_/¯.

## Plugin Configuration

### Condensed Example
```yaml
spinnaker:
  extensibility:
    plugins:
      Armory.ObservabilityPlugin:
        enabled: true
        config.metrics:
          additionalTags:
            customerName: armory
            customerEnvName: production
          prometheus.stepInSeconds: 10
```

### All Options
```yaml
spinnaker:
  extensibility:
    plugins:
      Armory.ObservabilityPlugin:
        enabled: true
        config:
          metrics:
              # Key value map of extra static default tags to add when generated the default tags
              # Optional, Default: empty map
              additionalTags:
                  # Human Readable Customer name for dashboarding.
                  # Optional, Default: omitted from default tags
                  customerName: armory
                  # Human Readable Customer Environment name for dashboarding.
                  # Optional, Default: omitted from default tags
                  customerEnvName: production
                  # Halyard generated UUID for non-managed and non-sass customers
                  # Optional, Default: omitted from default tags
                  customerEnvId: e0fb0422-aa8e-11ea-bb37-0242ac130002
              
              # By default this plugin adds a set of sane default tags to help with observability best practices, you can disable those here
              # Optional, Default: false
              defaultTagsDisabled: false
              
              # By default this plugin does some sane filtering and transformations on metrics, you can disable those here
              # Optional, Default: false
              meterRegistryFiltersDisabled: false
    
              prometheus:
                # The step size to use in computing windowed statistics like max.
                # To get the most out of these statistics, align the step interval to be close to your scrape interval.
                # Optional, Default: 30
                stepInSeconds: 30
                # true if meter descriptions should be sent to Prometheus.
                # Turn this off to minimize the amount of data sent on each scrape.
                # Optional, Default: false
                descriptions: false
    repositories:
      armory-observability-plugin-releases:
        url: https://raw.githubusercontent.com/armory-plugins/armory-observability-plugin-releases/master/repositories.json            
```
