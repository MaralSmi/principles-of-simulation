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

## From time observations to a single-server queue

1. **Fit the inputs.** Enter inter-arrival and service times. The page fits an exponential distribution to each series using the reciprocal of its sample mean. Histograms and Kolmogorov–Smirnov tables show how the observed times compare with each fitted distribution.
2. **Generate a simulation run.** Keep or edit the estimated arrival and service rates, choose the customer count, and supply random numbers between zero and one. The exercise uses the inverse exponential transform to generate times and places the first arrival at time zero.
3. **Trace the queue.** Customers enter service in arrival order. Each row shows when a customer arrives, when service starts and ends, how long the customer waits, and how long the server was idle before service began.

The final summary reports average waiting time, average time in the system, total server idle time, and utilization over the simulated run. To explore the effect of service capacity, keep the arrival rate and random inputs fixed and change the service rate. The customer rows show where waiting begins to accumulate or disappear.

## Interpreting the fit tests

The chi-square exercise retains categories with small expected frequencies, which can weaken the chi-square approximation. The queue exercise estimates each exponential rate from the same observations used for its K–S comparison and uses standard K–S critical values. Its displayed decisions should therefore be treated as classroom illustrations of the calculation.
