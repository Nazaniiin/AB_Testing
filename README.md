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

**Invariant Metrics:** 

After we divide our sample to control and experiment group, we want to make sure that this division has been done correctly i.e. we can asses the changes in our experiment group with more accuracy. To check this, we need to take a look at a few properties (aka. metrics) that check equivalences between the two groups, and which will not have direct implications on the efficacy of the experiment. We call these Invariant metrics. Invariant metrics perform consistent checking across all of our experiments. If the result of such checks are very different, it means that we might not be able to trust our data and need to do it all over again.

The Invariant metrics that we choose for our Udacity experiment are:

1. Number of cookies: This metric is also the Unit of Diversion in our A/B test. Since this cookie is generated when someone visits the Udacity webpage, and the webpage occurs **before** the experiment, it means that this metric does not get affected by the experiment and can be used as an invariant metric.

2. Number of clicks: Since the users click before the free trial screener pops up, this metric is also independent of the experiment, and can be counted as an invariant metric.

3. Click-through-probability: Since asking for the number of hours a user can devote to a course appears after the user clicks the 'start free trial' button; therefore, this metric remains the same between both groups and does NOT get affected by the experiment.

**Evaluation Metrics** are usually the metrics that are used to perform business decisions with. In the Udacity example, it can be 
, is usually the business metris, like for example market share, number of users, or user experience metrics. Sometime it measure what previously stated as taking long time because it doesn't contain enough information, like measure user that got a job after taking MOOC. This is a difficult metrics which require special technique, as discussed in next blog.



