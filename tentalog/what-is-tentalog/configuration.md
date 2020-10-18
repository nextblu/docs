---
description: Explore sample configurations and explaination on how we manage our log
---

# A custom configuration

 If you want to use a custom configuration you can just initialize your Tentacle in this way:

```text
tentacle = Tentacle(path='myconf.yaml')
```

### Yeah, but how the YAML looks like?

Here a quick look at _default\_configuration.yaml_

```text
version: 1
disable_existing_loggers: false

formatters:
  defaultFormatter:
    format: '%(asctime)s - %(filename)s - %(levelname)s - %(name)s - %(message)s'

handlers:
  console:
    class: logging.StreamHandler
    level: DEBUG
    formatter: defaultFormatter
  file:
    class: logging.handlers.TimedRotatingFileHandler
    level: INFO
    formatter: defaultFormatter
    filename: ./logs/log.log
    when: midnight
    interval: 15
    backupCount: 0

loggers:
  root:
    level: DEBUG
    handlers: [console, file]
    propagate: no

coloredlogs:
  active: true
  formatter: defaultFormatter

```

/\*TODO ADD EXPL ON HOW TO CHANGE THE VALUES IN THE YAML FILE\*/

