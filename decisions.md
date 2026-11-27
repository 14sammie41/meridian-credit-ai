# Meridian Credit Decision Journal

## Decision [000] - [Title]
**Date:** 00-00-0000
**Status:**

### Context:
...

### Options considered:
...

### Decision and reasoning:
...

### Limitations or risks:
...

## Decision 001 - Project structure and reproducibility
**Date:** 10-11-2026
**Status:** Decided

### Context:
Deciding on a structure for this project will give me boundaries to work with for the consistency of the project.

### Options considered:
I could either keep all of my exploration, documentation etc in one notebook for example, or I can split each stage up into its own space allowing me to create clear boundaries with each stage.

### Decision and reasoning:
I have decided to split each section and stage of this project into its own file and folder bubble in order to keep things like exploring the data separate from the actaul model code. This will improve organisation, reproducibility, observations and traceability. All of which are vital when considering the importance of explainability in this project.

### Limitations or risks:
The most prevalent limitation with this method is that there are going to be many more files to manage, more dependencies involved and it is more complicated to setup. All of these create risks for reproducing the project on another device, consistency when doing specific calculations and inconsistencies in implementation. The best way to manage this will be a clean file structure, clear documentation and consistent notes against my work.

## Decision 002 - Missing data handling
**Date:** 10-11-2026
**Status:** Provisional

### Context:
When initially investigating the data I found that the columns `years_at_current_address` and `savings_balance_gbp` both had missing values.

### Options considered:
I have to decide between keeping the missing values out of my model's process or imputing a result into those missing values.

### Decision and reasoning:
Given the importance of clarity when dealing with financial information I have decided to not include the missing values in my model. The consequences of imputing an applicant's answers are too great in the context of lending. The negative impact, should the imputation be less than fair, would affect both the business and the applicant negatively. I made sure I checked the difference in default rates for those with and without the relevant information and found that there was a difference of 1.75 percentage points difference. This is something I will have to carefully consider in all further decisions however I did not feel it was a large enough difference to warrant choosing a different method. This decision will obviously depend on which features I decide to select moving forwards.

### Limitations or risks:
The biggest limitation will be that an application may not be complete without this data. An applicant (in the final version) will be expected to complete these questions in order to fulfil a full application. Should an applicant not be able to produce an answer they may need to seek human assistance. This could obviously result in negative feedback for the business but this is a trade-off I believe is in the business' interests. The other risks involved in this decision are surrounding bias as it simply will not factor in those applicants who do not have that information and the model being fully representative of a whole population.

## Decision 003 - Initial feature selection and encoding startegy
**Date:** 11-11-2026
**Status:** Provisional

### Table:

| Feature | Current direction | Reason | Status |
|---|---|---|---|
| num_existing_credit_lines | Baseline candidate | To evaluate | Decided |
| num_late_payments_24m | Baseline candidate | To evaluate | Decided |
| credit_history | One-hot encoding | No clear ordering | Decided |
| checking_account_status | One-hot encoding | No clear ordering | Decided |
| loan_purpose | Exclude | No clear ordering | Decided |
| employment_status | One-hot encoding | No clear ordering | Decided |
| housing_status | Nominal for initial model | Fairness and proxy risk | Decided |
| years_at_current_address | Numerical for initial model | Fairness and proxy risk | Decided |
| months_in_current_job | Include as a numerical feature | Evidence gathered so far shows defaults decrease consistently accross exploratory groups but association does not mean causation. | Decided |
| previous_application_declined | Include in initial model to compare without later | Not enough information to discount entirely so in the name of fairness we will run a model with and a model without. | Decided |
| num_dependants | Exclude | Chosen to use monthly_expenses_gbp instead as it is a more relateable and numerically favourable decision | Decided |
| residency_status | Exclude | We have enough other information regarding an applicants housing which is tangible | Decided |

### Limitations or risks:
...

## Decision 004 - Predictive feature selection method
**Date:** 12-11-2026
**Status:** Decided

### Context:
I need to establish how predictive features are selected from the entire dataset for the model.

### Options considered:
Using an exclusion list which would remove each column that we chose individually or an inclusion list which would only include the individually selected columns.

### Decision and reasoning:
In the end I chose an inclusion list to ensure that if or when the dataset changes the risk of accidental leakage is reduced and that should a new engineer be maintaining the model they don't risk the model gaining information that has not yet been vetted.

### Limitations or risks:
There is an element of extra work and maintenance involved in using an inclusion list as all new features will need vetting and then adding to said inclusion list. The risk of this is that the manual approval of these new features does not mean they will be appropriate for the model.

## Decision 005 - Steps to take after baseline model complete.
**Date:** 18-11-2026
**Status:** Provisional

### Context:
Now we have a baseline model created, I need to determine what the next step of the investigation will be.

### Options considered:
Considering the baseline acheived a test ROC-AUC of 0.8129, which is close to its cross-validation mean, but misses 165 of 287 actual defaults at its current threshold of 0.5, we will need to investigate other thresholds to see if they offer a better balance.

### Decision and reasoning:
...

### Limitations or risks:
...

## Decision 006 - Test set protection
**Date:** 19-11-2026
**Status:** Decided

### Context:
Having reached a good point in my first models build I referred back to the original assignment breif to find that I had mistakenly used the test set before final evaluation.

### Options considered:
I could obviously discard the original evaluation then restructure the data splits, or I could annotate like I am here and make a plan to ensure I don't do it again.

### Decision and reasoning:
I will, from this point forwards be using training-data cross-validation to ensure I do not use the test set again until final evaluation. I acknowledge that my intrigue got the best of me and will ensure to note that going forwards, however I had not used the information gained from this to inform any preprocessing, feature selection or model parameter changes.

### Limitations or risks:
This does mean that I have touched the test set pre evaluation and that I cannot say that it was totally independantly held out. The test set was held out from model training, I did inspect the results before final model selection.

## Decision 007 - Deciding on a comparison model
**Date:** 19-11-2026
**Status:** Decided

### Context:
Having now created a baseline Logistic Regression model I need to create another model in order to have something to compare against. I am looking to compare non-linear relationships and feature interactions. 

### Options considered:
I could either go simpler or more complex, so it was between a decision tree and a random forest.

### Decision and reasoning:
I have chosen to use a random forest for my next model in order to add a layer of complexity as I would like to see if it genuinely improves our result of predicitve performance and interpretability. At this stage I will stick with one-hot encoding to minimise unnessecary differences and keep the comparison clear.

### Limitations or risks:
The following limitations are key to this, intrinsic interpretability will be reduced due to aggregations. Overfitting is a risk with each individual decision tree. Could require more resources than logistic regression.

## Decision 008 - Prioritising cost-sensitive evaluation over immediate hyperparamter tuning.
**Date:** 19-11-2026
**Status:** Provisional

### Context:
Given the complexities involved in comparing the impact of false positives to false negatives, I was at a cross roads as to what my next move should be. Logitic Regression gave me an ROC_AUC of 0.8212 compared with an ROC_AUC of 0.7998 for Random Forests and to compound on that confusion matixes showed different numbers under FP and FN for each.

### Options considered:
I could have either started changing the hyperparameters in both my Logistic Regression model and Random Forest models respectively, comparing the result against the baseline as I went, or I could start analysing and evaluating the results I have against the real world implications of them.

### Decision and reasoning:
Considering that I do not have the actual cost of missed defaults vs incorrect declines to Meridian-Credit I will need to conduct a sensitivity analysis of the results I got so far. Should this result in the need for further investigation into what it would look like if I changed hyperparameters on the models then so be it but given the potential consequences for both business and applicant, of classification errors, I will hold off on model optimisation for now.

### Limitations or risks:
The limitation of this decision is that I only have one model to compare against and both the models I have created are run using default thresholds so there is a lot to be desired. The risk of this is due to limited models for comparison and lack of real financial implications for Meridian-Credit.

## Decision 009 - Choosing threshold points
**Date:** 24-11-2026
**Status:** Decided

### Context:
The next step in sensitivity analysis is determining the range of the thresholds I should compare against.

### Options considered:
I could obviously choose an infinite number of options for this. In consideration of time and sanity I needed to find a happy medium between a wide enough range to justify my final decision and increments where I will actually see a change.

### Decision and reasoning:
Given the consistency of the results i got with the default 0.5 threshold it felt sensible to not stray too far from that so I have chosen a 0.4 - 0.6 range with increments of 0.02 to give us 11 datapoints per model. This is just an initial exploratory range.

### Limitations or risks:
I do not know that 0.5 was the optimal threshold and so I may find that actually moving in one direction or the other results is improvements beyond my hypothesis. There is a risk I will need to revise this later depending on the results I get. The other obvious risk is that the 0.02 increments wont be small enough so I will miss something.

## Decision 010 - Picking the threshold for evaluation
**Date:** 25-11-2026
**Status:** Provisional

### Context:
As you can see from decision 009 we had only looked at the results for our Logistic Regression model at the default threshold which meant I had no fair comparison of data put through the model. In order to determine the final threshold to use for the test set and evaluation I needed to run all of the thresholds explained in decision 009 in order to compare the results and weigh up all the implications of those changes. This process used out-of-fold predictions from the training data in order to preserve the test for the final evaluation.

### Options considered:
As can be seen in 03_model_comparison.ipynb all of the proposed thresholds between 0.40 and 0.60 with an increment of 0.02 were applied to the same predicted probabilities as we had done previously for the default threshold. In the interest of scientific analysis we ensured the only parameter changed was the threshold itself.

### Decision and reasoning:
When comparing the result I had a number of different options to choose from but the predominant thought whilst looking at them was the ultimate consdieration of fairness for the applicant, not just financial end game for the company. The results showed that 0.48 was the one threshold that produced the lowest combined number of classification errors within the range. We got a slight increase in recall, F1 score, false positive rate and decline rate, then an equally slight reduction in precision, false negative rate and total errors when compared to the default threshold of 0.50. Having looked at the difference at either extreme of my threshold experiment (0.40 and 0.60), there was no clear winner looking at the trade-off between incorrect declines and false approvals. Every time I incrementally changed the threshold around the extremes the trade-offs were more and more noticable. I must consider the implications of our decisions on the applicants themselves, therefore trade-offs become much harder to justify when the implications on the applicant are more serious. 

### Limitations or risks:
This decision comes as a result of using assumed financial costs rather than factual numbers which poses a risk as Meridian Credit may not feel this is relateable to their business. In using assumed financial costs I used equal numerical weighting for my final decison, again risking not relating to Meridian Credits actual figures because it was more important to me when considering responsible lending. There is also a consideration around a lack of a demographic fairness analysis potentially causing unfair results.

## Decision 011 - Model selection.
**Date:** 25-11-2026
**Status:** Decided

### Context:
I need to make a decision as to which model to eventually choose for final test and evaluation. This could only be done with comparative evaluations by using the training data and out-of-fold predictions.

### Options considered:
My initial observation of Logistic Regression was that it had good overall results for prediction, it could be slightly improved by dropping the threshold by 0.02 as evidenced in decision journal entry 010. I then ran exactly the same out-of-fold prediction comparison for the Random Forest model type to give us a complete comparison.

### Decision and reasoning:
When comparing directly, Logistic Regression(LR) and Random Forest(RF) at their respective lowest error thresholds, you can see a clear difference in the F1 score and combined classification errors with LR reporting 46 less classification errors at each models lowest total error count and a 4.45 percentage point drop for RF F1 score.
When considering the impacts of what I can see in this direct comparison to responsible lending, it was more important to me that there was a balance between an incorrect decline and a false approval. You can see that at a threshold or 0.48 for LR you get 861 total errors compared with a threshold of 0.46 for RF you get 907 total errors (The thresholds chosen for this comparison are that because they gave me the lowest combined error count). This will result in 19 more incorrect declines, 65 less incorrect acceptances and 46 fewer classification errors overall compared with RF at the same threshold.
The model that gave me the balance I was looking for was Logistic Regression at a threshold of 0.48 as the other thresholds checked against both models had trade offs that were not comparable. For example even when looking at all of the cost ratios Random Forest theoretically would have cost Meridian Credit more.
Given the importance of explainability in reference to lending LR lends itself to intrinsic interpretability, therefore supporting the need to explain to the applicant i plain English, the decision made.

### Limitations or risks:
There is a significant limitation in that I am choosing to accept a model which has a higher decline rate overall which must be justified in my next decisions relating to fairness. I also must acknowledge that I have not tuned Random Forest exhaustively or compared every possible threshold as explained in decision journal entry 009. I am yet to assess demographic fairness.

## Decision 012 - Explainability technique selection
**Date:** 26-11-2026
**Status:** Provisional

### Context:
When creating the final application which will be viewable to the applicant I need to be able to explain in plain English, why the model has made the decision that it has. I will also need a technical explanation for the Meridian Credit team to be able to analyse along with the need to audit the decisions the model has made too.

### Options considered:
In consideration of my choice to work with Logistic Regression in my final model, the two explainability techniques that fit best were SHAP and intrinsic feature contributions. SHAP would give me an explicit reference baseline and a broader potential for future changes in the model. Counter to that, intrinsic feature contributions would give me less complexity and no extra need for dependancies, keeping the dependancy footprint small. I would however need to define the explicit reference baseline myself.

### Decision and reasoning:
I did consider using both, having intrinsic feature contributions for my technical explanasion and SHAP for my plain English explanation but upon further review I accepted that the risk of the two different methods giving even slightly different results would risk inconsistent interpretations. Given the importance of transparency in financial lending scenario I have decided to use intrinsic feature contributions as it will avoid conflicting interpetations making auditing cleaner, reduces implemetation complexity and gives me meaningful responses for communicating good and bad factors to clients and colleagues alike.

### Limitations or risks:
Should the model get more complex in future iterations it would be harder to continue using intrinsic feature contributions as they do really lend themselves to the simpler model type of Logistic Regression meaning the developer in the future has some extra work. Reversing the encoding decisions made thus far will require grouping. Making my own decision about a reference baseline could have unfair influence on interpretations. 

## Decision 013 - Selection of reference points for individual feature contributions
**Date:** 26-11-2026
**Status:** Decided

### Context:
Having collated the transformed feature values in 04_explainability.ipynb I can now see the raw contributions each is making by multiplying the transformed feature value by its corresponding coefficient. This has allowed me to see that zero has different meanings depending on the preprocessing method used. If I was to use the all-zero one-hot encoded feature group as is, it is not representative of a valid category or any known categories for that matter.

### Options considered:
I could retain the original zero reference point, I could use the training population's weighted average or I could select an existing reference category for each of the nominal features.

### Decision and reasoning:
I have decided to keep the reference points for numerical and binary features as they are because they are easily explainable. Then with nominal categories I will reference it based on the most frequently occurring category in the training data. This will keep my model explainable both in the technical and plain English responses required, it will avoid extra subjective financial judgments on selection of reference categories and will be reproducible with training-derived reference points. The reference categories I choose will remain fixed during testing and application.

### Limitations or risks:
Just because it's the most common category, does not mean it doesn't have any bias or is financially neutral, I will document this as I work through. This decision will only change the explanation not the prediction itself.

## Decision 012 - Fairness evaluation methodology
**Date:** 27-11-2026
**Status:** Decided

### Context:
Having excluded protected characteristics in the model build itself does not eliminate the risk of discrimination and I must investigate and document these.

### Options considered:
Looking at demographic parity, predictive equality and equalised odds I evaluated all seven excluded attributes to determine my next steps.

### Decision and reasoning:
Considering the impact of financial experience, societal inequalities and geographical disadvantages I have opted to use equalised odds as it will be looking at actual results from false positive rates and false negative rates accross groups and backing it up with demographic parity. The three attributes I have chosen to prioritise are `age_years`, `sex` and `region`. Some other features could indirectly reflect age and it is a risk due to financial experience, someone at age 19 may have a less established credit history ect. of someone at 45 or 75. Sex has a significant risk with the known gender pay gap. Region could reveal geographical disparities that would not have been noticed otherwise.

### Limitations or risks:
Based on the sample size we have it could provide less reliable results than if we had a larger one. There is no way of establishing causation from the analysis. Only evaluating three attributes does limit the scope overall. There is an amount of statistical uncertainty.