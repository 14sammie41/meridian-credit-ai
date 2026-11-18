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