# PUI2016 HW 5.

## ASSIGNED READING:

- [10 Simple Rules for the Care and Feeding of Scientific Data](https://www.authorea.com/users/3/articles/3410/_show_article), Goodman A. (Live Document)

Also: make am Authorea account which you will use next week for the CitiBike Peer Review project!

## ASSIGNMENTS:

## Submission Info:
### You can work in groups, and you are encouraged to. Create a HW5\_\<netID> directory in your PUI2016\_\<netID> repository. 
Include a README.md that briefly summarizes the scope of the homework (so we know you understand what you did), and states with whome you worked and what you specifically contributed to.  
Submit Assignment 1 and Assignment 2  by pushing the **notebooks** into your PUI2016\_\<netID>/HW5\_\<netID>  repository.  
Provide the Null hypothesis for the experiments below in the README.md of your repository.
Keep in mind that we will look for possible cases of plagiarism, and if the code appears too similar to that of people that 
you did not work with to be original work (there are automated ways to look for plagiarism in code) *you will be penalized*. 


## Assignment 1: Compare Tests for Goodness of fit (on real data)
Test whether a gaussian model N($\mu$, $\sigma$) for the age distribution of citibike drivers is a sensible model, 
or if you can find a better fit with another distribution. 

- Use 2 tests: KS, AD, KL, chisq (even though we have not talked about it in detail yet) to do this. 

- Test at the Normal and a least one other distributions (e.g. Poisson, or Binomial, or Chisq, Lognormal.......)

No skeleton: you are on your own!


__Extra credit__: Divide your sample geographically: 
by Borrow + split Manhattan in an Uptown and a Downtown sample (use your discretion to do so, but ZIP code is a good idea) 
and see if you notice any differences in how the age distribution can be modeled. 


### GRADING: 

Your notebook must: 
- state the $H_0$ correcty
- generate the distributions correctly.
- use the test correctly
- make conclusions about the rejection of the Null
- each plot must have a caption which describes the plot in terms of Central Limit Theorem

## Assignment 2: Line fitting and data munging with income gender bias.

You may know that it is estimated that women earn about 78% of men in the same job position.
You will test if it is true on real income data, and turn your model into a prediction: 
if you get hired at a certain stipend as a men, what should you expect to make as a woman? 
Follow the [skeleton notebook](https://github.com/fedhere/PUI2016_fb55/blob/master/HW5_fb55/genderIncomeBias.ipynb)

Your notebook must: 
- have all celled filled in as indicated

- properly organize the data

- plot the scatter matrix

- plot the data (female vs male income) as directed

- do and plot a linear regression to the data, only Total Median Income and median income by category

- compare the linear regressions

- have predictions at the end of a salaty for a female, given the corresponding male salary

## Assignment 3: Practice formulating the null hypothesis 

Formulate the Null hypothesis in words and in formulae for the 4 experiments below:


- Do diets help lose more fat than the exercise? 

Experimental setup: you have a test and a control sample.

- Do American trust the president?

POLL RESULTS: On May 16, 1994, Newsweek reported the results of a public opinion poll that asked: “From everything you know about Bill Clinton, does he have the honesty and integrity you expect in a president?” (p. 23).
Poll surveyed 518 adults and 233, or 0.45 of them answered yes.

- Effectiveness of nicotine patches to quit smoking. 

Experimental setup: measure cessation rates for smokers randomly assigned to use a nicotine patch versus a placebo patch.

- Quantify the danger of smoking for pregnant women. 

Experimemtal setup: measure IQ of children at ages 1, 2, 3, and 4 years of age.
