# How to upload test results to the Qase

## Requirements

- .NET Core 8.0 or higher
- Docker

## Prepare test project

1. Add an external package for saving test results to Junit format

    ```bash
    dotnet add package JunitXml.TestLogger
    ```

2. Restore packages

    ```bash
    dotnet restore
    ```

3. Run tests with the following command

    ```bash
    dotnet test --logger:"junit;LogFilePath=results/test-results.xml"
    ```

We got the `test-results.xml` file with test results in the Junit format. Now we need to upload it to the Qase.

## Install Qase TMS CLI

1. Download the latest version of the Qase CLI to the Docker container

    ```bash
    docker pull ghcr.io/qase-tms/qase-cli:latest
    ```

2. Test the CLI

    ```bash
    docker run --rm ghcr.io/qase-tms/qase-cli:latest version
    ```

## Upload test results to the Qase

Run the following command to upload the test results to the Qase

```bash
docker run --rm -v $(pwd)/results:/results ghcr.io/qase-tms/qase-cli:latest testops result upload --project DEMO --token <token> --title "Nunit test run" --format junit --path ./results/test-results.xml --verbose
```

Where:

- `--project` - the project code in the Qase
- `--token` - the token from the Qase
- `--title` - the title of the test run
- `--format` - the format of the test results
- `--path` - the path to the test results file
