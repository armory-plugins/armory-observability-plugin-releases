# armory-observability-plugin-releases

Spinnaker plugin for configuring and customizing observability features such as

- Customizing and tweaking the Micrometer registry.
- Enabling direct transmision of metrics to promethous and skipping the Spinnaker Monitoring Daemon

## Plugin Configuration

```yaml
spinnaker:
  extensibility:
    plugins:
      Armory.ObservabilityPlugin:
        enabled: true
        config:
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
            # Optional, Default: 8009
            scrapePort: 8009
            # Optional, Default: /prometheus
            path: /prometheus
    repositories:
      armory-observability-plugin-releases:
        url: https://raw.githubusercontent.com/armory-plugins/armory-observability-plugin-releases/master/repositories.json            
```

## Potential Future Additions
- Enable distrubuted tracing, Slueth?
- Customize logging to filter noise or have custom appender for shipping important logs to log aggregator
- Customize Error handling? Can we enable something like Backstopper in a plugin? 
