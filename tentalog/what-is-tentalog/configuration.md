---
description: Explore sample configurations and explaination on how we manage our log
---

# A custom configuration

 If you want to use a custom configuration you can just initialize your Tentacle in this way:

```python
tentacle = Tentacle(path='myconf.yaml')
```

### Yeah, but how the YAML looks like?

Here a quick look at _default\_configuration.yaml_

```yaml
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

### Configuration management

Tentalog aims to grant the user the same configuration flexibility of the pure _logging_ module, with the benefit of the usage of the YAML format to allow change tracking and having an overview of the current configuration at glance in a human-readable format.

Currently there are a few main keys for the logging configuration:

* version
* disable\_existing\_loggers
* formatters
* handlers
* loggers
* coloredlogs

#### version

The version key allows to update the current file versione whenever is needed. For example you could want to update the version at each tag or at each commit, based on your versioning workflow. It's a plain number and is configured like this:

```yaml
version: 1
```

#### disable\_existing\_loggers

This key is a parameter required by the _logging_ module whenever a file configuration is used. It will cause any non-root loggers existing before the current configuration with file to be disabled unless they or an ancestor are explicitly named in the configuration.

{% hint style="info" %}
In order to pursue retrocompatibility, the advice is to set this parameter to false, but feel free to set your own value.
{% endhint %}

```yaml
version: 1
disable_existing_loggers: false
```

#### formatters

This section is a map where any key is a formatter object with its own format. Here you can create all the formatters you need in your loggers. Here an example:

```yaml
version: 1
disable_existing_loggers: false

formatters:
  defaultFormatter:
    format: '%(asctime)s - %(filename)s - %(levelname)s - %(name)s - %(message)s'
  lightFormatter:
    format: '%(asctime)s - %(message)s'
```

{% hint style="info" %}
Choose wisely which information to print with each formatter accordingly to which handler it will use it. If a specific format is too verbose could be need more resources to get stored/processed. On the other hand, if a format is too general, could not carry the right amount of information to get an overview of the current state.
{% endhint %}

#### handlers

This section is a map where any key is a handler object. You can configure each handler as map accordingly to the specific fields for each class in the logging module documentation

{% embed url="https://docs.python.org/3/library/logging.handlers.html" %}

Here an example:

```yaml
version: 1
disable_existing_loggers: false

formatters:
  defaultFormatter:
    format: '%(asctime)s - %(filename)s - %(levelname)s - %(name)s - %(message)s'    

handlers:
  consoleHandler:
    class: logging.StreamHandler
    formatter: defaultFormatter
    level: INFO
    stream: ext://sys.stdout    
```

#### loggers

This is the section where you can define all your loggers. Let's start with an example:

```yaml
version: 1
disable_existing_loggers: false

formatters:
  defaultFormatter:
    format: '%(asctime)s - %(filename)s - %(levelname)s - %(name)s - %(message)s'

handlers:
  consoleDebug:
    class: logging.StreamHandler
    level: DEBUG
    formatter: defaultFormatter
  consoleInfo:
    class: logging.StreamHandler
    level: INFO
    formatter: defaultFormatter

loggers:
  root:
    level: DEBUG
    handlers: [console]
    propagate: yes
  myLogger:
    level: DEBUG
    handlers: [consoleInfo]
```

As you can see, there are a few configuration parameters that allow you to create your loggers based on your handlers. You can declare a child logger by using the form _parent.child_ where parent and child are the logger names. Remember that the _root_ logger is the parent of all the loggers.

{% hint style="info" %}
Be aware of the use of the _propagate_ parameter. In fact, if set to _True_ it allows a logger to propagate any received message to its ancestor's handlers. If the configuration is not properly defined, there could be a duplication of logs. Anyway, it's a powerful tool that could be used to create complex logging structures.
{% endhint %}

#### coloredlogs

Tentalog comes with the [coloredlogs](https://pypi.org/project/coloredlogs/) library in order to give you the pleasure to distinguish all the information in the console output. It can be activated by declaring the block and setting the key _active_ to _true_. It accepts a formatter. It should correspond to the formatter used for the handlers that will print the logs in console. Here is the example:

```yaml
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

loggers:
  root:
    level: DEBUG
    handlers: [console]
    propagate: no

coloredlogs:
  active: true
  formatter: defaultFormatter
```



