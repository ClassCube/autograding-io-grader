## GitHub Classroom IO Grader

### Overview
**GitHub Classroom IO Grader** is a plugin for GitHub Classroom's Autograder. Use it to ensure student executables output the correct values on tests.

### Key Features
- **Automatic Grading**: Test student code submissions and provide immediate feedback.
- **Customizable Test Setup**: Define pre-test setup commands and specific testing commands.
- **Flexible Output Comparison**: Supports multiple methods to compare the stdout output.
- **Timeout Control**: Limit the runtime of tests to prevent excessive resource usage.

### Inputs

| Input Name | Description | Required |
|------------|-------------|----------|
| `test-name` | The unique identifier for the test. | Yes |
| `setup-command` | Command to execute prior to the test, typically for environment setup or dependency installation.| No |
| `command` | The main command executed during the test. It receives input via stdin (if provided) and its output is evaluated against `expected-output` based on the `comparison-method`. | Yes |
| `input` | The input data that will be passed to the command via stdin. | No |
| `expected-output` | The expected output that the command should print to stdout. | Yes |
| `comparison-method` | Defines how the stdout output will be compared. Supported values are `included`, `exact`, `regex`, and `loose`. | Yes |
| `timeout` | Duration (in minutes) before the test is terminated. Defaults to 10 minutes with a maximum limit of 6 hours.| No |
| `max-score` | The maximum amount of points a student can receive for this test.| No |

#### Loose Options

If you're using the `loose` comparison, the following options are available. 

| Option | Description | Default | Required |
| --- | --- | --- | --- | 
| `loose-trim` | If true, both sides of each line are trimmed before comparing | false | ➖ |
| `loose-r-trim` | If true, only the right side of each line is trimmed | false | ➖ |
| `loose-l-trim` | If true, only the left side of each line is trimmed | false | ➖ |
| `loose-ignore-blank-lines` | Whether blank lines should be considered when comparing results | true | ➖ |
| `loose-squash-spaces ` | If true, multiple spaces are squashed down to a single space before comparisons are made | true | ➖ |




### Outputs

| Output Name | Description |
|-------------|-------------|
| `result` | Outputs the result of the grader, indicating the success or failure of the test. |

### Usage

1. Add the GitHub Classroom IO Grader action to your workflow.

```yaml
name: Autograding Tests

on:
  push

jobs:
  run-tests:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    - name: Run Autograding Tests
      uses: compscirocks/autograding-io-grader@main
      with:
        test-name: 'Test Name'
        command: './bin/shout'
        input: 'hello'
        expected-output: 'HELLO'
        comparison-method: 'exact'
    - name: Autograding Reporter
      uses: ...
