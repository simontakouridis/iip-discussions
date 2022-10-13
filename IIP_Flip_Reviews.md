# IIP: Flip Reviews
<br/>

## Abstract

This proposal introduces flip reviews, which includes both the reporting and scoring of flips based on AI resistance and keyword usage during the long session. Incentive structures have been transformed to utilise this information to effectively and accurately disburse rewards and penalties. The result of the changes proposed herein are expected to significantly improve flip quality.
<br/>

## Motivation

The current state of affairs with regard to overall flip quality is abysmal. Witnessed too often are poor quality flips that are by no objective measure AI resistant. The incentive to produce these poor quality flips are mostly for economic reasons, specifically the ability for identity farms to apply a templated flip to almost any set of keywords drastically reduces the cost of production. In doing so however, the security and sybil resistance of the underlying network is denuded, thereby limiting its promise of being a trustworthy platform for which applications and stakeholders can depend on. Moreover, the current reporting system implemented to prevent poor quality flips is insufficient, and counter-productive in that objectively good flips too often result in penalisation. The primary motivation of this proposal is to provide mechanisms where 1) flips can be conveniently and accurately scored for quality, 2) quality flip creation and accurate flip reviews are properly incentivised, and 3) network abusers submitting consistent poor quality flips are severely punished. These mechanisms are expected to place the Idena network on a trajectory where flip quality improves over time, thereby instilling security and sybil resistance that can foster network dependability and growth.
<br/>

## Specification

### Overview

The proposed flip review system is an overhaul of the flip reporting system to include the scoring of flips based on their AI resistance and keyword usage, so that an overall grade can be calculated for each flip. From this, all flips submitted in the epoch can be ranked based on their grade. Flip rewards are then distributed based on their relative flip ranking.

The role of committee consensus with regards to flip reviews is limited to the determination of reviewer rewards. Consensus is achieved when the scoring of flips is sufficiently similar with other committee members, and when relative majority of consensus points has been reached within the committee. These changes are designed to generously reward accuracy in flip scoring so as to train effective reviewers, thereby leading to increased review accuracy over time. To further improve accuracy, flip reviewer committees give more weight to identities with human status.

Other significant changes include identity penalties and rewards for consistently poor or good flip submissions, significant increases in flip rewards and reviewer rewards, and various other flip review related changes to the protocol and incentives.
<br/>

### The Long Session UI

The long session UI for the flip reporting round will be altered to include extra detail allowing users to report or score each presented flip (see Figure 1 below). Flip reporting is very similar to as previous, where the reviewer can either Approve or Report each flip. A key difference to note is that there is no limitation on how many reports the reviewer can give. If the review selects to Approve the flip, they are then presented with the options for scoring the flip. Scoring allows reviewers to give a score of 1, 2, or 3 for AI Resistance, and furthermore 1, 2, or 3 for Keyword Usage. The labels of these buttons are “1 (best)”, “2”, and “3”.

The information modal for “AI resistance score?” includes the following description:
> *An ideal AI resistant flip:*
> - *has a meaningful storyline that encompasses all images in the flip.*
> - *is free of elements or patterns that make the image order predictable without knowing the story.*

The information modal for “Keyword usage score?” includes the following description:
> *A flip with ideal keyword usage:*
> - *has both keywords well integrated as subject matter of the storyline.*
> - *is difficult to be re-used with another set of keywords.*
> - *is free of a pattern that is reusable with another set of keywords.*

The reviewer is able to either leave the scorings blank, or select 1, 2 or 3 for each of the criteria. The reviewer is also provided a note on top of the review selections as follows:
> *All review options presented to you must be selected with accuracy otherwise severe penalties may apply.*

Flips in the long sessions are randomly shuffled and displayed in a unique order for each of the committee members, so as not to introduce any perverse Schelling points of consensus based on flip display order e.g. everyone selecting the first flip for reporting.


*Figure 1: The Long Session UI for the flip review round.*
<br/>

### Flip Review Definitions

The flip review definitions for scoring are:

- **Report:** an explicit definition where the flip has clearly flouted one of the defined flip rules.
- **Approve:** an explicit definition that the flip correctly passed all of the defined flip rules.
- **1 (best):** an implicit and non-specified definition that this is the best relative scoring a flip can receive.
- **2:** an implicit and non-specified definition that this is the closest relative to 1, but without a definition of distance from 1.
- **3:** an implicit and non-specified definition that this is the closest relative to 2, but without a definition of distance from 2.

The first option “report” or “approve” is relatively straight forward for the reviewer to follow i.e. does the flip follow the explicitly defined rules. Scoring becomes ill-defined for options “1 (best)”, “2”, and “3”, whereby they have been intentionally implicit and non-specific with their underlying meaning. A clear definition for the ideal flip with respect to AI resistance and keyword usage has been provided to the reviewer as an absolute reference point to assist with their relative scorings.

The scorings of 1, 2 and 3 are intentionally implicit and non-specific so that the community of reviewers, over the epochs, can unconsciously define and agree via Schelling points on the definitions, which is ultimately driven by the generous consensus rewards. The benefit here is that the definitions can evolve over time in concert with the advancement of overall flip quality. For example, what defines 1, 2 and 3 today may be radically different to 100 epochs from today when overall flip quality is significantly improved. This makes the flip review system automatically adaptable without the need for explicit definitions that must be continually updated, disseminated and educated.

Three scoring levels was required to properly resolve the flip quality of all submitted flips into a relative ranking, which is used for accurate disbursement of flip rewards, and identity penalties and rewards.
<br/>

### Flip Review Submission

At the conclusion of the long session, the reviewer sends all of the review selections (reports and scores, including all abstentions) in the “SubmitLongAnswersTx”, whereby the protocol will use this information to calculate the associated grade assigned to each flip.

The “Bit answers” structure with respect to the flags is altered and expanded to include information about the review selections. More specifically, for each flip the Flags structure is now 6 bits in length, comprising of 3 bit pairs i.e. (##)(##)(##).

1st bit pair (Flip Correct)
00 = abstain
01 = report
10 = approve
11 = invalid tx

2nd bit pair (AI Resistance)
00 = abstain
01 = score of 1
10 = score of 2
11 = score of 3

3rd bit pair (Keyword Usage)
00 = abstain
01 = score of 1
10 = score of 2
11 = score of 3
<br/>

### Flip Grading and Ranking

When all of the reviewer selections have been submitted for each flip in the epoch, they are then translated into a grading from 0 to 4 for each flip (see Table 1 below). Notice that several scoring combinations can lead to the same flip grade. The principle goal of the flip review round is to gather as much information about the quality of each flip from the reviewers. Whereby each reviewers’ scoring (and subsequent grade) is utilised for the overall quality determination of each flip – committee consensus is irrelevant in this regard.

*Table 1: Scoring combination translation into a flip grade.*
| Flip Correct | AI Resistance Score | Keyword Usage Score | Flip Grade |
|--------------|---------------------|---------------------|------------|
| abstain      | n/a                 | n/a                 | n/a        |
| report       | n/a                 | n/a                 | 0          |
| approve      | abstain             | abstain             | 1          |
| approve      | 3                   | abstain             | 0.5        |
| approve      | 2                   | abstain             | 1          |
| approve      | 1                   | abstain             | 2          |
| approve      | abstain             | 3                   | 0.5        |
| approve      | 3                   | 3                   | 0.25       |
| approve      | 2                   | 3                   | 1          |
| approve      | 1                   | 3                   | 1          |
| approve      | abstain             | 2                   | 1          |
| approve      | 3                   | 2                   | 1          |
| approve      | 2                   | 2                   | 2          |
| approve      | 1                   | 2                   | 3          |
| approve      | abstain             | 1                   | 2          |
| approve      | 3                   | 1                   | 1          |
| approve      | 2                   | 1                   | 3          |
| approve      | 1                   | 1                   | 4          |
<br/>

Depending on committee sizes, each flip in the epoch is expected to have one or more gradings associated with it. Using this information, a relative flip quality ranking of all flips from highest grading to lowest grading is made, by the following method:

1. The median and average grade for each flip is calculated (using human gradings only).
2. The median and average grade for each flip is calculated (using non-human-validator gradings only).
3. The final median grade for the flip is calculated as follows: `Final_Median_Grade = ((2 * Human_Median_Grade) + Non_Human_Median_Grade) / 3`
4. The final average grade for the flip is calculated as follows: `Final_Average_Grade = ((2 * Human_Average_Grade) + Non_Human_Average_Grade) / 3`
5. If in the very rare case that no grade was awarded to a flip, the flip receives a “benefit of the doubt” mediocre grading of 2 (Both median and average).
6. The flips are sorted, first by the final median grade descending, then by the final average grade descending, then by committee size descending, then by flip submission time ascending.

This relative quality ranking of all flips for the epoch enables the effective disbursement of rewards and penalisations. Human gradings have more weight to ensure that a more experienced and competent assessment of flip quality determination.
<br/>

### Flip Rewards

The ranked flips for the epoch are used to determine the flip rewards. Each flip is judged on their specific location in the sorted flip ranking and assigned a reward based on this location, whereby the author of that flip is rewarded. The ranked flips are split equally into 5 parts or tiers, from the highest quality tier to the lowest quality tier, with each tier having 20% of all flips for the epoch. More specifically, the flip rewards are distributed as follows:

- All flips in the highest tier receive 52% of total flip rewards.
- All flips in the second highest tier receive 27% of total flip rewards.
- All flips in the middle tier receive 14% of total flip rewards.
- All flips in the second lowest tier receive 7% of total flip rewards.
- All flips in the lowest 20% tier receive no rewards.

Rewards for each tier are equally distributed amongst the flips, and subsequently directed toward the corresponding author identities. Also note that flips in the lowest tier are disqualified from validating identities in the short session.
<br/>

### Flip Reviewer Consensus

With this proposal, the concept of a qualification committee remains, and is unchanged with respect to the agreed answer (left or right). Although it is altered with respect to flip scorings whereby consensus is sought only as a means to reward reviewers. However, the definition of consensus is very specific:

1. Consensus is only achieved when sufficient reviewers in the committee have matching scorings for a flip, with some scoring combinations being grouped due to being similar. The categories of scoring combinations for consensus is shown in Table 2 below. For example, consensus may be achieved if sufficient committee members selected scoring category 1. Note that any abstentions by a reviewer will disqualify them from the committee in relation to consensus for reviewer rewards.
2. When a human reviewer selects a scoring category for a flip, they apply 1 consensus point to that scoring category. When a non-human-validator reviewer selects a scoring category for a flip, they apply 0.5 consensus points to that scoring category. The total consensus points are summed for each scoring category for that flip.
3. Consensus is achieved by scoring category with highest number of consensus points, with a minimum of 2 consensus points required. Furthermore, ties are permissible between the scoring categories so long as the tie spans no more than 2 grades (1 grade distance). For example, a tie between categories 2, 3 and 4 will all achieve consensus because together they only span 2 grades (grades 1 to 2), whereas a tie between categories 3 and 7 will not achieve consensus because they span 4 grades (grades 1 to 4).

Humans and non-human-validator reviewers have different weightings, for both flip grades and reviewer consensus points. As such, the flip lottery will be adjusted to maximise the spread of humans and non-human-validators evenly across qualification committees. More specifically, the flip lottery will include provisions that ensure flip authors share their flips with a consistent ratio of human to non-human-reviewer ceremony participants. A technical suggestion to ensure this consistent ratio would be to conduct an initial flip lottery with only humans as the cohort of flip authors, then overlay the results with a subsequent flip lottery where only non-human-validators are the cohort of flip authors.
<br/>

### Reviewer Rewards

The rewards for reviewers is 80% of the total reviewer rewards. Consensus among the qualification committee of reviewers of a flip is used to determine the disbursement of these rewards.

The total reviewer rewards has been split and equally allocated across the flip grades, with 16% of these rewards allocated for each grade, see Share of Reviewer Rewards in Table 2 below. However, where two scoring categories correspond to one flip grade, the allocated rewards are split into 8% for each of the scoring categories for that grade. As such, all instances of consensus within a particular scoring category (for all flips for that epoch) will share in the pool of rewards that are allocated to that category. More specifically, all reviewers that achieve a consensus scoring will receive an equal share in the rewards pool for that scoring category, moreover the number of shares a reviewer has within that rewards pool is directly proportional to the number of consensus scores that reviewer achieved within that scoring category. For example, in an epoch, if there were 10 instances of consensus for scoring category 3, with a total of 40 reviewers that participated in those 10 instances of consensus, then the 8% rewards pool for scoring category 3 will be equally distributed amongst those 40 reviewers. Note however that of those 40 reviewers, there may in fact be duplicate identities, because the same reviewer can be in multiple instances of consensus i.e. for reviews of different flips. As such, of those 40 reviewers, there may be just 30 identities, with the duplicates identities having duplicate shares of the rewards pool. This method of reward distribution ensures that flip scoring accuracy is evenly awarded regardless of committee sizes.

As previously mentioned, if a reviewer abstains from any of the review options provided to them for a particular flip, then they will not be considered a part of the qualification committee for the puporses of reviewer rewards. Furthermore, rewards will not be distributed for any reviewers of a flip if consensus was not found e.g. by having insufficient consensus points for any scoring category, or by having an invalid tie that spans more than 2 grades.


*Table 2: Scoring categories associated with specific scoring combinations.*
| Scoring Category | Scoring Combinations                                                                             | Corresponding Flip Grade | Share of Reviewer Rewards |
|------------------|--------------------------------------------------------------------------------------------------|--------------------------|---------------------------|
| 1                | Flip Correct – report<br/> *or*<br/> AI Resistance – score of 3<br/> Keyword Usage –  score of 3 | 0 - 0.5                  | 16%                       |
| 2                | Flip Correct – approve<br/> AI Resistance – score of 3<br/> Keyword Usage – score of 1 or 2      | 1                        | 8%                        |
| 3                | Flip Correct – approve<br/> AI Resistance – score of 1 or 2<br/> Keyword Usage – score of 3      | 1                        | 8%                        |
| 4                | Flip Correct – approve<br/> AI Resistance – score of 2<br/> Keyword Usage – score of 2           | 2                        | 16%                       |
| 5                | Flip Correct – approve<br/> AI Resistance – score of 2<br/> Keyword Usage – score of 1           | 3                        | 8%                        |
| 6                | Flip Correct – approve<br/> AI Resistance – score of 1<br/> Keyword Usage – score of 2           | 3                        | 8%                        |
| 7                | Flip Correct – approve<br/> AI Resistance – score of 1<br/> Keyword Usage – score of 1           | 4                        | 16%                       |
<br/>

The purpose of having flip reviewer rewards is not only to incentivise the giving of flip reviews, but also to train reviewers for accuracy when scoring flips. As such, rewards are given only when the calculated grade is in consensus, and when the scores that lead to that grade are sufficiently similar. Such an incentive scheme is expected to lead to greater flip reviewing accuracy over the epochs.

The relatively large number of scoring categories mean that consensus is challenging for a reviewer to achieve. This has been alleviated by introducing the concept of consensus points, and only requiring plurality for consensus. Notwithstanding, this is inconsequential over a time because when consensus is achieved, the reward is greater due to the greater share of the rewards pool. In other words, a ‘low frequency / high reward’ incentive scheme as implemented here, is similar in outcome to that of a ‘high frequency / low reward’ scheme.

The share of reviewer rewards are evenly pooled and allocated across all of the gradings and consensus categories because it coerces reviewers to focus on accuracy of scoring. Had there just been one large pool of reviewer rewards, then many reviewers will solely focus on finding and being rewarded for perverse Schelling points of consensus to the detriment of the network. For example the reporting all flips, not because there was an issue with these flips but because others will behave in the same way to easily achieve consensus and thus disingenuously extract reviewer rewards.
<br/>

### Low Accuracy Reviewer Rewards

There will also be reviewer rewards allocated to low accuracy reviews. This will comprise of 20% of total reviewer rewards and is established to incentivise review participation (non-abstentions) with some level of accuracy. More specifically, if a flip review misses the awarded consensus scoring category, however is within just one grading away from the consensus scoring category, then the reviewer of that flip will be awarded a share from the 20% low accuracy reviewer rewards pool. For example, if the reviewer consensus for a flip was scoring category 5 (flip grade of 3), then all flip reviewers who gave a scoring category of 4 to 7 (flip grades of 2 to 4) for that flip will be eligible for a share from the low accuracy reviewer rewards pool. Note, if a reviewer had already been awarded consensus reviewer rewards for a flip, then they are ineligible to also receive low accuracy reviewer rewards for that flip.

A couple of edge cases can result in rewards when consensus was not acheived for a flip:
1. A committee with a total of 2 consensus points or less (i.e. maximum of 2 humans, 1 human and 2 non-humans or 4 non-humans) can all recieve a share of the low accuracy rewards if the members scorings are all within 1 grade distance from one another.
2. A committee of just 1 member is eligible to recieve a share of the low accuracy rewards out of fairness.
<br/>

### Flip Author Identity Penalties and Rewards

It is a systemic risk to only deny rewards for consistent poor quality flip authors, because there will be some authors that are not incentivised by rewards, but rather by destroying the network. As such, going forth the flip producers who consistently produce the poorest quality flips, and who also represent a systemic risk to the network, will be severely penalised. Conversely, those authors that consistently produce the best quality flips, will be exceptionally rewarded. The author penalties and rewards will occur in two independent time horizons, as described:

**“Per epoch” flip author penalties and rewards**

5% of the poorest quality flip authors for the current epoch will not pass validation. Any validation rewards that were awarded to these authors for that epoch are stripped, pooled and equally shared amongst 5% of the best quality flip authors. The lowest and highest quality flip authors for the epoch are determined as such:

1. For each flip author, the median and average grade is calculated for their submitted flips for the epoch.
2. The authors are sorted, first by the median grade descending, then by the average grade descending, then by number of flips submitted ascending, then by last flip submission time ascending.
3. The bottom 5% of authors in that list are determined to be the worst flip authors.
4. The top 5% of authors in that list are determined to be the best flip authors.

Edge cases:
- Some of the worst flip authors may also be in the pool of worst reviewers for that epoch (See Reviewer Penalties and Rewards section below). In that case, the validation rewards stripped are split in half, with one half going to rewards pool for the best flip authors, and the other half going to rewards pool for the best reviewers.
- Some of the best flip authors may also be determined to be in the worst reviewers camp. In such a case, the identity will be disqualified from recieving the flip author identity rewards. And as such, the pool of flip author identity rewards will be shared by a pool of less than 5% of the best flip authors.

**“Per 5 epoch” flip author penalties and rewards**

The “per 5 epoch” author penalties and rewards are calculated with the exact same method as the “per epoch” author penalties and rewards, except for the following differences:

1. The calculations, penalties and rewards are performed at every 5th epoch interval, e.g. Epoch 95, 100, 105 etc. These epochs are aligned with the “Per 5 epoch” reviewer penalties and rewards (See Reviewer Identity Penalties and Rewards section below).
2. The calculations include all of the submitted flips for each author over the past 5 epochs.
<br/>

### Reviewer Identity Penalties and Rewards

Like with flip authoring, it is a systemic risk to only deny rewards for consistently inaccurate flip reviews. As such, reviewers will be penalised when consistently providing inaccurate reviews, and conversely will be rewarded when providing consistently accurate reviews.

**“Per epoch” reviewer penalties and rewards**

5% of the least accurate reviewers for the current epoch will not pass validation. Any validation rewards that were awarded to these reviewers for that epoch are stripped, pooled and equally shared amongst 5% of the most accurate flip reviewers. The least and most accurate reviewers for the epoch are determined as such:

1. For each reviewer, the number of inaccurate flip reviews are determined for the epoch. An inaccurate flip review is one that misses the awarded consensus scoring category by more than one grading.
2. All human reviewers are sorted, first the number inaccurate flip reviews descending, then by identity age ascending, then by SubmitLongAnswersTx submission time ascending.
3. All non-human-validator reviewers are sorted, first the number inaccurate flip reviews descending, then by identity age ascending, then by SubmitLongAnswersTx submission time ascending.
4. The human cutoff number of identities for reviewer penalties and rewards is calculated with the following equation: `Human_Cutoff_Number = 0.05 * ( Total_Validators_In_Epoch / ( 1 + ( Total_Non_Human_Validators_In_Epoch / ( 2 * Total_Humans_In_Epoch ) ) ) )`.
5. The non-human-validator cutoff number of identities for reviewer penalties and rewards is calculated with the following equation: `Non_Human_Cutoff_Number = 0.05 * ( Total_Validators_In_Epoch / ( 1 + ( ( 2 * Total_Humans_In_Epoch ) / Total_Non_Human_Validators_In_Epoch ) ) )`.
6. All reviewers in the human sorted list that are below the Human_Cutoff_Number are determined to be the worst human reviewers for the epoch. For example, if the Human_Cutoff_Number calculated was 60 identities, then the bottom 60 identities in the human sorted list are considered the worst human human reviewers of that epoch. Conversely, all reviewers in the human sorted list that are above the Human_Cutoff_Number are determined to be the best human reviewers for the epoch.
7. The same process as in point 6 above (albeit with the non-human-validator sorted list) is used to determine the worst and best non-human-validator reviewers.
8. The 5% worst reviewers for the epoch are the combined worst human and non-human-validator reviewers.
9. The 5% best reviewers for the epoch are the combined best human and non-human-validator reviewers.

Edge cases:
- Some of the worst reviewers may also be in the pool of worst flip authors for that epoch. In that case, the validation rewards stripped are split in half, with one half going to rewards pool for the best reviewers, and the other half going to rewards pool for the best flip authors.
- Some of the best reviewers may also be determined to be in the worst flip authors camp. In such a case, the identity will be disqualified from recieving the reviewer identity rewards. And as such, the pool of reviewer identity rewards will be shared by a pool of less than 5% of the best reviewers.

**“Per 5 epoch” reviewer penalties and rewards**

The “per 5 epoch” reviewer penalties and rewards are calculated with the exact same method as the “per epoch” reviewer penalties and rewards, except for the following differences:

1. The calculations, penalties and rewards are performed at every 5th epoch interval, e.g. Epoch 95, 100, 105 etc. These epochs are aligned with the “Per 5 epoch” flip author penalties and rewards.
2. The calculations include all of the reviewed flips for each reviewer over the past 5 epochs.
<br/>

### Validation Rewards Structure

The reviewer rewards (formerly report rewards) will be raised to 48% of the total validation rewards. This increase is designed to provide sufficient incentive for reviewers to participate in flip reviewing (without abstentions), and also to place effort into achieving accuracy with reviews. This proposal lays out the means for improving flip quality, but only insofar as people are willing to participate in the process.

Flip rewards will also be increased to 48% of validation rewards, for two reasons: 1) to make it economically feasible for more people to engage in flip creation as a means for income, and have a pathway to earn iDNA for mined staking rewards. This therefore increases competition against those that mass produce cheap templated flips. And 2) to motivate flip creators to put in significant effort into authoring quality flips out of fear from losing out on the generous flip rewards.

These reward changes will be accommodated by removing the following validation rewards:

- Staking rewards (18%). These rewards are redundant now that staked mining rewards have been implemented.
- Invitation rewards (18%). These rewards are wasted because people are sufficiently motivated to invite family/friends/strangers for reasons other than receiving a transactional reward.
- Idena foundation payouts (10%). These rewards are redundant because the Foundation Wallet has already accumulated ~1,418,000 iDNA, and the Zero Wallet continues to accumulate iDNA.

The new Validation Rewards structure is shown in Table 3 below:

*Table 3: The breakdown of total validation rewards.*
| Total Validation Rewards              | 100%   |
|---------------------------------------|--------|
| Candidate rewards                     | 2%     |
| Zero wallet fund                      | 2%     |
| Flip rewards – Highest tier           | 24.96% |
| Flip rewards – Second highest tier    | 12.96% |
| Flip rewards – Middle tier            | 6.72%  |
| Flip rewards – Second lowest tier     | 3.36%  |
| Reviewer rewards – Scoring category 1 | 7.68%  |
| Reviewer rewards – Scoring category 2 | 3.84%  |
| Reviewer rewards – Scoring category 3 | 3.84%  |
| Reviewer rewards – Scoring category 4 | 7.68%  |
| Reviewer rewards – Scoring category 5 | 3.84%  |
| Reviewer rewards – Scoring category 6 | 3.84%  |
| Reviewer rewards – Scoring category 7 | 7.68%  |
| Reviewer rewards – Low accuracy       | 9.6%   |
<br/>

## Rationale

The current protocol has measures in place to enforce flip quality, such as randomly assigned keywords designed to prevent flip re-use, flip qualification designed to prevent nonsense, and flip reporting designed to prevent ordering cues, and keyword non-compliance. Of these measures, it is flip reporting that has failed to prevent abuse, even with the severe penalisations already in place. The main reasons for failure are:

- A lack of clarity on what constitutes a reportable flip which contributes to both false negatives and false positives. In many instances poor quality flips are technically valid flips as per the reporting guidance provided. Worse still, expanding on the guidance is not expected to improve things because poor quality flips are in a spectrum of quality and thus what might be reportable to one person, might be acceptable to another. The guidance would need to be massively expanded to include all variations and examples of reportability, which would simply be impossible for most reviewers to accurately follow.
- The current implementation directly couples the successful reporting of a flip, to the rewards associated with reporting the flip, and moreover to the loss of rewards associated with the authoring of that flip. These direct couplings create a rigidity in the incentive structure making it impossible to effectively tailor incentives and penalisations. Current reporting deals with this rigidity by limiting the number of reports one can give during the long session to 1/3, as a compromise between having enough reports to avoid having false negatives, but not too many reports to avoid having false positives. Though this has proved to be an impossible balance to accommodate.
- Various other reasons such as allowing an identity to consistently submit poor quality flips, allowing identities to consistently make inaccurate flip reports, allowing inexperienced users to report flips, and having insufficient flip rewards and reviewer rewards.

These failures have resulted in a trajectory of lower overall flip quality where the current state of affairs is in disarray.

The solution herein provides an intuitive scoring system that takes into account the variation in flip quality. By providing users with simple guidance on what defines a clearly reportable flip, and conversely what defines a clearly ideal flip, users can then use their own intuition to score how well each flip fits into the relative spectrum of quality. The rewards received for accurate scorings will sharpen reviewers intuition for accuracy with scorings over time. Consequently, the advantages are immense for both flip reviewers and flip authors, the former having a user friendly manner upon which to provide feedback, and the latter having clarity into the measure of quality improvements needed.

The solution here also removes the direct coupling of review committee consensus from all flip and grading related rewards and penalisations, and therefore provides flexibility in the construction of an effective incentive structure. This significantly reduces false positives and false negatives, and thereby accurately rewards flip makers and flip reviewers, and accurately punishing identities with a consistent pattern of authoring poor flip quality. Flip grading results in a relative ranking between all flips known, thus flip rewards can be distributed in manner consistent with the ranked flip quality. This in-turn incentivises quality flip authoring as it greatly rewarded, and conversely disincentivises low quality flip production because it is not rewarded and potentially penalised. Moreover, giving more scoring weight of flips to experienced users (those with human status) is expected to better resolve the relative ranking of quality amongst flips, and thus reduce both false positives and false negatives when distributing the associated rewards and penalties.

With this solution, flip reviews can be rewarded for accuracy as determined by the level of agreement with other reviewers. This incentivises reviewers to not only grade flips, but to also be as accurate as possible so as to maximise the chance for rewards. Over time, the reviewers will have a refined intuitive sense of the deserving score for each flip that they encounter.

Moreover, this solution includes a massive increase in both flip rewards and reviewer rewards, which is needed to incentivise quality flip authoring, and accurate flip scoring. However, rewards alone are insufficient to ensure a trajectory of continual network flip quality improvements, for example abusers who want to destroy the network are not incentivised by rewards for good behaviour. Hence, penalties for consistent poor quality flip submissions and consistent inaccurate flip reviews is included to protect the network from such abuse.
<br/>

## Backward Compatibility

This change requires a hard fork.
<br/>

## Security Considerations

- This is a complex and nuanced proposal that fundamentally alters various aspects of the protocol and incentives. As such, a great deal of care needs to be taken during programming, testing and release in order to not introduce bugs.
- Low participation (high abstentions) in flip reviews has the potential to produce insufficient gradings of flips, which would have all sorts of negative flow-on effects. This has been mitigated by altering committee consensus to plurality (as opposed to absolute majority), significantly increasing reviewer rewards in general, and also by providing low accuracy reviewer rewards to ensure a level of incentive for merely participating with a reasonable level of accuracy.
<br/>
