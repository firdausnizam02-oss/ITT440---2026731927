# MUHAMMAD FIRDAUS BIN KHAIRON NIZAM

**Course:** ITT440 - Network Programming  
**Group:** NBCS2555B  
**Name:** Muhammad Firdaus Bin Khairon Nizam
**Student ID:** 2026731927 
**Lecturer:** Sir Shahadan Bin Saad

---

## 1. INTRODUCTION

Performance testing is a fundamental practice for evaluating the reliability, responsiveness, and scalability of modern web applications. As web-based platforms increasingly handle concurrent user interactions such as flight searches, form submissions, and online transactions, understanding how these systems behave under varying levels of demand becomes essential for delivering a seamless user experience. This project focuses on analyzing the performance of a demo travel booking website, specifically BlazeDemo (https://blazedemo.com), which simulates a real-world flight reservation process. Unlike traditional performance testing approaches that target static pages or simple endpoints, this study incorporates a complete user journey involving dynamic interactions such as selecting departure and arrival cities, choosing a flight from a list, and completing a purchase form. The main objectives of this study are:

* To evaluate system performance under expected normal user loads through load testing

* To determine the application's breaking point by progressively increasing user concurrency beyond normal limits

* To assess system behavior during sudden, unexpected traffic surges using spike testing

Through these tests, this project aims to identify performance bottlenecks, measure response times, and provide insights into the application's overall stability and capacity for handling real-world usage patterns.

### Main Objectives:

- To evaluate API performance under increasing user load
- To analyze system behavior under sudden traffic spikes
- To assess long-term stability through endurance testing

---
## 2.METHODOLOGY
This section outlines the systematic approach taken to conduct performance testing and bottleneck analysis on the BlazeDemo web application using Apache JMeter. The methodology is divided into five phases: test environment setup, test script development, test scenario design, test execution, and results analysis.

### 2.1 Test Environment Setup
Before conducting any performance tests, the testing environment was properly configured to ensure accurate and repeatable results. The following tools and specifications were used:

| Component | Specification |
| :---         |     :---:      |
| Testing Tool |	Apache JMeter (latest version) |
|Target Application	| BlazeDemo (https://blazedemo.com) |
|Operating System |	macOS / Windows / Linux (depending on available setup) |
| Network |	Stable internet connection |
| Listeners	| View Results Tree, Summary Report, Aggregate Report |

Additionally, necessary JMeter plugins were installed, and the system was verified to have sufficient memory and processing power to simulate the required number of virtual users without becoming a bottleneck itself.


### 2.2. Test Script Development 
A realistic user journey was scripted in JMeter to accurately simulate how a real user would interact with the BlazeDemo website. The complete end-to-end workflow consisted of the following sequential HTTP requests:

| Step | Action | Endpoint/Path |
| :---         |     :---:      |     :---:      |
|1	| Access the homepage |	/ |
|2	| Select departure and arrival cities	| /reserve.php |
|3	| Choose a specific flight | /purchase.php |
|4	|Fill in passenger and payment details	|/purchase.php (POST)|
|5	|Confirm booking and receive confirmation	|/confirmation.php|

The script was developed using two methods:

* BlazeMeter Chrome Extension: Used to record the complete user journey and export it as a .jmx file.

* Manual Construction: HTTP Request samplers were manually added and configured in JMeter for greater control and understanding.

To simulate realistic user behavior, think times (pauses between actions) were added using the Constant Timer or Gaussian Random Timer in JMeter.

### 2.3 Test Scenario Design
Three distinct performance test scenarios were designed to evaluate different aspects of the application's behavior under load. Each scenario was configured using JMeter's Thread Group with specific parameters.

#### 2.3.1 Load Test - 
The load test was designed to simulate expected normal operating conditions. The goal was to establish a baseline performance metric.
     
| Label | Avg Response (ms) | 90th pct (ms) | 95th pct (ms) | 99th pct (ms) | Max (ms) | Error % | Status |
|-------|-------------------|----------------|----------------|----------------|----------|---------|--------|
| Homepage | 374.40 | 443.80 | 489.45 | 884.31 | 1154 | 0.00% | ⚠️ Slowest step |
| Find Flights | 360.72 | 420.80 | 454.60 | 570.80 | 709 | 0.00% | ✅ Best performance |
| Choose Flight | 366.45 | 425.90 | 472.60 | 711.89 | 838 | 0.00% | ✅ Good |
| Complete Booking | 362.47 | 404.00 | 437.85 | 702.62 | 1020 | 0.00% | ✅ Best 90th pct |

### Overall Statistics

| Metric | Value |
|--------|-------|
| Total Requests | 1000 |
| Error Rate | 0.00% ✅ |
| Average Response | 366.01 ms |
| Min Response | 292 ms |
| Max Response | 1154 ms (1.15 sec) |
| Median | 350.00 ms |
| 90th Percentile | 421.00 ms |
| 95th Percentile | 458.95 ms |
| 99th Percentile | 634.66 ms |
| Throughput | 7.94 txn/s |

      
#### 2.3.2 Stress Test - 
The stress test aimed to push the application beyond its normal capacity to identify the breaking point and observe failure behavior.

| Parameter	| Value |
| :--- | :---: |
| Number of Threads (Users)	| Starts at 50, increments to 500 |
| Ramp-Up Period	| 120 seconds |
| Loop Count	| 10 |
| Duration	| ~20 minutes |
| Test Objective	| Determine maximum user capacity and observe degradation patterns |

#### 2.3.3 Spike Test - 
The spike test was designed to simulate sudden, dramatic surges in traffic, such as during a flash sale or viral event.

| Parameter | Value |
| :--- | :---: |
| Number of Threads (Users)	| 10 → 500 → 10 (sudden changes) |
| Ramp-Up Period |	1 second for spike, 60 seconds for normal |
| Loop Count | 3 cycles |
| Duration |	~10 minutes |
| Test Objective	| Assess system reaction to instant traffic surges and recovery capability | 


### 2.4 Test Execution
All tests were executed using JMeter in non-GUI (command-line) mode to maximize performance and resource efficiency. The following command was used for each test
      bash
      jmeter -n -t blazedemo_test_plan.jmx -l test_results.jtl -e -o ./html_report
Before each test, the following steps were performed:

* The application (BlazeDemo) was verified to be accessible and responsive.

* System resources (CPU, memory, network) on the testing machine were monitored to ensure they were not interfering with results.

* A warm-up run was conducted to allow Just-In-Time compilation and caching mechanisms to stabilize.

* Each test was executed three times to ensure consistency and reliability of results. The average values were then calculated for analysis.

### 2.5 Results Collection and Analysis
During and after each test execution, performance data was collected using JMeter listeners and the generated HTML report. The key performance metrics analyzed included:

| Metric	| Description |
| :--- | :---: |
| Average Response Time	| Mean time taken for each request to receive a response |
| 90th/95th/99th Percentile	| Response time thresholds for the majority of requests | 
| Throughput	| Number of requests successfully processed per second |
| Error Rate	| Percentage of failed requests (e.g., HTTP 4xx/5xx errors)| 
| Standard Deviation	| Variability in response times | 

Bottlenecks were identified by analyzing which steps in the user journey exhibited the highest response times or error rates under increasing load. Potential causes such as network latency, server processing limits, or application code inefficiencies were considered.

---
## 3.OUTPUT

## 3.1 Load Test Results with Analysis

| Label | Avg Response (ms) | 90th pct (ms) | Error % | Status |
|-------|-------------------|---------------|---------|--------|
| Homepage | 374.40 | 443.80 | 0.00% | ⚠️ Slowest step |
| Find Flights | 360.72 | 420.80 | 0.00% | ✅ Good |
| Choose Flight | 366.45 | 425.90 | 0.00% | ✅ Good |
| Complete Booking | 362.47 | 404.00 | 0.00% | ✅ Best performance 

**Conclusion:** The application handled 1000 requests with 0% errors. Average response time was 366ms, which meets acceptable performance standards for a web application.

## 3.2 Stress Test Results with Analysis

| Label | Avg Response (ms) | 90th pct (ms) | 95th pct (ms) | 99th pct (ms) | Max (ms) | Error % | Status |
|-------|-------------------|----------------|----------------|----------------|----------|---------|--------|
| Homepage | 437.87 | 491.50 | 643.25 | 3527.86 | 3784 | 0.00% | ❌ Critical bottleneck |
| Find Flights | 402.77 | 458.90 | 510.00 | 2029.74 | 3950 | 0.00% | ⚠️ High max spike |
| Choose Flight | 414.16 | 457.00 | 536.70 | 2186.01 | 4097 | 0.00% | ❌ Worst max response |
| Complete Booking | 405.34 | 441.60 | 485.25 | 2259.63 | 3829 | 0.00% | ⚠️ High 99th pct |

### Overall Statistics

| Metric | Value |
|--------|-------|
| Total Requests | 1000 |
| Error Rate | 0.00% ✅ |
| Average Response | 415.04 ms |
| Min Response | 289 ms |
| Max Response | 4097 ms (4.1 sec) |
| Median | 362.50 ms |
| 90th Percentile | 464.90 ms |
| 95th Percentile | 525.00 ms |
| 99th Percentile | 2442.74 ms (2.4 sec) |
| Throughput | 7.77 txn/s |


## Spike Test Results with Analysis

| Label | Avg Response (ms) | 90th pct (ms) | 95th pct (ms) | 99th pct (ms) | Max (ms) | Error % | Status |
|-------|-------------------|----------------|----------------|----------------|----------|---------|--------|
| Homepage | 431.40 | 600.10 | 637.90 | 931.54 | 1057 | 0.00% | ⚠️ High 99th pct |
| Find Flights | 421.23 | 555.00 | 619.85 | 982.07 | 1274 | 0.00% | ⚠️ Highest max spike |
| Choose Flight | 440.05 | 578.80 | 648.70 | 1360.89 | 1549 | 0.00% | ❌ Slowest average |
| Complete Booking | 435.43 | 581.30 | 695.90 | 1033.29 | 1098 | 0.00% | ⚠️ High 95th pct |

### Overall Statistics

| Metric | Value |
|--------|-------|
| Total Requests | 1000 |
| Error Rate | 0.00% ✅ |
| Average Response | 432.03 ms |
| 90th Percentile | 575.00 ms |
| 95th Percentile | 641.85 ms |
| 99th Percentile | 1022.93 ms (1.02 sec) |
| Maximum Response | 1549 ms (1.55 sec) |
| Throughput | 7.90 txn/s |


## Final Conclusion

### Summary of All Tests

| Test Type | Verdict | Key Finding |
|-----------|---------|-------------|
| Load Test | ✅ **PASS** | 366ms avg, 635ms at 99th pct |
| Stress Test | ⚠️ **CONDITIONAL PASS** | 415ms avg, but 2.4s at 99th pct |
| Spike Test | ⚠️ **CONDITIONAL PASS** | 432ms avg, 1.02s at 99th pct |

### Overall Assessment

| Aspect | Rating | Explanation |
|--------|--------|-------------|
| Stability | ⭐⭐⭐⭐⭐ | 0% error rate across ALL tests (3000 total requests) |
| Average Performance | ⭐⭐⭐⭐ | Acceptable at 366-432ms |
| Outlier Performance (99th pct) | ⭐⭐ | Exceeds 1 second in stress and spike tests |
| Worst-Case Handling | ⭐⭐⭐ | Handles spikes better than stress (1.02s vs 2.4s at 99th pct) |

### Primary Bottleneck Identified

| Bottleneck | Severity | Evidence |
|------------|----------|----------|
| **Homepage** | ❌ Critical | 99th pct reached 3527ms (3.5s) during stress test |
| **Choose Flight** | ❌ Critical | 99th pct reached 2186ms (2.2s) during stress test |
| **Complete Booking** | ⚠️ High | 99th pct exceeded 1s in all tests beyond load |

### Recommendations

| Priority | Action | Expected Impact |
|----------|--------|-----------------|
| **High** | Optimize homepage loading | Reduce 99th pct from 3.5s to <1s |
| **High** | Investigate Choose Flight endpoint | Address 2.2s outliers |
| **Medium** | Implement caching for flight search | Improve spike test performance |
| **Medium** | Add request queuing | Prevent extreme 4.1s spikes |
| **Low** | Database query optimization | Improve overall consistency |

### Final Verdict

BlazeDemo is a **stable and reliable** web application that maintains **0% error rate** under all tested conditions (load, stress, and spike). The application performs **well under normal load** (366ms average, 635ms at 99th percentile). However, under **stress and spike conditions**, the 99th percentile response times exceed acceptable thresholds (1.02-2.4 seconds), with the **homepage and flight selection** identified as critical bottlenecks.

**Production Readiness:** ✅ Suitable for low-to-medium traffic environments. ⚠️ Not recommended for high-traffic or flash sale events without optimization of the identified bottlenecks.
