# Hotel Management System - Performance Testing with JMeter

## Overview
This document provides instructions for running comprehensive performance tests on the Hotel Management System using Apache JMeter.

## Test Plan Overview

The `Hotel_Management_Full_Performance_Test.jmx` test plan includes:

### Test Scenarios Covered:
1. **Home Page Access (Light Load)**
   - 50 concurrent users
   - 20 loops per user
   - 30-second ramp-up time
   - Tests: GET index.php

2. **User Login (Normal Load)**
   - 30 concurrent users
   - 10 loops per user
   - 20-second ramp-up time
   - Tests: POST user login

3. **User Signup (Medium Load)**
   - 20 concurrent users
   - 5 loops per user
   - 15-second ramp-up time
   - Tests: POST user registration

4. **Staff Login**
   - 10 concurrent users
   - 10 loops per user
   - 10-second ramp-up time
   - Tests: POST staff login

5. **Room Booking (Stress Test)**
   - 40 concurrent users
   - 5 loops per user
   - 25-second ramp-up time
   - Tests: POST room booking form

6. **Admin Dashboard Access**
   - 15 concurrent users
   - 15 loops per user
   - 10-second ramp-up time
   - Tests: GET admin dashboard, roombook, room, staff pages

7. **Stress Test (High Load)**
   - 100 concurrent users
   - 10 loops per user
   - 30-second ramp-up time
   - Tests: Random requests to home page and home.php

## Prerequisites

1. **Apache JMeter** (Version 5.6.3 or higher)
   - Download from: https://jmeter.apache.org/download_jmeter.cgi
   - Extract to a directory (e.g., `C:\apache-jmeter-5.6.3`)

2. **XAMPP or Web Server**
   - Ensure the hotel-management-docker application is running
   - Default: http://localhost:80/hotel-management-docker

3. **Java Runtime Environment (JRE)**
   - JMeter requires Java 8 or higher
   - Check with: `java -version`

## Running the Tests

### Option 1: Using Batch Script (Windows)

1. Update the JMeter path in `run_full_performance_test.bat`:
   ```batch
   set JMETER_HOME=C:\apache-jmeter-5.6.3
   ```

2. Double-click `run_full_performance_test.bat` or run from command prompt:
   ```batch
   run_full_performance_test.bat
   ```

3. The script will:
   - Run all performance tests
   - Generate CSV results
   - Create an HTML report
   - Open the report in your browser

### Option 2: Using Shell Script (Linux/Mac)

1. Make the script executable:
   ```bash
   chmod +x run_full_performance_test.sh
   ```

2. Update the JMeter path in `run_full_performance_test.sh`:
   ```bash
   JMETER_HOME="/opt/apache-jmeter-5.6.3"
   ```

3. Run the script:
   ```bash
   ./run_full_performance_test.sh
   ```

### Option 3: Manual Command Line

#### Windows:
```batch
jmeter -n -t Hotel_Management_Full_Performance_Test.jmx ^
    -l jmeter_results/performance_test_results.jtl ^
    -e -o jmeter_results/html_report ^
    -J HOST=localhost ^
    -J PORT=80 ^
    -J PROTOCOL=http ^
    -J PATH=hotel-management-docker
```

#### Linux/Mac:
```bash
jmeter -n -t Hotel_Management_Full_Performance_Test.jmx \
    -l jmeter_results/performance_test_results.jtl \
    -e -o jmeter_results/html_report \
    -J HOST=localhost \
    -J PORT=80 \
    -J PROTOCOL=http \
    -J PATH=hotel-management-docker
```

### Option 4: GUI Mode (For Testing/Development)

1. Open JMeter GUI:
   ```batch
   jmeter.bat
   ```

2. Load the test plan:
   - File → Open → Select `Hotel_Management_Full_Performance_Test.jmx`

3. Configure variables (if needed):
   - Right-click Test Plan → User Defined Variables
   - Modify HOST, PORT, PROTOCOL, PATH if needed

4. Run the test:
   - Click Run → Start (or press Ctrl+R)

## Test Configuration Parameters

You can customize test parameters using JMeter properties:

| Parameter | Default | Description |
|-----------|---------|-------------|
| HOST | localhost | Server hostname or IP |
| PORT | 80 | Server port |
| PROTOCOL | http | Protocol (http/https) |
| PATH | hotel-management-docker | Application path |

### Example: Testing Remote Server
```batch
jmeter -n -t Hotel_Management_Full_Performance_Test.jmx ^
    -l results.jtl ^
    -e -o html_report ^
    -J HOST=192.168.1.100 ^
    -J PORT=8080
```

## Understanding Results

### Output Files

1. **performance_test_results.jtl**
   - Detailed results in CSV format
   - Contains all request/response data

2. **html_report/**
   - Comprehensive HTML report with:
     - Summary statistics
     - Response time graphs
     - Throughput graphs
     - Error analysis
     - Response codes distribution

3. **jmeter.log**
   - Execution log
   - Useful for debugging issues

### Key Metrics to Monitor

1. **Response Time**
   - Average: Should be < 2 seconds for most requests
   - 90th percentile: Should be < 5 seconds
   - Max: Identify outliers

2. **Throughput**
   - Requests per second
   - Higher is better (indicates better performance)

3. **Error Rate**
   - Percentage of failed requests
   - Should be < 1% for good performance

4. **Response Codes**
   - 200: Success
   - 302: Redirect (normal for login/logout)
   - 404: Page not found (error)
   - 500: Server error (critical)

### Expected Performance Targets

| Endpoint | Target Response Time | Target Throughput |
|----------|---------------------|-------------------|
| Home Page | < 500ms | > 100 req/sec |
| User Login | < 1s | > 50 req/sec |
| User Signup | < 1.5s | > 30 req/sec |
| Room Booking | < 2s | > 25 req/sec |
| Admin Dashboard | < 1s | > 40 req/sec |

## Troubleshooting

### Common Issues

1. **JMeter not found**
   - Verify JMeter installation path
   - Add JMeter bin directory to PATH

2. **Connection refused**
   - Ensure XAMPP/webserver is running
   - Check HOST and PORT settings

3. **High error rate**
   - Check server logs
   - Verify database connection
   - Increase ramp-up time for gradual load

4. **Out of memory errors**
   - Increase JVM heap size:
     ```batch
     set HEAP=-Xms1g -Xmx4g -XX:MaxMetaspaceSize=256m
     jmeter.bat -n -t test.jmx ...
     ```

## Modifying Test Plan

### To Modify Load Parameters:

1. Open test plan in JMeter GUI
2. Select Thread Group
3. Modify:
   - **Number of Threads**: Concurrent users
   - **Ramp-up Period**: Time to start all threads
   - **Loop Count**: Number of iterations

### To Add New Test Scenarios:

1. Right-click Test Plan → Add → Thread Group
2. Add HTTP Request samplers
3. Configure request parameters
4. Add assertions for validation

## Best Practices

1. **Start Small**: Begin with low user counts and gradually increase
2. **Monitor Resources**: Watch CPU, memory, and database during tests
3. **Run Multiple Times**: Perform multiple test runs for consistency
4. **Test Environment**: Use a dedicated test environment matching production
5. **Baseline Tests**: Create baseline metrics before optimization
6. **Document Results**: Keep records of all test runs for comparison

## Additional Resources

- JMeter Documentation: https://jmeter.apache.org/usermanual/
- JMeter Best Practices: https://jmeter.apache.org/usermanual/best-practices.html
- Performance Testing Guide: https://jmeter.apache.org/usermanual/test_plan.html

## Support

For issues or questions:
1. Check JMeter logs in `jmeter_results/jmeter_*.log`
2. Review server logs (XAMPP/webserver)
3. Verify database connectivity
4. Check network connectivity

---

**Note**: Ensure your test environment has sufficient resources and is isolated from production systems to avoid impacting real users.
