# Principles of Simulation

An individual course project by **Maral Esmikhani**, completed in **August 2026** for *Principles of Simulation* in the Industrial Engineering program at Shahid Beheshti University.

The project explores two questions that arise when modeling a workstation. How closely do observed counts follow a Poisson distribution? How do the times between arrivals and the lengths of service affect the wait at a single server? Two browser-based exercises make the calculations visible, from individual observations to test statistics and queue performance measures.

| Exercise | Main output |
| --- | --- |
| [Poisson goodness-of-fit test](./chi-square-time-study.html) | A frequency table showing how much each count contributes to the chi-square statistic |
| [Single-server queue simulation](./single-server-queue-ks-simulation.html) | Distribution-fit diagnostics and a customer-by-customer record of arrivals, service, and waiting |

## Running the exercises

Download the repository ZIP from GitHub, extract it, and open either HTML file in a browser. Both exercises use HTML, CSS, and JavaScript and run without a build step or local server. The queue exercise requires an internet connection to load Chart.js from jsDelivr.

## Workstation count data and the chi-square test

Choose a sample size and a Poisson mean, then select **Generate Data & Run Test**. The entered mean controls data generation. The exercise then estimates the mean again from the generated sample and uses that estimate to calculate expected frequencies.

The table pairs each observed frequency with its expected frequency and its contribution to the test statistic, $(O-E)^2/E$. The last category includes the upper tail of the fitted distribution. The page sums the contributions and walks through the degrees of freedom, critical value, and decision at the 5% significance level. Repeating the exercise with the same settings produces a fresh sample.

### Reading a frequency-table row

Suppose a run with 100 observations has a fitted mean of 3 and a maximum count above 2. The Poisson probability of observing exactly two events is approximately 0.224, so the expected frequency for that row is 22.4. If two events occurred in 28 of the observations, the row contributes about 1.4 to the chi-square statistic. The same calculation is repeated across categories. A row with a large contribution identifies a count for which the observed frequency differs substantially from the model prediction.

Because the sample mean was estimated during fitting, the implementation uses $k-2$ degrees of freedom for $k$ included categories. The last category represents all counts at or above the largest observed count, retaining the probability the fitted distribution assigns to larger values.

## From time observations to a single-server queue

1. **Fit the inputs.** Enter inter-arrival and service times. The page fits an exponential distribution to each series using the reciprocal of its sample mean. Histograms and Kolmogorov–Smirnov tables show how the observed times compare with each fitted distribution.
2. **Generate a simulation run.** Keep or edit the estimated arrival and service rates, choose the customer count, and supply random numbers between zero and one. The exercise uses the inverse exponential transform to generate times and places the first arrival at time zero.
3. **Trace the queue.** Customers enter service in arrival order. Each row shows when a customer arrives, when service starts and ends, how long the customer waits, and how long the server was idle before service began.

The final summary reports average waiting time, average time in the system, total server idle time, and utilization over the simulated run. To explore the effect of service capacity, keep the arrival rate and random inputs fixed and change the service rate. The customer rows show where waiting begins to accumulate or disappear.

### How the queue advances

The input model and the queue model answer different questions. The input model describes the spacing of arrivals and the duration of service. For either series, the K–S table sorts the observations and compares the fitted exponential cumulative probability at each observation with the empirical cumulative probability immediately before and after it. The largest gap across the table gives the reported test statistic.

The queue model takes one uniform input $u$ and converts it to a duration $-\ln(1-u)/r$, where $r$ is the arrival or service rate. For each customer, the program adds the inter-arrival time to the previous arrival time. Service begins at the later of the customer's arrival and the previous customer's departure. Subtracting arrival time from service start gives time spent waiting. Subtracting it from departure time gives total time in the system.

### Reproduce a three-customer run

After fitting the input distributions, set both rates to **1 per minute**, set the customer count to **3**, and select **Generate Random Number Fields**. Replace the generated values with the inputs below and run the simulation. The first arrival is fixed at zero. Times in the table are in minutes, rounded to two decimal places.

| Customer | Arrival input | Service input | Arrival time | Service begins | Departure | Wait |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 0.50 | 0.75 | 0.00 | 0.00 | 1.39 | 0.00 |
| 2 | 0.50 | 0.50 | 0.69 | 1.39 | 2.08 | 0.69 |
| 3 | 0.90 | 0.50 | 3.00 | 3.00 | 3.69 | 0.00 |

Customer 2 arrives while customer 1 is still in service and waits about 0.69 minutes. Customer 3 arrives after customer 2 leaves, so the server has about 0.92 minutes of idle time. Over the whole run, the average wait is **0.23 minutes**, the average time in the system is **1.16 minutes**, and server utilization is **75.2%**. Utilization here covers the interval from time zero to the final departure.

## Interpreting the fit tests

The chi-square exercise retains categories with small expected frequencies, which can weaken the chi-square approximation. The queue exercise estimates each exponential rate from the same observations used for its K–S comparison and uses standard K–S critical values. Its displayed decisions should therefore be treated as classroom illustrations of the calculation.
