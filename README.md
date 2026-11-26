### File structure notes:
- After noticing my mistake with the test data in my logistic regression model I have since renamed my model files to clearly reflect what each notebook is doing.

### Research notes:

- We will need to explain the need for human intervention when considering the impact of declines on applicants who are on the cusp. this will also need to be a consideration with fairness taken into account.
- When looking at `credit_history` we should consider using one-hot encoding due to the lack of evidence to back a clear order.
- The FCA has researched AI explanations in credit decisions
In February 2025, the FCA published research titled:
Credit where credit is due: How can we explain AI's role in credit decisions for consumers? The researchers investigated whether different explanations helped consumers identify errors in AI-assisted credit decisions. They found that the way an explanation was presented affected consumers' ability to identify and challenge errors. Different approaches helped with different types of errors. 
- Found the following paper fascinating from a statistical point of view: https://www.jstor.org/stable/7431d683-ccf1-3ecd-97b9-3baa4261b105?googleloggedin=true&seq=8

### Explainability notes:

- The more I look at the implications of explaining my decisions, the more I realise that it is important to explain both sides to the applicant. An applicant should know the good and the bad.
- It is vital that I incorporate a fallback mechanism for situations where the applicant doesn't fall into the thresholds I will implement for explanations. In a situation where all options come below the designed threshold I will need a response which makes sense to the applicant with a human oversight note to ensure that the applicant gets the explanation they deserve.
- When choosing an explanatin technique I spent some time considering using both intrinsic feature contributions and SHAP for this project. The theory was that I could use Intrinsic feature contributions for the technical explanation and then SHAP for the plain English explanation to the applicant. The biggest push back with this idea was that in the end the results could differ which would mean that the applicant would not have received a transparent explanation for their rejection or acceptance.
- So i know from a real life perspective that a zero hours contract is high risk unfavourable coefficient makes sense but I would have thought the influence of a permanent position would be greater than unemployed for what I consider to be obvious reasons. Then again with critical account status in credit history, i know that means they have reasonably bad credit and so why does that sway the model less than the amount they are requesting does?
- Unemployment has a more favourable coefficient than permanent employment does. This is counterintuitive when considering the security that permanent employment should bring. There could be a link with other financial variables that could account for some of this. This needs to be verified.
- Treating One-Hot encoded values the same way that binary values are treated is not the correct way due to the fact that any one-hot encoded feature having a zero tranformed value does not represent a genuine absense of that category. We must treat them inherently differently. See decision journal entry 013 for more information.