
# Bruno API Tests + GitHub Integration

This repository contains Bruno API tests along with a seamless integration with GitHub. Bruno is a powerful API testing framework that enables running API tests locally or as part of your CI/CD pipelines.

## Features
- Easily manage and run API tests using **Bruno CLI**.
- Generate detailed **HTML reports**.
- Support for running tests in multiple environments (e.g., `dev`, `staging`).
- Integrate with GitHub Actions for continuous testing.

## Prerequisites
- **Node.js** (LTS version recommended)
- **Bruno CLI**: No need for a separate installation, as it can be executed using `npx`.

## Running Tests Locally
To execute Bruno API tests locally, follow these steps:

1. Clone the repository:
   ```bash
   git clone https://github.com/grajk88/bruno-github-integration.git
   cd collection
   ```

2. Run the tests with the following command:
   ```bash
   npx @usebruno/cli run --env dev --reporter-html ./reports/Results.html
   ```

   - `--env dev`: Specifies the environment to run the tests (`dev`, `staging`, `production`, etc.).
   - `--reporter-html`: Generates an HTML report at the specified path (`./reports/Results.html`).

3. View the generated test report in your browser by opening `./reports/Results.html`.

## GitHub Integration
This repository is integrated with GitHub for continuous testing. Every pull request or commit to specific branches (e.g., `main`) will trigger the Bruno API tests automatically.

### GitHub Actions Workflow
The `.github/workflows/Tests.yml` file defines the workflow for running Bruno API tests. It includes steps such as:
1. Checkout the code.
2. Install Node.js.
3. Run Bruno API tests.
4. Upload the test results.

### Example Workflow Configuration:
```yaml
name: Run Bruno Tests

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  bruno-test:
    name: Run Bruno CLI Tests
    runs-on: ubuntu-latest

    steps:
      # Checkout the repository
      - name: Checkout Code
        uses: actions/checkout@v3

      # Set up Node.js environment
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18' # Adjust Node version as needed

      # Install Bruno CLI dependencies
      - name: Install Dependencies
        run: |
          npm install

      # Run Bruno CLI tests with Production environment and HTML reporting
      - name: Run Bruno Tests
        run: |
          cd collection
          npx @usebruno/cli run --env Production --reporter-json ../collection/reports/results.json --reporter-html ../collection/reports/Results.html

      # Upload the test report as an artifact
      - name: Upload Test Report
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: Bruno Test Results
          path: ./collection/reports/Results.html

      # (Optional) Post-run: Summary output
      - name: Display Test Run Summary
        if: always()
        run: echo "Bruno tests completed. HTML report is generated and uploaded as an artifact."
```

## Repository Structure
```
/tests                # Bruno test collections
/environments         # Environment-specific configurations
/reports              # Generated test reports
```

## License
This project is licensed under the [MIT License](LICENSE).