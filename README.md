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

### Calcularing Standard Deviation

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



