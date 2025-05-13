# SLOG - Simple Logger Class Documentation

## Overview
`SLOG` is a lightweight C++ utility for logging messages with various severity levels (DEBUG, INFO, WARNING, ERROR, FATAL).

## Logging Levels
The class uses the `COLOR` enumeration to specify console colors:
- **DEBUG**: For debugging information.
- **INFO**: For general information.
- **WARNING**: For warnings.
- **ERROR**: For errors.
- **FATAL**: For critical errors.

## Methods

### Logging Methods
Each logging level has two variants: `_NE` (for C-style strings with variable arguments) and without a suffix (for `std::string` strings).

#### `DEBUG_NE(const char* str, ...) noexcept`
- **Description**: Outputs a debug message to the console in blue and, if a file is open, writes it to the file with a timestamp.
- **Parameters**:
  - `str`: C-style string with the message format (similar to `printf`).
  - `...`: Variable number of arguments for string formatting.
- **Example**:
  ```cpp
  LOG log;
  log.DEBUG_NE("User %s logged in with ID %d", "Alice", 123);
  ```
  **Console Output**: `[DEBUG]: User Alice logged in with ID 123`  
  **File Output** (if open): `(09:41:23)[DEBUG]: User Alice logged in with ID 123`

#### `DEBUG(std::string str)`
- **Description**: Outputs a debug message from a `std::string` to the console in blue and, if a file is open, writes it to the file.
- **Parameters**:
  - `str`: String containing the message.
- **Example**:
  ```cpp
  log.DEBUG("Starting initialization");
  ```
  **Console Output**: `[DEBUG]: Starting initialization`  
  **File Output**: `(09:41:23)[DEBUG]: Starting initialization`

#### `INFO_NE(const char* str, ...) noexcept`
- **Description**: Outputs an informational message to the console in cyan and, if a file is open, writes it to the file.
- **Parameters**: Similar to `DEBUG_NE`.
- **Example**:
  ```cpp
  log.INFO_NE("Processing %d records", 500);
  ```

#### `INFO(std::string str)`
- **Description**: Outputs an informational message from a `std::string` to the console in cyan.
- **Parameters**: Similar to `DEBUG`.
- **Example**:
  ```cpp
  log.INFO("Operation completed");
  ```

#### `WARNING_NE(const char* str, ...) noexcept`
- **Description**: Outputs a warning message to the console in orange.
- **Parameters**: Similar to `DEBUG_NE`.
- **Example**:
  ```cpp
  log.WARNING_NE("Low memory: %d MB left", 256);
  ```

#### `WARNING(std::string str)`
- **Description**: Outputs a warning message from a `std::string` to the console in orange.
- **Parameters**: Similar to `DEBUG`.
- **Example**:
  ```cpp
  log.WARNING("Configuration file not found");
  ```

#### `ERROR_NE(const char* str, ...) noexcept`
- **Description**: Outputs an error message to the console in red.
- **Parameters**: Similar to `DEBUG_NE`.
- **Example**:
  ```cpp
  log.ERROR_NE("Failed to open file %s", "config.txt");
  ```

#### `ERROR(std::string str)`
- **Description**: Outputs an error message from a `std::string` to the console in red.
- **Parameters**: Similar to `DEBUG`.
- **Example**:
  ```cpp
  log.ERROR("Database connection lost");
  ```

#### `FATAL_NE(const char* str, ...) noexcept`
- **Description**: Outputs a critical error message to the console in dark red.
- **Parameters**: Similar to `DEBUG_NE`.
- **Example**:
  ```cpp
  log.FATAL_NE("System crash at module %s", "core");
  ```

#### `FATAL(std::string str)`
- **Description**: Outputs a critical error message from a `std::string` to the console in dark red.
- **Parameters**: Similar to `DEBUG`.
- **Example**:
  ```cpp
  log.FATAL("Critical failure detected");
  ```

### File Management Methods

#### `bool new_open(std::string filename)`
- **Description**: Opens a new file for logging, overwriting an existing file if it exists.
- **Parameters**:
  - `filename`: Name of the file for logging.
- **Return Value**: `true` if the file is successfully opened, `false` otherwise.
- **Example**:
  ```cpp
  if (log.new_open("mylog.log")) {
      log.INFO("Log file created");
  }
  ```

#### `bool open(std::string filename)`
- **Description**: Opens a file for appending logs (append mode).
- **Parameters**:
  - `filename`: Name of the file for logging.
- **Return Value**: `true` if the file is successfully opened, `false` otherwise.
- **Example**:
  ```cpp
  if (log.open("mylog.log")) {
      log.INFO("Appending to log file");
  }
  ```

#### `bool auto_open()`
- **Description**: Opens a file for appending logs with an automatically generated name in the format `logging_DD_MM_YYYY.log`, where `DD`, `MM`, `YYYY` are the current day, month, and year.
- **Return Value**: `true` if the file is successfully opened, `false` otherwise.
- **Example**:
  ```cpp
  if (log.auto_open()) {
      log.INFO("Auto-opened log file");
  }
  ```
  **Example File Name**: `logging_13_05_2025.log`

#### `void close()`
- **Description**: Closes the open log file.
- **Example**:
  ```cpp
  log.close();
  ```

## Usage Examples

### Example 1: Logging to Console
```cpp
#include "slog.hpp"

int main() {
    LOG log;
    log.DEBUG("Debug message");
    log.INFO_NE("Processing %d items", 42);
    log.WARNING("Potential issue detected");
    log.ERROR_NE("Error in module %s", "network");
    log.FATAL("System shutdown required");
    return 0;
}
```
**Console Output**:
```
[DEBUG]: Debug message
[INFO]: Processing 42 items
[WARNING]: Potential issue detected
[ERROR]: Error in module network
[FATAL]: System shutdown required
```

### Example 2: Logging to File
```cpp
#include "slog.hpp"

int main() {
    LOG log;
    if (log.auto_open()) {
        log.INFO("Program started");
        log.ERROR_NE("Failed to load resource %s", "image.png");
        log.close();
    }
    return 0;
}
```
**File Content** (e.g., `logging_13_05_2025.log`):
```
(09:41:23)[INFO]: Program started
(09:41:23)[ERROR]: Failed to load resource image.png
```

## Notes
- Methods with the `_NE` suffix are marked as `noexcept`, ensuring no exceptions are thrown.
- For proper color display in the console, a terminal with ANSI code support is required (e.g., most modern Linux/Mac terminals or Windows Terminal).
- If no file is open, messages are output only to the console.
