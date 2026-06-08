# Logger System - Low Level Design

## Problem Statement

Design a configurable logging system that allows applications to record log messages at different severity levels and write them to one or more destinations.

The system should support filtering log messages based on a configured minimum severity level. Messages below the configured level should be ignored.

The logger should be extensible so that new log destinations can be added without modifying existing logging logic.

---

## Functional Requirements

### 1. Support Multiple Log Levels

The system should support the following log levels:

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

Example:

```java
logger.debug("Debug message");
logger.info("Application started");
logger.error("Payment failed");
```

---

### 2. Severity-Based Filtering

The logger should be configured with a minimum log severity.

Only messages with severity greater than or equal to the configured severity should be processed.

Example:

Configured Level: INFO

| Log Level | Should Log |
| --------- | ---------- |
| DEBUG     | No         |
| INFO      | Yes        |
| WARN      | Yes        |
| ERROR     | Yes        |
| FATAL     | Yes        |

---

### 3. Support Multiple Log Destinations

The logger should support writing logs to different destinations.

Examples:

* Console
* File
* Database

A log message may be written to multiple destinations simultaneously.

Example:

```java
LoggerConfig config = new LoggerConfig(
    LogSeverity.INFO,
    List.of(
        new ConsoleDestination(),
        new FileDestination()
    )
);
```

---

### 4. Log Message Metadata

Each log entry should contain:

* Log message
* Log severity
* Timestamp

These details should be encapsulated in a LogMessage object.

---

### 5. Extensibility

The design should allow new log destinations to be added without changing existing logger code.

Example:

```java
class KafkaDestination implements LogDestination
{
    @Override
    public void addLogToDestination(LogMessage logMessage)
    {
        // implementation
    }
}
```

---

## Non-Functional Requirements

* Follow Object-Oriented Design principles.
* Promote separation of concerns.
* Support future extension with minimal code changes.
* Keep the logger independent of destination-specific implementation details.

---

## Core Components

### LogSeverity

Represents the severity of a log message and determines whether the message should be processed.

### LogMessage

Represents a log entry containing:

* Message
* Severity
* Timestamp

### LogDestination

Abstraction representing a destination where logs can be written.

Implementations:

* ConsoleDestination
* FileDestination
* DbDestination

### LoggerConfig

Stores logger configuration:

* Minimum severity level
* List of destinations

### Logger

Acts as the entry point for clients.

Responsibilities:

* Accept log requests
* Validate severity threshold
* Create LogMessage objects
* Dispatch logs to configured destinations

---

```java
package lldproblems;

import java.time.LocalTime;
import java.util.List;

enum LogSeverity
{
    DEBUG(0), INFO(1), WARN(2), ERROR(3), FATAL(4);

    private final int code;

    LogSeverity(int code)
    {
        this.code=code;
    }

    int getCode()
    {
        return this.code;
    }
}

class LogMessage
{
    private final String message;
    private final LogSeverity severity;
    private final LocalTime logTimestamp;

    public LogMessage(String message, LogSeverity severity, LocalTime logTimestamp) {
        this.message = message;
        this.severity = severity;
        this.logTimestamp = logTimestamp;
    }
}

interface LogDestination
{
    void addLogToDestiny(LogMessage logMessage);
}

class DbDestination implements LogDestination
{
    @Override
    public void addLogToDestiny(LogMessage logMessage)
    {
        //logic;
    }
}

class FileDestination implements LogDestination
{
    @Override
    public void addLogToDestiny(LogMessage logMessage)
    {
        //logic;
    }
}

class ConsoleDestination implements LogDestination
{
    @Override
    public void addLogToDestiny(LogMessage logMessage)
    {
        //logic;
    }
}

class LoggerConfig
{
    private final LogSeverity severity;
    private final List<LogDestination> logDestinations;

    public LoggerConfig(LogSeverity severity, List<LogDestination> logDestinations) {
        this.severity = severity;
        this.logDestinations = logDestinations;
    }

    public LogSeverity getSeverity() {
        return severity;
    }

    public List<LogDestination> getLogDestinations() {
        return logDestinations;
    }
}

class Logger
{
    private final LoggerConfig loggerConfig;

    public Logger(LoggerConfig loggerConfig) {
        this.loggerConfig = loggerConfig;
    }

    private LogMessage getLogMessage(String s, LogSeverity logSeverity) {

        // Format message and assign;
        String formatedMessage = formatMessage(s);
        return null;
    }

    private String formatMessage(String s) {
        //logic;
        return null;
    }

    void debug(String s)
    {
        log(s, LogSeverity.DEBUG);
    }

    void info(String s)
    {
        log(s, LogSeverity.INFO);
    }

    void warn(String s)
    {
        log(s, LogSeverity.WARN);
    }

    void error(String s)
    {
        log(s, LogSeverity.ERROR);
    }

    void fatal(String s)
    {
        log(s, LogSeverity.FATAL);
    }

    private void log(String s, LogSeverity logSeverity)
    {
        if(!canProcessLog(logSeverity))
            return;

        LogMessage logMessage = getLogMessage(s, logSeverity);

        for(LogDestination dest:loggerConfig.getLogDestinations())
        {
            //Need to discuss on how these Objects will be created ?;
            dest.addLogToDestiny(logMessage);
        }
    }



    private boolean canProcessLog(LogSeverity currSeverity)
    {
        LogSeverity severity = loggerConfig.getSeverity();
        return currSeverity.getCode() >= severity.getCode();
    }


}


```