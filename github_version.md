# Product Analyst Challenge: Report
by: Jonathan Batscha

# Part 1

### Interpretation 

In the accompanying code, I calculated the chi-square significance for each experiment, treating each metric/vertical pair as a separate experiment, as well as treating all verticals as an aggregate:

This lead to some interesting results:


#### aggregated click data

$clickrate_{control} = .0295$

$clickrate_{variation} = .0281$

$\frac{clickrate_{variation}}{clickrate_{control}} = .951$

#### aggregated signup data

$signuprate_{control} = .0119$

$signuprate_{variation} = .0138$

$\frac{signuprate_{variation}}{signuprate_{control}} = 1.16$



Viewing all verticals as an aggregate results in the variation showing significantly lower clicks, and significantly higher signups. 

NOTE: This report will use an alpha of .05 as a threshold for statistical significance. 

However, when verticals are disagreggated, we see that the variation results in increased clicks and signups across the board (though not necessarily at a significant level). How is this possible? The answer lies in [Simpson's Paradox](https://en.wikipedia.org/wiki/Simpson%27s_paradox). In short, the control group oversamples from the Colleges vertical, which shows significantly higher click/signup rates.

### Disaggregated Results

Below, you can see the relative click/signup rates for each vertical, along with chi-square pvalue:

#### Clicks

[Clicks](clicks.png)

#### Signups

[Signups](signups.png)

### Recommendation

Keeping Simpson's Paradox in mind, I would base my recommendations on the **disaggregated data**, as follows:

1. **K-12**: there is sufficient evidence to switch to the variation, as both metrics (clicks/signups) are up by a statistically significant margin. I would move to start showing the variation to all users in the K-12 vertical.

2. **Colleges**: there is sufficient evidence that the variation increases signup rates, but insufficent evidence that it increases clicks. If signups are to be prioritized over clicks, I would make the switch to the variation; otherwise, I would continue to gather data, to have a better idea if there are potential trade-offs to be had between clicks and signups in making the switch to the variation.

3. **Places to Live**: there is sufficient evidence that the variation increases click rates, but insufficent evidence that it increases signups. If clicks are to be prioritized over signups, I would make the switch to the variation; otherwise, I would continue to gather data, to have a better idea if there are potential trade-offs to be had between clicks and signups in making the switch to the variation.

NOTE:
A common issue that arises when running multiple experiments on a given data set is that you increase your chances of finding what seems to be a statistically significant result. As an example, if you set the $pvalue = .05$ and split the data into 20 groups, you should expect to find 1 group with a statistically significant difference, even when the data is randomly generated. This is at the root of the [Replicability Crisis](https://en.wikipedia.org/wiki/Replication_crisis), in which ~50% of scientists fail to reproduce at least one of their own studies. For that reason, before committing to a new variant 100%, i would try to re-run the experiment, even if just on a subset of users, to validate that the results are replicable.


### Notes

1. It seems to me that there is a significant difference in users between the separate education verticals, but that both overlap to varying degrees with the Places to Live vertical. Specifically, parents looking for a great place to live are likely interested in K-12; and young people looking for a college may be interested in where those colleges are located. With that in mind, it may be useful to split up the Places to Live vertical into 3 groups based on a given user's interests (e.g Places-K12, Places-College, Places-only).

2. In the case where there is a variant that has an opposite effect on clicks and signups, I would want to get a better idea of the business value of each metric (i.e the "exchange rate" between clicks and signups) to be able to make a better informed decision.

3. When was the data collected? For example: Does user behavior differ at different points of the college application cycle? Would a College user be more likely to "add to list" at the beginning of the application process, and more likely to "compare" once they they have some decision?

4. Could you use cookies to decide whether a user is deciding on a city on a neighborhood (can infer from cookies related to housing searches), and use that data to decide which variant to show the user?

## Draft Email to CEO

*
SUBJECT: A/B Test Results, Add To List/Compare

Hey Luke,

We gathered the results from our A/B test. We did find some statistically significant results, along with some areas that require additional data. In short, I can strongly recommend changing the copy on K-12 to "Compare" from "Add to List". I can also recommend the same for Colleges, though slightly less strongly. Signup rate for our Places to Live vertical requires us to gather more data before I can make a confident recommendation. For details on the methodology, additional notes, and possible next steps, see the attached report.


Cheers,
Jon
*


# Part 2

### Benefits of upgrading to premium

- Niche a is a respected platform used by millions of prospective students each year
- Upgrading to premium allows your college to put the best foot forward through [numerous options](https://www.niche.com/about/niche-premium-profile-college/) to enhance your profile:
    - enhanced rankings
    - enhanced listings in search
    - sponsored listings
    - removing ads from profile
    - SEO friendly links
    - additional customization via school-provided links, cover photo, youtube video highlight, instagram integration, website link, etc
- All these features lead to real [results](https://www.niche.com/about/success-stories/?vertical=college):
    - 2-4x increase in click rates for colleges using premium
    - [increased brand awareness](https://www.niche.com/about/success-stories/university-of-chicago-increased-brand-awareness-by-100/)
    - [increased prospective student interest](https://www.niche.com/about/success-stories/florida-southern-college-has-300-students-expressing-interest-monthly/) 
- These results have a real impact on your colleges financial health and brand value:
    - increased awareness
    - increased interest
    - increased applications
    - increased yield
    - increased enrollment

### Goals

Certain subsets of Colleges have been [struggling](https://www.forbes.com/sites/michaelhorn/2018/12/13/will-half-of-all-colleges-really-close-in-the-next-decade/#4f7ff26052e5) with decreased enrollment and the lower tuition payments that come along with that.

At the core of it, colleges are looking for: 

1) Additional students willing to pay tuition, money that can be reinvested toward the mission of the college

2) Boosting their brand by attracting more students to apply, and top students to attend

As outlined in the above section, Niche offers many features to colleges interested in boosting key metrics that directly impact their financial health and brand value:
- brand awareness
- expressed interest
- applications
- yield
- enrollment


### Return on Investment

Colleges spend a lot in reaching out to new students, and have sizeable advertising budgets:
- online targeted ads
- mail catalogs
- billboards
- tv ads

A potential client could look at their ad spending in previous years, along with the associated results, and see how it compares to some of our existing clients's success stories. This can give them a gauge of how much money an X% increase in awareness, applications, etc, is worth to them.

For example, if a college has previously spent $\$10K$ on print ads with relatively little to show for it, they may be willing to spend that same amount to accurately target Niche's large user base of prospective students.



# Part 3

## Approach

- Load csv into pandas dataframe

- Process dataframe to make it easier to:
    - visualize relationships
    - run through machine-learning algorithms
    
    
- Processing steps
    - one hot encode metro area
    - one hot encode state
    - split type into multiple binary variable columns: private/charter, religious/non-religious, K-8/HS; then one-hot encode these new categorical variables
    - split status into 2 binary variables: new/not new, won/lost
    - convert grade into percent score, ranging from 0 to 1 (e.g using F = 0, A+ = 1)


- With the data augmented in this way, I would move on to visualizing relationship between different variables and the following target metrics:
    - won/lost (only using data from one of those 2 categories)
    - contract value (using only data from won)
    - I would then move on to try to build 2 predictive models:
        - estimating the probability that a new school will be won/lost
        - predicting the contract value if the school is won
        


- Visualization Tool:
    - [seaborn](https://seaborn.pydata.org/)


- Statistical tools
    - [scipy stats package](https://docs.scipy.org/doc/scipy/reference/stats.html)
    - [statsmodels package](https://www.statsmodels.org/stable/index.html)

- After data visualization, I would experiment with a few off the shelf machine learning algorithms for each of the 2 previously mentioned problems (predicting win/lost status, and predicting contract value):
    - [gradient boost](https://xgboost.readthedocs.io/en/latest/)
    - [neural nets](https://docs.fast.ai/tabular.html)
    - [linear/logistic regression](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.linear_model)


- Without getting into too much detail on the training process, I would be sure to:
    - use separate training/validation/test sets
    - Aggregate data with school from other regions, to augment the training set
    - other best practices from the following excellent book: [Machine Learning Yearning by Andrew Ng](https://www.deeplearning.ai/machine-learning-yearning/)



- Once I have a decent model for predicting status and contract value, I would calculate an expected value (EV) for each school
    
    $EV = Probability_{win\_contract} * Estimated\_Contract\_Value$
    
- Rank schools by EV (descending order) to prioritize schools that will have the largest expected impact on revenue
    - Target schools for sales outreach in order of priority, starting at the top of the list and working down 



## Additional Data To Help

I would seek the following additional data for each school, that may be helpful in estimating the key metrics:

- Tuition
- Zip Code
- \# of students
- Which sales person was responsible for each won/lost school
    - this may be the most predictive factor in winning a contract
- What time of year the school was contacted by sales
- Whether a similar/competing school in the area is already on Niche Premium
