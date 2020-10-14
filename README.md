# LAB-4-

# In this first running, it is about the craetion of the data to respond our question

> attach(acs2017_ny)
> to_use_varb<-(AGE>=25)&(AGE<=55)&(LABFORCE==2)&(WKSWORK2 > 4) & (UHRSWORK >= 35)
> data_to_use<- subset(acs2017_ny,to_use_varb)
> detach()
> attach(data_to_use)

# Then we wrote some code to run the regression of Income wage on multiple variable such as AGE, Gender, Language , Veteran Status and Education

> model_templ1<-lm(INCWAGE~AGE+ female+ LINGISOL+ VETSTAT + educ_nohs + educ_hs+ educ_college+ educ_advdeg)
> summary(model_templ1)

Call:
lm(formula = INCWAGE ~ AGE + female + LINGISOL + VETSTAT + educ_nohs + 
    educ_hs + educ_college + educ_advdeg)

Residuals:
<Labelled double>: Wage and salary income
    Min      1Q  Median      3Q     Max 
-147249  -33223  -10577   12923  625268 

Labels:

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)   29260.02    3353.92   8.724  < 2e-16 ***
AGE            1339.63      39.66  33.774  < 2e-16 ***
female       -25946.72     723.49 -35.863  < 2e-16 ***
LINGISOL     -12291.02    1608.58  -7.641  2.2e-14 ***
VETSTAT       -4675.06    2153.56  -2.171   0.0299 *  
educ_nohs    -23181.84    1845.33 -12.562  < 2e-16 ***
educ_hs      -12068.39    1046.88 -11.528  < 2e-16 ***
educ_college  35320.27    1031.05  34.257  < 2e-16 ***
educ_advdeg   61275.22    1100.57  55.676  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 76850 on 46962 degrees of freedom
Multiple R-squared:  0.148,	Adjusted R-squared:  0.1479 
F-statistic:  1020 on 8 and 46962 DF,  p-value: < 2.2e-16

> plot(model_templ1)
Error: $ operator is invalid for atomic vectors

# from the results, we can draw a conclusion such as the variable Education is highly significant in the determination of one income wage. Also, one variable stands out which is Linguistic Isolation with very small pvalue. this shows that the language ability could influence one income wage.
 
# the variable veteran is signficant only at alpha= 10%, although statistcally significant, its impact on the income wage is not that strong.
 
> NNobserv<-length(INCWAGE)
> set.seed(123456)
> graph_observ<-(runif(NNobserv)<0.1)
> data_to_graph<-subset(data_to_use,graph_observ)
> 
> plot(INCWAGE ~ jitter(AGE, factor = 2), pch = 16, col = rgb(0.5, 0.5, 0.5, alpha = 0.2), data = data_to_graph)
> # ^^ that looks like crap since Wages are soooooooo skew!  So try to find some sensible ylim = c(0, ??)
> plot(INCWAGE ~ jitter(AGE, factor = 2), pch = 16, col = rgb(0.5, 0.5, 0.5, alpha = 0.2), ylim = c(0,150000), data = data_to_graph)

# discus what you see in this plot

> tobepredicted<-data.frame(AGE= 25:55, female = 1, LINGISOL= 1,VETSTAT=2, educ_nohs=0,educ_hs=1,educ_college = 0, educ_advdeg= 0)
> tobepredicted$yhat<- predict(model_templ1, newdata = tobepredicted)
> lines(yhat~AGE,data= tobepredicted)

# In the data to be predicted, we tried to get rid of eductaion but have give  attention to teh variabe AGE,Gender as man and the avdantage of Language. the graph shows a positive regression line, that those variables have a significant role in the determination of Income wage. 
> detach()
