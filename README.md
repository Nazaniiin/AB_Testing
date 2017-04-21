# A/B Testing

## Experiment Overview: Free Trial Screener
> At the time of this experiment, Udacity courses currently have two options on the home page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

> In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot shows what the experiment looks like.

> The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.

> The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

Before proceeding to the rest of the analysis, let's define our hypothesis better:

The main effect of the screener is to reduce the number of students who enroll by screening them out. There are two types of students who could be screened out:
- Students who would have **canceled during the free trial** after seeing they cannot devite more than 5 hour/week, and 
- Students who would have **continued past the free trial**.  

The hypothesis for the experiment is that students of the first group would be _reduced_, but students of the second group would _not be significantly reduced_; meaning if a student chooses to enroll in the course, the chances that they actually continue and finish the course stays high.

**Unit of Diversion:** 

When running A/B experiments, we need to divide our users into _control_ and _experiment_ group. To be able to do this division we need a unit to use to count as a single subject in our experiment. The unit of diversion should match up with how we identify and divide our users in the experiment. In this analysis, when a visitor lands on the Udacity webpage, they are assigned a cookie which will be used to divert them into either control or experiment group. Later we use our unit of diversion to identify our `Invariant Metrics` for the analysis.  When a visitor lands on the page they are assigned a unique cookie.. This cookie is used to divert the visitor into the experiment or control groups. 

# Experiment Design

## Metrics Choice

Here is a list of all metrics available to choose from. We need to choose our `Invariant` and `Evaluation` metrics from this list.

The practical significance boundary for each metric, that is, the difference that would have to be observed before that was a meaningful change for the business, is given in parentheses. All practical significance boundaries are given as absolute changes.

Any place `unique cookies` are mentioned, the uniqueness is determined by **day**. (That is, the same cookie visiting on different days would be counted twice.) `User-ids` are automatically unique since the site does not allow the same user-id to enroll twice.
- **Number of cookies:** That is, number of unique cookies to view the course overview page. `(dmin=3000)`
- **Number of user-ids:** That is, number of users who enroll in the free trial. `(dmin=50)`
- **Number of clicks:** That is, number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is trigger). `(dmin=240)`
- **Click-through-probability:** That is, number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page. `(dmin=0.01)`
- **Gross conversion:** That is, number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. `(dmin= 0.01)`
- **Retention:** That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. `(dmin=0.01)`
- **Net conversion:** That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. `(dmin= 0.0075)`

### Invariant vs. Evaluation Metrics

Defining metrics is one of the most crusial steps in an A/B testing process. These metrics are going to reveal whether launching the new experiment is going to work or not; meaning, if asking students about the number of hours they can devote to a course can let them re-think whether they want to enroll in the free trial and actually finish the course.

There are two sets of metrics that we need to choose in our analysis: Invariant and Evaluation metrics.

**Invariant Metrics:** 

After we divide our sample to control and experiment group, we want to make sure that this division has been done correctly i.e. we can asses the changes in our experiment group with more accuracy. To check this, we need to take a look at a few properties (aka. metrics) that check equivalences between the two groups, and which will not have direct implications on the efficacy of the experiment. We call these Invariant metrics. Invariant metrics perform consistent checking across all of our experiments. If the result of such checks are very different, it means that we might not be able to trust our data and need to do it all over again.

The Invariant metrics that we choose for our Udacity experiment are:

1. **Number of cookies**: This metric is also the Unit of Diversion in our A/B test. Since this cookie is generated when someone visits the Udacity webpage, and the webpage occurs **before** the experiment, it means that this metric does not get affected by the experiment and can be used as an invariant metric.

2. **Number of clicks**: Since the users click before the free trial screener pops up, this metric is also independent of the experiment, and can be counted as an invariant metric.

3. **Click-through-probability**: Since asking for the number of hours a user can devote to a course appears after the user clicks the 'start free trial' button; therefore, this metric remains the same between both groups and does NOT get affected by the experiment.

**Evaluation Metrics:** 

After defining our invariant metrics, it is time to see what metrics are going to get affected by the new experiment; hence, showing us whether we can launch the new changes or not. Evaluation metrics are used to draw the final business conclusions with. Let's see what can be possible evaluation metrics in our Udacity analysis:

1. **Gross conversion**: This metric is defined as _number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button_ . Users who complete checkout and enroll in the free trial have already seen the pop-up message as part of the new experiment; therefore, they affect the Gross coversion. This metric shows whether Udacity can lower its costs by showing the new pop-up. After the experiment, we expect Gross conversion to decrease.

2. **Retention**: Since users who enroll and remain in the course for at least 14 days have already seen the message regarding hours they can devote to the course, they are affected by the change. Therefore, retention is another evaluation metric in our study. Since we expect to see that the new changes will filter out those students who cannot spend more than 5 hours/week on the course and keep students who already enroll in the course to finish it, we expect to see rention to increase if the new changes in our experiment works as we want it. 

3. **Net conversion**: Another evaluation metric since it contains users who have enrolled in the course and have passed the 14-day trial; thus, they have already seen the message regarding the number of working hours per week. This metric shows how the new pop-up message affects the revenue; thus, we expect to see no change or an increase in this metric after the new experiment.

Later while using the evaluation metrics for our analysis, we will make some justifications to this list. However, we wanted to keep this list to what we originally chose for our evaluation metrics. In the `Duration vs. Expore` section, we go into depth of what evaluation metrics we finally decide to choose for the experiment and why.

**Unused Metrics:** 

1. **Number of user-ids**: Since this defines the number of users who enroll in the free trial, hence, see the new pop-up message, it could be chosen as an evaluation metric; however, this is just a _raw number_ that cannot necessarily provide any value on whether the new experiment is an efficient _business_ decision or not. Also, the way we assign users to control and experiment groups is random, and the data we collect is on daily basis. Considering this, we can see that user-ids do not really get affected here, hence, not a good choice for an evaluation metric. We could use this metric in conjuction with another metric to draw better conclusions. For example, Gross conversion as a metric uses the number of user-ids who enrolled in the free trial (after seeing the pop-up), by the number of cookcies who clicked on 'start a free trial'(before seeing the pop-up). Now this ratio makes more sense in a way that we want to see this Gross conversion decrease to reduce costs for frustrated users. Counting simply the number of user-ids who have enrolled in a course does not provide value for our analysis to conclude any decisions. That is why I left this metric out.

## Measuring Variability 

Given a `sample size = 5000` cookies visiting the course overview page, here are the baseline values for Udacity. These baseline values are dervied from history and gathered prior to planning and running the experiments; however, they do not represent the actual data on UDacity.

| Baseline                                               | Values           | 
| -------------------------------------------------------|:----------------:| 
| Unique cookies to view page per day                    | 40000            | 
| Unique cookies to click "Start free trial" per day     | 3200             |  
| Enrollments per day                                    | 660              | 
| Click-through-probability on "Start free trial"        | 0.08             |
| Probability of enrolling, given click (Gross)          | 0.20625          | 
| Probability of payment, given enroll (Retention)       | 0.53             |
| Probability of payment, given click (Net)              | 0.1093125        | 

### Calculating Standard Deviation

For each metric we chose as an evaluation metric, we calculate its standard deviation.

To calculate the standard deviation, let's first see the kind of distribution we have. Since we are calculating SD for our evaluation metrics, all of which are probabilities, we say we have a binomial distribution.

The standard deviation for a binomial distribution is `σ= sqrt((p(p-1))/n)` where:
- **_p_** is the probability of success on an individual trial
- **_1-p_** is the probability of failure on an individual trial
- **_n_** is the number of trials in the binomial experiment

Gross = user-ids to complete checkout and enroll in the free trial / unique cookies to click the "Start free trial" button

To find N, we have to look at what makes somebody eligible to be counted. For gross conversion, that's anybody who clicks on the start free trial button. 

- N = 5000 * 3200/40000 
- N = 400 
- P = 0.20625 
- 1-P = 0.7937 

**SD for Gross = 0.0202**

Retention = user-ids to remain enrolled past the 14-day boundary / user-ids to complete checkout

- N = 5000 * 660/40000
- N = 82.5
- P = 0.53
- 1-P = 0.47

**SD for Retention = 0.0.549**

Net = user-ids to remain enrolled past the 14-day boundary / unique cookies to click the "Start free trial" button

- N = 5000 * 3200/40000
- N = 400
- P = 0.53
- 1-P = 0.47

**SD for Net = 0.0156**

Here are the standard deviations for our evaluation metrics:

| Metrics                | Standard Deviation | 
| -----------------------|:------------------:| 
| Gross conversion       | 0.0202             | 
| Retention              | 0.0549             |   
| Net conversion         | 0.0156             |    


**Evaluating statistical and practical significance**

Now that we have calculated the standard deviation for our evaluation metrics (i.e. the analytical variability), we would like to also see how different they can be later when we run the actual study. Ideally, we do not like to see a big difference between statistical and practical significance.

Since the demoniators of Gross and Net conversion are the same as the unit of diversion (i.e. cookie), it is safe to say that the analytical variability will be similar to the empirical variability, but we will later calculate emperical variability and see how they differ.

Retention is an evaluation metric that has a different denominator `user-ides to complete checkout` than the unit of conversion (i.e. cookies), it might result in a significant difference between analytical variability and emperical variability for this metric. We will also dive more into this later. 

## Sizing

### Choosing Number of Samples given Power
Indicate whether you will use the Bonferroni correction during your analysis phase, and give the number of pageviews you will need to power you experiment appropriately. 

To calculate the sample size that covers the experiment for all of our chosen evaluation metrics, let's first look through the sample sizes we need for _each_ metric. 

We know that the `statistical power` and `significance level` is the same for all metrics:

```
Statistical power 1−β .............. 80% or 0.2
Significance level α  .............. 5% or 0.05
```

To calculate the sample size:

```
Gross conversion
----------------
Baseline conversion rate   .............. 20.625%
Minimum Detectable Effect  .............. 1%

Sample size: 25,835

Retention
---------
Baseline conversion rate   .............. 53%
Minimum Detectable Effect  .............. 1%

Sample size: 39,115

Net conversion
--------------
Baseline conversion rate   .............. 10.93125%
Minimum Detectable Effect  .............. 0.75%

Sample size: 27,413
```
To calculate the needed pageviews, we choose one sample size (our of the 3 we have) which covers all the groups. Since `Retention Sample Size = 39,115` and it already covers sample sizes needed for Gross and Net conversion, we choose it. Now to see how many pageviews we need, we work with `Click-through-probability on "Start free trial" = 0.08` from our baseline and calculate the total number of pageviews per group. Since we have two groups of control and experiment people, we then need to multiply our answer by 2.

```
Pageviews needed: ....... 39,115/(660/40000) = 2370606 * 2 = 4,741,212
```

Given this calculation, we also do not use the `Bonferroni correction` during our analysis phase.

**Note** As we proceed with calculating the duration, there will be a need to come back to this section, iterate over the chosen metric and make some changes. All will be discussed in `Duration vs. Expore` section.

### Choosing Duration vs. Exposure
Indicate what fraction of traffic you would divert to this experiment and, given this, how many days you would need to run the experiment. 

Give your reasoning for the fraction you chose to divert. How risky do you think this experiment would be for Udacity?

This experience does not affect the existing users enrolled in a course, as they do not see the pop-up message anymore. There are a group of students who are already a member of Udacity and might have started/finished a few courses. Adding the new pop-up message will probably not affect this group either, as they are already used to Udacity's approach and learning process, and might actually appreciate the honesty of Udacity. This experiment, however, can affect those who are new to Udacity or have been members but have never taken any courses. There is a chance that they might not like the warning, and find it a bit discouraging although logically speaking, the message shows how Udacity is dedicated to deliver the best results and is not only looking for possible revenue. The experiment does not collect any sensitive information either (e.g. credit card no). None of this will cause any harm to a user. Since our risk is very minimal, we can consider 100% of our traffic to be exposed to the change;however, since the experiment needs to be run for a few weeks and there might be infrastructure problems or bugs that occur during it, we decided to include 80% of our traffic in the experiment. This means out of 40000 views per day, we consider 32000. 


Choosing a `Fraction of traffic = 0.8` and `Pageviews  = 4,741,212`, when we calculate the days needed to run the experiment, we get `duration = 119` days. 

```
Duration = 4,741,212 / 32000 
         = 149
```
This is a very long duration for Udacity to run the experiment and they want this time to be reduced significantly. While running A/B tests, it is important to iterate over the metrics chosen until we can reach to a point where running the experiment is not risky and can also give significant results.

We need to take a look back at our evaluation metrics, gross conversion, retention and net conversion. Let's calculate the needed pageview also for gross and net conversions and see how they might change the duration of our analysis.

```
Pageviews needed for Gross conversion : ....... 25,835/(3200/40000) = 322937.5 * 2 = 645875
Duration of experiment choosing Gross:  ....... 645875 / 32000 = 21


Pageviews needed for Net conversion: ....... 27,413/(3200/40000) = 342662.5 * 2 = 685325
Duration of experiment choosing Net: ....... 685325 / 32000  = 22
```

Comparing the during of gross and net conversion with retention, shows how much longer it takes to run our experiment if we choose retention as an evaluation metric. Since the length of the experiment is an important factor for Udacity, we need to take another visit to what retention really measures and if it is safe to exclude it from the list of evaluation metrics.

Retention = user-ids to remain enrolled past the 14-day boundary / user-ids to complete checkout

Although retention is a nice metric to have and evaluate in our experiment, we see a low risk of excluding it since evaluations from Gross and Net conversion already cover the goals of the experiment: Reducing number of students who cannot devote enough time to the course and increasing their stay in the course which can hopefully lead to completion. 

Now we are on to have Gross and Net conversion as our two evaluation metrics. Since the sample size of net conversion is higher than gross conversion, we proceeded with choosing net conversion to calculate pageview and duration of the experiment.

```
Final Evaluation Metrics Chosen:
Gross conversion
Net conversion

Pageviews and Duration based on Net conversion:
Sample size ..... 27,413
Pageviews ....... 685325
Duration ........ 22
```

# Experiment Analysis
Now that we have defined invariant and evaluation metrics and calculated the duration of our experiment, it is time to see what we can conclude from the data that we have using these metrics. However, we need to run some sanity checks to make sure that our design was done properly.

For each of our invariant metrics, we give the 95% confidence interval for the value we expect to observe, the actual observed value, and whether the metric passes our sanity check.

## Sanity Checks

```
Confidence Interval
-------------------
95% -> z-score = 1.96

Number of cookies
-----------------
Total number of pageviews (control) .......... 345543
Total number of pageviews (experiment) ....... 344660
Total number of pageviews .................... 690203
Probability of success ....................... 0.5

Standard Error = Sqrt(P(1-P) / ([Pageviews Cont] + [Pageview Exp]) ) 
               = Sqrt( (0.5 * 0.5) / 690203)
               = 0.0006
Margin of Error = 1.96 * Standard Error
                = 1.96 * 0.0006
                = 0.00117
Lower CI Bound = 0.5 - [Margin of Error]
               = 0.5 - 0.00117
               = 0.49883 -> 0.4988
Upper CI Bound = 0.5 + [Margin of Error]
               = 0.5 + 0.00117
               = 0.50117 -> 0.0512
               
Observed = [Pageviews Cont] / ([Pageviews Cont] + [Pageviews Exp])
         = 345543 / 690203
         = 0.5006

Number of clicks
----------------
Total number of clicks (control) ............. 28378
Total number of clicks (experiment) .......... 28325
Total number of clicks ....................... 56703
Probability of success ....................... 0.5

Standard Error = Sqrt(P(1-P) / ([Clicks Cont] + [Clicks Exp]) ) 
               = Sqrt( (0.5 * 0.5) / 56703)
               = 0.00209
Margin of Error = 1.96 * Standard Error
                = 1.96 * 0.00209
                = 0.00409
Lower CI Bound = 0.5 - [Margin of Error]
               = 0.5 - 0.00409
               = 0.49591 -> 0.4959
Upper CI Bound = 0.5 + [Margin of Error]
               = 0.5 + 0.00409
               = 0.50409 -> 0.5040
               
Observed = [Clicks Cont] / ([Clicks Cont] + [Clicks Exp])
         = 28378 / 56703
         = 0.5004

Click probability
-------------------
CTR (control) .................... 0.0821258
CTR (experiment) ................. 0.0821824
CTR (pool) = [Clicks Cont + Clicks Exp] / [Pageviews Cont + Pageviews Exp]
           = 56703 / 690203
           = 0.082154        
SE (pool) = Sqrt(CTR_pool * (1 - CTR_pool) * (1/Pageviews Cont + 1/Pageviews Exp))
          = Sqrt( 0.082154 * 0.917846 * (0.00000289 + 0.0000029)
          = Sqrt(0.082154 * 0.917846 * 0.00000579
          = 0.00066
Margin of Error = 1.96 * Standard Error
                = 1.96 * 0.00066 
                = 0.00129
                
Difference = CTR_exp - CTR_cont

Under null hypothesis the difference equals zero. Then we would expect our estimation on the difference to be distributed normally with a mean of zero and standard deviation of SE_pool.

Lower CI Bound = 0 - [Margin of Error]
               = - 0.00129 -> -0.0013

Upper CI Bound = 0 + [Margin of Error]
               = 0.00129 -> 0.0013
               
Difference (observed) = CTR_exp - CTR_cont
                      = 0.08218 - 0.082125
                      = 0.00006 -> 0.0001

```
### Can we pass the sanity check and continue with our experiment?

Let's take another look at our confidence intervals and the observed values:

| Invariant Metric          | Confidence Interval | Observed Difference| Pass/Fail|
|---------------------------|:-------------------:|:------------------:|----------|
| Number of cookies         | [0.4988, 0.5012]    | 0.5006             | Pass     |
| Number of clicks          | [0.4959, 0.5040]    | 0.5004             | Pass     |
| Click-through-probability | [-0.0013, 0.0013]   | 0.0001             | Pass     |

Since all of the sanity checks for our invariant metrics have passed, it is safe to say that we can continue to do the experiment.

# Result Analysis
## Effect Size Tests
For each of our evaluation metrics, we give a 95% confidence interval around the difference between the experiment and control groups. We then indicate whether each evaluation metric is **statistically** and **practically** significant.

```       
Gross Conversion with Enrollment
--------------------------------
Total clicks (control) ................... 17293
Total clicks (experiment) ................ 17260
Total enrollments (control) .............. 3785
Total enrollments (experiment) ........... 3423

G probability (control) = Enrollment Cont / Clicks Cont
                        = 3785 / 17293
                        = 0.21887

G probability (experiment) = Enrollment Exp / Clicks Exp
                           = 3423/ 17260
                           = 0.19831

Gross probability (pool) = [Enrollment Cont + Enrollment Exp] / [Clicks Cont + Clicks Exp]
                         = (3785 + 3423) / (17293 + 17260)
                         = 7208 / 34553
                         = 0.2086
SE (pool) = Sqrt(G_probability_pool * (1 - G_probability_pool) * (1/Clicks Cont + 1/Clicks Exp))
          = Sqrt(0.2086 * 0.79139 * (0.0000578 + 0.0000579)
          = 0.00437 
Margin of Error = 1.96 * SE (pool)
                = 1.96 * 0.00437
                = 0.00856
                
Difference (Gross) = G_Probability_Exp - G_Probability_Cont
                   = 0.19831 - 0.21887
                   = - 0.02056
                   
Lower CI Bound = Difference - [Margin of Error]
               = - 0.02056 - 0.00856
               = - 0.02912 -> - 0.0291

Upper CI Bound = Difference + [Margin of Error]
               = - 0.02056 + 0.00856
               = -0.0120

Net Conversion with Payment
---------------------------
Total clicks (control) ................... 17293
Total clicks (experiment) ................ 17260
Total payments (control) ................ 2033
Total payments (experiment) ............. 1945

N probability (control) = Payment Cont / Clicks Cont
                        = 2033 / 17293
                        = 0.11756

N probability (experiment) = Payment Exp / Clicks Exp
                           = 1945/ 17260
                           = 0.11268

Net probability (pool) = [Payment Cont + Payment Exp] / [Clicks Cont + Clicks Exp]
                         = (2033 + 1945) / (17293 + 17260)
                         = 3978 / 34553
                         = 0.1151
SE (pool) = Sqrt(N_probability_pool * (1 - N_probability_pool) * (1/Clicks Cont + 1/Clicks Exp))
          = Sqrt(0.1151 * 0.8849 * ( 0.0000578 + 0.0000579 )
          = 0.00343
Margin of Error = 1.96 * SE (pool)
                = 1.96 * 0.00343
                = 0.00672 
                
Difference (Net) = N_Probability_Exp - N_Probability_Cont
                   = 0.11268 - 0.11756
                   = - 0.00488
                   
Lower CI Bound = Difference - [Margin of Error]
               = - 0.00488 - 0.00672 
               = - 0.0116 

Upper CI Bound = Difference + [Margin of Error]
               = - 0.00488 + 0.00672 
               = 0.00184 -> 0.0018

```
Let's put the result of our confidence interval and practical significance boundry in a table and start the comparison:

| Evaluation Metric          | Confidence Interval   | Practical Significance Boundry| Statistically/Practically|
|----------------------------|:---------------------:|:-----------------------------:|--------------------------|
| Gross conversion           | [-0.0291, -0.0120]    | dmin = 0.01                   | YES/YES                  |
| Net conversion             | [-0.0116, 0.0018]     | dmin = 0.0075                 | NO/NO                    |

For Gross conversion, we were looking for a statistically and practically significant decrease, and based on our calculations, we have it. Zero is NOT a part of the confidence interval, and the upper bound of our confidence interval is past the practical significance boundry. 

On the other hand, the results from Net conversion are not as we expected it. We wanted our experiment to NOT have a significant decrease. We wanted it to either stay the same (i.e. no statistically significant difference), or to have an increase. Zero is part of our confidence interval for Net conversion; thus, it is not statistically significant which is okay. However, CI does not contain the practical significance boundry; therefore, it is not practically significant either.

## Sign Tests
For each of our evaluation metrics, we do a sign test using the day-by-day data, and report the p-value of the sign test and whether the result is statistically significant.

To calculate the p-value, we use a two-tailed test since our null hypothesis says there won't be any change among the controlled and experiment group, while our alternative hypothesis says there is a change between these two groups.

To calculate P-value we need to know the number of our success. We define the number of success as follows:

If the difference between Gross/Net conversion in Experiment group is higher than the Control group, we count this trial as a success, otherwise, it is a failure.

```
Gross Conversion:
Number of successes.............  4
Number of trials................ 23
Probability..................... 0.5
Two-tailed p-value.............. 0.0026

Statistically significant?...... YES

Net Conversion:
Number of successes............. 10
Number of trials................ 23
Probability..................... 0.5
Two-tailed p-value.............. 0.6776

Statistically significant?...... NO
```

## Summary
Here we state whether we used the Bonferroni correction, and explain why or why not. If there were any discrepancies between the effect size hypothesis tests and the sign tests, we describe the discrepancy and why we think it arose.

In this experiment we decided not to use Bonferroni correction. The Bonferroni is used to reduce the chances of having false-positives, meaning it is unlikely for us to see false-positives in the experiment by chance. In our experiment, we are already covering this by wanting both Gross AND NEt conversions to be statistically significant. This also reduces the chance of having false-positive in our results. Therefore, we did not see the need to use Bonferroni correction as well.

Based on both `Effect Size Test` and `Sign Test` we could conclude that Gross conversion is statistically significant, while Net conversion is not. Also, during our effect size test we calculated that Gross conversion is also practically significant, while Net conversion is not.

## Recommendation
In this experiment, it was important for us to include the results from both of our evaluation metrics into account. This means we wanted to see the number of students who cannot devote 5 hours/week to decrease, and the number of those who enroll in the course and hopefully stay to finish it doesn't change or increase. In statistics terms, this means we did not only want to see statistically significant change in one of them, but in both.

Based on the results from Gross conversion, the new changes can lead to a decrease in Gross. This means students who cannot devote enough time to the course, would be filtered out so that the coaches can dedicte more of their time to those students who are more likely to continue the course and finish it. This is good; however, we also wanted to see either no change or an increase in Net conversion. This means, after filtering our students who could not devote enough time, we wanted to see if it brings more revenue to Udacity or just keeps the revenue the same. Since the confidence interval of Net conversion does include the negative of the practical significance boundary(dmin), launching this experiment might cause a decrease in revenue. This is definately something we don't want to see as a result of our experiment. I'd recommend **Not to launch** the new changes, and instead run tests to figure out whether net conversion really decreases or not.

# Follow-Up Experiment
In this section we give a high-level description of the follow up experiment we would run, what our hypothesis would be, what metrics we would want to measure, what our unit of diversion would be, and the reasoning for these choices.

I feel this experiment has done a great job in considering the metrics that could affect Gross and Net. If I want to suggest other experiments, maybe one would be to warn students about the `learning curves` in the course. From personal experience, I found the learning curves to be quite much during a Nanodegree program, and when I first encountered them, I felt that nobody had warned me about it. It made me feel frustrated and question whether I could continue with the course. Only after asking in the forums whether others had run into the same issues, I could get a better feeling that it was not only me. I started to try harder and could manage to finish all the Nanodegree projects. Therefore, I definately recommend a seminar or Q&A session where students can be informed about these learning curves and can clear things up even more. 

Speaking from another personal experience, one of my favorite parts while doing a course is to get feedback from a reviewer who has reviewed my project. Since the studying is being done online and I do not have that much interaction with the lecturers and course developers, having a reviewer going through my project and making personalized comments about it makes me feel part of the team. It makes the learning experience more "real". Maybe one metric to include in our experiment can be to see the probability of students staying the course, after they have made at least 1 project review (P0) from a Udacity reviewer. 

**Hypothesis**: There will be an increase in Renetention as more students decide to stay in the course

**Unit of Diversion**

I would choose `user-id` as the unit of diversion since the change only affects those who are already enrolled in a course.

**Metrics** 

List of all metrics:
- user-id
- Retention
- Attendance (for seminars)
- P0_completion

**Invariant** 

I choose `user-id` as my invariant metric since it stays the same as this experiment only affect students who are already enrolled in the course. The seminar and project review both happen after enrolling.

**Evaluation**  

I would choose the below metrics for evaluation:

- Retention: This measures the probability of payment, given a user is enrolled. It can be a great metric to measure since P0_completion is more likely to occure during this period. 
- Attendance: This metric is about students attending the seminar/Q&A sessions used to warn them about learning curves of a course. This can directly affect whether students continue to be enrolled in the course or not

Since all our evaluation metrics deal with experiments **AFTER** student enrollment, it might be costly for Udacity to carry them our. For instance, students might not necessarily finish P0 during their 14-day period, or having seminars can be financially expensive and Udacity might need to reasses whether spending the money on this point can benefit them later with possible increase in net conversion.


