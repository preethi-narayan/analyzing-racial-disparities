# analyzing-racial-disparities
Studying the complexity of the learned model (a summary of the real dataset) and the accuracy of results of statistical queries on the derived synthetic dataset, under differential privacy
## Objectives
Study the interaction between the complexity of the learned model (a summary of
the real dataset) and the accuracy of results of statistical queries on the derived
synthetic dataset, under differential privacy <br>
Understand the variability of results of statistical queries under differential privacy, by
generating multiple synthetic datasets under the same settings (model complexity and
privacy budget) and observing how result accuracy varies <br>
Be aware of the trade-off between privacy and utility, by generating and querying
synthetic datasets under different privacy budgets and observing the accuracy of the
results <br>
### Racial disparities in predictive polling
#### Part A
Give three distinct reasons why racial disparities might arise in the predictions
of such a system.
#### Part B
Propose two mitigation strategies to counteract racial disparities in the
predictions of such a system. Note: It is insufficient to state that we could use a specific pre-,
in- or post-processing technique that we covered in class when we discussed fairness in
classification. Additional details are needed to demonstrate your understanding of how the
ideas from fairness in classification would translate to this scenario.
### Randomized response
#### Part A
The simplest version of randomized response involves flipping a single fair coin
(50% probability of heads and 50% probability of tails). Suppose an individual is asked a
potentially incriminating question, and flips a coin before answering. If the coin comes up tails,
he answers truthfully, otherwise he answers “yes”. Is this mechanism differentially private? If so,
what epsilon value does it achieve? Carefully justify your answer.
### Privacy-preserving synthetic data
In this problem, you will take on the role of a data owner who owns two sensitive datasets,
called hw_compas and hw_fake, and is preparing to release differentially private synthetic
versions of these datasets. <br>
The first dataset, hw_compas is a subset of the dataset released by ProPublica as part of their
COMPAS investigation. The hw_compas dataset has attributes age, sex, score, and race, with
the following domains of values: age is an integer between 18 and 96, sex is one of ‘Male’ or
‘Female’, score is an integer between -1 and 10, race is one of 'Other', 'Caucasian',
'African-American', 'Hispanic', 'Asian', or 'Native American'. <br>
The second dataset, hw_fake, is a synthetically generated dataset. We call this dataset “fake”
rather than “synthetic” because you will be using it as input to a privacy-preserving data
generator. We will use the term “synthetic” to refer to privacy-preserving datasets that are
produced as output of a data generator. <br>
We generated the hw_fake dataset by sampling from the following Bayesian network: <br>
<img width="292" alt="image" src="https://user-images.githubusercontent.com/98421957/202917483-e1dbb457-f3e6-4cb1-8a52-8349b5b19183.png"> <br>
In this Bayesian network, parent_1, parent_2, child_1, and child_2 are random variables.
Each of these variables takes on one of three values {0, 1, 2}. <br>
Variables parent_1 and parent_2 take on each of the possible values with an equal
probability. Values are assigned to these random variables independently. <br>
Variables child_1 and child_2 take on the value of one of their parents. Which parent’s
value the child takes on is chosen with an equal probability. <br>
To start, use the Data Synthesizer library to generate 4 synthetic datasets for each sensitive
dataset hw_compas and hw_fake (8 synthetic datasets in total), each of size N=10,000, using
the following settings: <br>
A: random mode <br>
B: independent attribute mode with epsilon = 0.1. <br>
C: correlated attribute mode with epsilon = 0.1, with Bayesian network degree k=1 <br>
D: correlated attribute mode with epsilon = 0.1, with Bayesian network degree k=2 <br>
#### Part A
Execute the following queries on synthetic datasets and compare the results to
those on the corresponding real datasets: <br>
##### Execute basic statistical queries over synthetic datasets
The hw_compas has numerical attributes age and score. Calculate the median, mean,
min, max of age and score for the synthetic datasets generated with settings A, B, C,
and D (described above). Compare to the ground truth values, as computed over
hw_compas. Present results in a table. Discuss the accuracy of the different methods in
your report. Which methods are accurate and which are less accurate? If there are
substantial differences in accuracy between methods - explain these differences. <br>
##### Compare how well random mode (A) and independent attribute
mode (B) replicate the original distribution.
Plot the distributions of values of age and sex attributes in hw_compas and in synthetic
datasets generated under settings A and B. Compare the histograms visually and
explain the results in your report. Next, compute cumulative measures that quantify the difference between the probability
distributions over age and sex in hw_compas vs. in privacy-preserving synthetic data.
To do so, use the Two-sample Kolmogorov-Smirnov test (KS test) for the numerical
attribute and Kullback-Leibler divergence (KL-divergence) for the categorical attribute,
using provided functions ks_test and kl_test. Discuss the relative difference in
performance under A and B in your report. <br>
Display the pairwise mutual information matrix by heatmaps, showing mutual information
between all pairs of attributes, in hw_fake and in two synthetic datasets (generated
under C and D). Discuss your observations in your report, noting how well / how badly
mutual information is preserved in synthetic data. <br>
##### Compare the accuracy of correlated attribute mode with k=1 (C) and
with k=2 (D).
Display the pairwise mutual information matrix by heatmaps, showing mutual information
between all pairs of attributes, in hw_fake and in two synthetic datasets (generated
under C and D). Discuss your observations in your report, noting how well / how badly
mutual information is preserved in synthetic data.
#### Part B
Study the variability in the mean and median of age for
synthetic datasets generated under settings A, B, and C. <br>
To do this, fix epsilon = 0.1, and generate 10 synthetic datasets (by specifying different seeds)
for each setting A, B, and C. Calculate the mean and median of age in each of the generated
datasets. Then, for each setting, plot the 10 median values and the 10 mean values using a
box-and-whiskers plot. Compare these metrics to the ground truth median and mean from the
real data. Carefully explain your observations: which mode gives more accurate results and
why? In which cases do we see more or less variability? <br>
Specifically, you should generate 30 datasets in total: 10 under setting A, 10 under setting B, 10
under setting C. For the box-and-whiskers plots, we expect to see two subplots: one for the
mean and one for the median, with the three settings (A, B and C) along the X-axis and age on
the Y-axis. You should include these plots in your report. <br>
#### Part C
Study how well statistical properties of the data are
preserved as a function of the privacy budget, epsilon. To see robust results, execute your
experiment with 10 different synthetic datasets (with different seeds) for each value of epsilon,
for each data generation setting (B, C, and D). Specifically, you should: <br>
Compute the KL-divergence over the attribute race in hw_compas. For each setting
(B, C, and D), vary epsilon from 0.02 to 0.1 in increments of 0.02. Specifically, the
epsilons are [0.02, 0.04, 0.06, 0.08, 1]. In total, you should generate 150 synthetic
datasets (3*10*5) and calculate the KL-divergence for race in each dataset. Create three
box-and-whiskers plots, one for each setting (B, C, D). Each plot should have epsilon on
the X-axis and KL-divergence on the Y-axis. Discuss your findings in the report and
include your plots.
