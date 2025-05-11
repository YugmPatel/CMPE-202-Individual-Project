# CMPE-202-Individual-Project

# Log Analyzer System

## Project Overview
Log Analyzer is a command-line Java application developed to parse and analyze different types of log entries from log files. The application classifies logs into three categories (APM, Application, and Request logs), performs specialized aggregations on each type, and outputs the results to separate JSON files.

## Problem Statement
This project addresses the challenge of processing diverse log formats within a single file. The system needs to identify different log types, apply appropriate processing logic to each, and generate meaningful statistical summaries.

## Design Patterns Implemented
The application implements two key design patterns:

1. **Strategy Pattern**: Encapsulates different log processing algorithms, making them interchangeable through the `LogProcessor` and `LogAggregator` interfaces.

2. **Factory Pattern**: Centralizes the creation logic for processors in the `LogProcessorFactory` class, which determines the appropriate processor for each log entry.

## Features
- Parses three types of log entries:
  - **APM Logs**: Performance metrics (CPU, memory, etc.)
  - **Application Logs**: Application events with severity levels
  - **Request Logs**: HTTP request details

- Generates aggregations for each log type:
  - **APM Logs**: Min, median, average, and max values for each metric
  - **Application Logs**: Count of logs by severity level
  - **Request Logs**: Response time percentiles and status code counts per route

- Outputs results to three JSON files:
  - `apm.json`
  - `application.json`
  - `request.json`

## Project Structure
```
com.loganalyzer
├── LogApp.java (main application class)
├── model
│   ├── LogEntry.java (abstract base class)
│   ├── APMLogEntry.java
│   ├── ApplicationLogEntry.java
│   └── RequestLogEntry.java
├── processor
│   ├── LogProcessor.java (interface)
│   ├── APMLogProcessor.java
│   ├── ApplicationLogProcessor.java
│   └── RequestLogProcessor.java
├── factory
│   └── LogProcessorFactory.java
└── aggregator
    ├── LogAggregator.java (interface)
    ├── APMLogAggregator.java
    ├── ApplicationLogAggregator.java
    └── RequestLogAggregator.java
```

## How to Use
1. Clone the repository:
   ```
   git clone https://github.com/yourusername/log-analyzer.git
   ```

2. Build the project:
   ```
   cd log-analyzer
   mvn clean package
   ```

3. Run the application:
   ```
   java -jar target/log-analyzer-1.0-SNAPSHOT.jar --file input.txt
   ```

4. Check the output files:
   - `apm.json`: Contains APM metrics statistics
   - `application.json`: Contains log counts by severity level
   - `request.json`: Contains API route statistics

## Sample Log Format
```
timestamp=2024-02-24T16:22:15Z metric=cpu_usage_percent host=webserver1 value=72
timestamp=2024-02-24T16:22:20Z level=INFO message="Scheduled maintenance starting" host=webserver1
timestamp=2024-02-24T16:22:25Z request_method=POST request_url="/api/update" response_status=202 response_time_ms=200 host=webserver1
```

## Sample Output
### APM Logs (apm.json)
```json
{
  "cpu_usage_percent": {
    "minimum": 65.0,
    "median": 68.5,
    "average": 68.5,
    "max": 72.0
  },
  "memory_usage_percent": {
    "minimum": 85.0,
    "median": 87.5,
    "average": 87.5,
    "max": 90.0
  }
}
```

### Application Logs (application.json)
```json
{
  "INFO": 3,
  "ERROR": 2,
  "DEBUG": 1,
  "WARNING": 1
}
```

### Request Logs (request.json)
```json
{
  "/api/update": {
    "response_times": {
      "min": 200,
      "50_percentile": 200,
      "90_percentile": 200,
      "95_percentile": 200,
      "99_percentile": 200,
      "max": 200
    },
    "status_codes": {
      "2XX": 1,
      "4XX": 0,
      "5XX": 0
    }
  }
}
```

## Test Results

All 14 tests have been successfully implemented and passed:

- LogProcessorFactory tests: ✅
- APMLogProcessor tests: ✅
- ApplicationLogProcessor tests: ✅
- RequestLogProcessor tests: ✅
- APMLogAggregator tests: ✅
- ApplicationLogAggregator tests: ✅
- RequestLogAggregator tests: ✅
- LogApp tests: ✅

The tests verify:
- Correct log classification and parsing
- Handling of invalid log entries
- Accurate statistical calculations
- Proper aggregation of metrics

## Dependencies
- Java 21 or higher
- Gson 2.8.9 (for JSON processing)
- JUnit 5.8.2 (for testing)
