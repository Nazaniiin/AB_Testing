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

3. **Net conversion**: Another evaluation metric since it contains users who have enrolled in the course and have passed the 14-day trial; thus, they have already seen the message regarding the number of working hours per week. This metric shows how the new pop-up message affects the revenue; thus, we expect to see an increase in this metric after the new experiment.

Later while using the evaluation metrics for our analysis, we will make some justifications to this list. However, we wanted to keep this list to what we originally chose for our evaluation metrics. In the `Duration vs. Expore` section, we go into depth of what evaluation metrics we finally decide to choose for the experiment and why.

**Unused Metrics:** 

1. **Number of user-ids**: Since this defines the number of users who enroll in the free trial, hence, see the new pop-up message, it could be chosen as an evaluation metric; however, this is just a _number_ that cannot necessarily provide any value on whether the new experiment is an efficient _business_ decision or not. We could use this metric in conjuction with another metric to draw better conclusions. For example, Gross conversion as a metric uses the number of user-ids who enrolled in the free trial (after seeing the pop-up), by the number of cookcies who clicked on 'start a free trial'(before seeing the pop-up). Now this ratio makes more sense in a way that we want to see this Gross conversion decrease to reduce costs for frustrated users. Counting simply the number of user-ids who have enrolled in a course does not provide value for our analysis to conclude any decisions. That is why I left this metric out.

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

The fraction of traffic we would divert to this experiment is 1. We would choose a lower value in case exposing 100% of traffic to the experiment would be of high risk. Since nothing regarding potential risks has been mentioned in the definition of the project, we continue with 100% of our traffic. Choosing a `Fraction of traffic = 1` and `Pageviews  = 4,741,212`, when we calculate the days needed to run the experiment, we get `duration = 119` days. 

```
Duration = 4,741,212 / 40000 
         = 118.5
```
This is a very long duration for Udacity to run the experiment and they want this time to be reduced significantly. While running A/B tests, it is important to iterate over the metrics chosen until we can reach to a point where running the experiment is not risky and can also give significant results.

We need to take a look back at our evaluation metrics, gross conversion, retention and net conversion. Let's calculate the needed pageview also for gross and net conversions and see how they might change the duration of our analysis.

```
Pageviews needed for Gross conversion : ....... 25,835/(3200/40000) = 322937.5 * 2 = 645875
Duration of experiment choosing Gross:  ....... 645875 / 40000 = 17

Pageviews needed for Net conversion: ....... 27,413/(3200/40000) = 342662.5 * 2 = 685325
Duration of experiment choosing Net: ....... 685325 / 40000 = 18
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
Duration ........ 18

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


