# ECOSS Test Automation Framework (ECOSS-TAF)

A comprehensive, enterprise-grade test automation framework designed for the ECOSS ecommerce project, supporting both API and UI testing across multiple platforms.

## Overview

ECOSS-TAF is a modular, technology-agnostic testing framework built with industry best practices. It provides robust support for:
- **API Testing**: RestAssured and Playwright-based API automation
- **UI Testing**: Playwright and Selenium-based browser automation
- **Multi-Application Support**: Seamless testing across Cookiedent and Twix applications
- **Advanced Reporting**: Allure integration for detailed test execution reports
- **Infrastructure Integration**: CI/CD ready with Jenkins and local execution support
- **Data Management**: JSON-driven test data with placeholder substitution

---

## Tech Stack

| Component | Technology | Version |
|-----------|-----------|---------|
| **Test Framework** | TestNG | 7.10.1 |
| **API Testing** | RestAssured | 5.4.0 |
| **Browser Automation** | Playwright | 1.48.0 |
| **Alternative Web Driver** | Selenium | 4.28.1 |
| **Reporting** | Allure | 2.24.0 |
| **Build Tool** | Maven | 3.13.0 |
| **Logging** | SLF4J + Logback | 2.0.9 / 1.4.11 |
| **JSON Processing** | Jackson | 2.15.2 |
| **Data Models** | Lombok | 1.18.30 |
| **Excel Support** | Apache POI | 5.2.5 |
| **Database** | Azure Cosmos DB | 4.67.0 |
| **JDK** | Java | 17 |

---

## Project Structure

```
TAF-ECOSS/
├── src/
│   ├── main/java/com/automation/framework/
│   │   ├── ui_pages/
│   │   │   └── playwright/
│   │   │       ├── cookiedent/          # Cookiedent UI page objects
│   │   │       └── twix/                # Twix UI page objects
│   │   └── utils/
│   │       ├── api/
│   │       │   ├── restassured/         # RestAssured API utilities
│   │       │   ├── playwright/          # Playwright API utilities
│   │       │   └── common/              # Shared API utilities
│   │       ├── ui/
│   │       │   ├── playwright/          # Playwright UI utilities
│   │       │   ├── selenium/            # Selenium UI utilities
│   │       │   └── core/                # Common UI utilities
│   │       └── common/
│   │           ├── ConfigPropertyReader.java
│   │           ├── JsonReader.java
│   │           ├── JsonSaveToFile.java
│   │           └── AllureLogger.java
│   │
│   └── test/java/com/automation/framework/
│       └── sprint_two/regression/
│           ├── api_tests/              # API test classes
│           └── ui_tests/               # UI test classes
│
├── src/test/resources/
│   ├── config.properties               # General configuration
│   ├── playwright-config.properties    # Playwright settings
│   ├── configs/
│   │   └── ecos.properties            # ECOSS-specific URLs & headers
│   ├── data/
│   │   ├── api/                        # API test data
│   │   └── ui/
│   │       ├── okta_login.json         # Login credentials
│   │       └── product-data-73973001.json  # Product test data
│   └── suites/
│       ├── api/sprint_two/             # API test suites
│       └── ui/sprint_two/              # UI test suites
│
└── pom.xml                             # Maven configuration
```

---

## Key Features

### 1. **Multi-Technology Support**
- **Playwright**: Modern browser automation with API support
- **Selenium**: Traditional browser automation with WebDriver protocol
- **RestAssured**: Fluent API testing library with advanced validations

### 2. **Organized Test Structure**
- **Page Object Model (POM)**: Separated page objects for maintainability
- **Base Classes**: Reusable test base classes for API and UI
- **Utility Layers**: Independent utility modules for API, UI, and common operations

### 3. **Configuration Management**
- Property-based configuration with caching
- Multi-environment support (QA, local, etc.)
- Tenant domain configuration for multi-tenant testing
- Browser parameter configuration via TestNG XML

### 4. **Test Data Management**
- JSON-based test data files
- Placeholder substitution for dynamic data
- Direct JSON value fetching with JSONPath
- Model deserialization for type-safe data handling

### 5. **Reporting & Logging**
- Allure reporting with rich attachments
- Request/Response logging to Allure reports
- SLF4J structured logging
- Test execution summaries and metrics

### 6. **CI/CD Integration**
- Jenkins-ready with Freestyle job support
- Maven Surefire plugin for test execution
- XML suite file parameterization
- Allure results cleanup automation

---

## Installation & Setup

### Prerequisites
- **Java 17** or higher
- **Maven 3.8.0** or higher
- **Git** for version control
- Modern browser (Chrome/Chromium) for UI tests

### Local Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd ECOSS-TAF
   ```

2. **Install dependencies**
   ```bash
   mvn clean install
   ```

3. **Configure environment**
   - Update `src/test/resources/configs/ecos.properties` with your environment URLs
   - Update `src/test/resources/playwright-config.properties` for browser settings
   - Place credentials in `src/test/resources/data/ui/okta_login.json`

---

## Usage & Execution

### Command-Line Execution

#### Run Specific API Test Suite
```bash
Remove-Item -Recurse -Force .\allure-results
mvn test "-DsuiteXmlFile=src/test/resources/suites/api/sprint_two/general_api_test.xml"
```

#### Run Specific UI Test Suite
```bash
Remove-Item -Recurse -Force .\allure-results
mvn test "-DsuiteXmlFile=src/test/resources/suites/ui/sprint_two/cart-tests.xml"
```

#### Run All Tests
```bash
Remove-Item -Recurse -Force .\allure-results
mvn clean test
```

### Jenkins Execution

1. **Create a Freestyle Job** in Jenkins
2. **Configure Source Code**
   - Repository URL: Your Git repository
   - Branch: Desired branch (main/develop)

3. **Build Step Command**
   ```bash
   Remove-Item -Recurse -Force .\allure-results
   mvn test "-DsuiteXmlFile=src/test/resources/suites/api/sprint_two/general_api_test.xml"
   ```

4. **Post-Build Action**
   - Allure Plugin configuration
   - Point to `allure-results` directory

### Docker Execution

For containerized environments:
```bash
docker run -v $(pwd):/workspace maven:3.8-openjdk-17 \
  sh -c "cd /workspace && mvn clean test -DsuiteXmlFile=..."
```

---

## Test Suites

TestNG XML suite files define test execution scope and parameters.

### Available Suites

**API Test Suites:**
- `general_api_test.xml` - Core API functionality tests
- `product_api_exception_handling.xml` - Error handling scenarios

**UI Test Suites:**
- `products-page-tests.xml` - Product listing page tests
- `product-details-page-tests.xml` - Individual product details tests
- `cart-tests.xml` - Shopping cart functionality
- `all-ecos-tests.xml` - Comprehensive regression suite
- `fullflow.xml` - End-to-end user journeys
- `twixfull.xml` - Twix application full flow

### Custom Suite Creation

Create new XML suite files in `src/test/resources/suites/` following TestNG DTD structure:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="Custom Suite">
    <test name="Test Group">
        <parameter name="browser" value="chromium" />
        <parameter name="headless" value="true" />
        <classes>
            <class name="com.automation.framework.sprint_two.regression.api_tests.YourTestClass" />
        </classes>
    </test>
</suite>
```

---

## Configuration

### Environment Setup (ecos.properties)

```properties
# Frontend URLs
cookiedent.frontend.base.url.qa = https://ecospoc-cfbuhxase3fchke3.z02.azurefd.net/
twix.frontend.base.url.qa = https://ecospoc-twix-eqftcdbca0dha2hu.z02.azurefd.net/en/

# Backend API URLs
poc.backend.base.url = http://localhost:8000
poc.backend.products.end.url = /api/products/paginated

# Multi-Tenant Configuration
poc.header.tenant.domain.key = X-Ecos-Tenant-Domain
poc.cookiedent.tenant.domain.value = cookiedent.local
poc.twix.tenant.domain.value = twix.local
```

### Browser Configuration (playwright-config.properties)

```properties
# Browser settings
playwright.viewport.width=1280
playwright.viewport.height=720

# Timeouts (milliseconds)
playwright.timeout.default=30000

# Reporting
playwright.screenshot.on.failure=true
playwright.video.enabled=false
```

---

## Test Data Management

### JSON-Based Data Files

Test data is stored in JSON format for flexibility and maintainability:

```json
{
  "SKU": 73973001,
  "Category": "Dental alloy",
  "Color": "grau",
  "Indication": "Aufbrennlegierung"
}
```

### Accessing Test Data

```java
// Read entire JSON file
String userData = JsonReader.readStaticJsonFile("src/test/resources/data/ui/okta_login.json");

// Fetch specific value by JSONPath
Object username = JsonReader.fetchJsonValueByKey("okta_login.json", "$.username");

// Load with placeholder substitution
Map<String, String> placeholders = new HashMap<>();
placeholders.put("{{PRODUCT_ID}}", "12345");
String data = JsonReader.loadAndReplaceJsonPlaceholders("product-data.json", placeholders);

// Deserialize to model class
ProductDetailsModel product = JsonReader.readJsonFileOnce("product-data.json", ProductDetailsModel.class);
```

---

## Framework Components

### Base Classes

**API Testing Base:**
- `PW_API_BaseTest` - Playwright API test base with browser context
- `RA_API_Utils` - RestAssured utility methods

**UI Testing Base:**
- Playwright UI utilities for page interactions
- Selenium WebDriver utilities for legacy support

### Utility Classes

| Utility | Purpose |
|---------|---------|
| `ConfigPropertyReader` | Property file management with caching |
| `JsonReader` | JSON file operations with JSONPath support |
| `JsonSaveToFile` | Persist JSON objects to files |
| `AllureLogger` | Structured logging to Allure reports |
| `RA_API_Utils` | RestAssured API operations |
| `RA_ResponseHelpers` | Response assertion and validation |
| `PW_API_Utils` | Playwright API testing |
| `JsonUtils` | JSON transformation and validation |

### Page Objects

Organized by application and feature:
- **Cookiedent Pages**: Login, Products, Cart, Checkout
- **Twix Pages**: Landing, Products, Cart, Product Details

---

## CI/CD Integration

### Jenkins Pipeline Example

```groovy
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://your-repo-url'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn clean test -DsuiteXmlFile=src/test/resources/suites/api/sprint_two/general_api_test.xml'
            }
        }
        
        stage('Generate Report') {
            steps {
                allure includeProperties: true,
                       jdk: '',
                       results: [[path: 'allure-results']]
            }
        }
    }
    
    post {
        always {
            junit 'target/surefire-reports/**/*.xml'
            archiveArtifacts 'allure-results/**/*'
        }
    }
}
```

---

## Reporting

### Allure Reports

Generate and view Allure reports:

```bash
# Generate report from existing results
mvn allure:report

# Open report in browser (Results in target/site/allure-report/)
mvn allure:serve
```

Reports include:
- Test execution status and timeline
- Request/Response details for API tests
- Screenshots for UI test failures
- Detailed step-by-step execution logs
- Test history and trends

---

## Best Practices

### Test Development
1. **Use Page Object Model** - Separate page logic from test logic
2. **Data-Driven Testing** - Leverage JSON test data files
3. **Meaningful Assertions** - Use helper assertion methods
4. **Proper Logging** - Log important steps via AllureLogger
5. **Configuration Over Hardcoding** - Use properties files

### Test Execution
1. **Clean Before Run** - Remove old allure-results: `Remove-Item -Recurse -Force .\allure-results`
2. **Specific Suite Selection** - Run targeted suites, not everything
3. **Headless in CI/CD** - Use headless mode for pipeline execution
4. **Screenshot on Failure** - Enable for UI tests, useful for debugging

### Maintenance
1. **Update Configuration** - Keep ecos.properties in sync with environments
2. **Version Control** - Commit all test code and configuration
3. **Document Complex Tests** - Use Allure @Description annotations
4. **Regular Cleanup** - Archive old allure-results periodically

---

## Troubleshooting

### Common Issues

**Issue: Maven build fails with dependency errors**
- Solution: Run `mvn clean install -U` to update dependencies

**Issue: Tests timeout on slow networks**
- Solution: Increase timeouts in playwright-config.properties

**Issue: Configuration file not found**
- Solution: Ensure files are in `src/test/resources/` and classpath is correct

**Issue: Allure report not generating**
- Solution: Verify `allure-maven` plugin version and run `mvn allure:report`

**Issue: Browser not launching in UI tests**
- Solution: Ensure Playwright browsers are installed: Run tests once with headless=false for installation

### Debug Mode

Enable verbose logging:
```bash
mvn test -X -DsuiteXmlFile=...
```

---

## Contributing

### Guidelines
1. Follow existing code structure and naming conventions
2. Add tests for new features
3. Update relevant configuration/data files
4. Keep framework utilities generic and reusable
5. Document non-obvious implementation details

### Code Review Checklist
- [ ] No hardcoded URLs or credentials
- [ ] Uses property files for configuration
- [ ] Follows POM pattern for UI pages
- [ ] Includes Allure logging/attachments
- [ ] Tests are independent and repeatable

---

## Support & Contact

For framework issues, questions, or enhancements:
- Review existing test suites as examples
- Check logs in `allure-results/` directory
- Refer to TestNG and Playwright official documentation

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2024 | Initial framework release with API/UI support |

---

## License

[Add your license information here]
