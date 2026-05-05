---
title: "How We Test a Conversational API"
url: "https://foursquare.com/resources/blog/developer/how-we-test-a-conversational-api/"
date: "Wed, 29 Apr 2026 18:05:58 +0000"
author: "fkhan"
feed_url: "https://foursquare.com/feed/"
---
<p class="has-medium-font-size">Conversational APIs break most of the evaluation methods that search and recommendation teams have built up over the past two decades. When we started working on the <a href="https://foursquare.com/resources/blog/developer/introducing-the-foursquare-ask-api/">Foursquare ASK API</a>, our natural language place search endpoint, we quickly learned that we would need to take a new approach to evaluation. We couldn’t label a static ground-truth set the way we did for keyword search, because the query surface was effectively infinite. Similarly, in a pre-launch period, we didn’t yet have enough traffic to lean on behavioral signals like clicks and dwell time. To meet our standards of quality to take this offering to customers, we knew we needed defensibility.</p>



<p>So we built our own solution. In this blog, we’ll walk through what we built, what it produces, and what we’ve learned along the way. While our approach largely focuses on the use case of place search, there is much to be taken away from our journey to inspire your own evaluation methods specific to your products, your constraints, and your users.&nbsp;</p>



<div class="wp-block-spacer" style="height: 35px;"></div>



<hr class="wp-block-separator has-text-color has-black-color has-alpha-channel-opacity has-black-background-color has-background is-style-default" />



<div class="wp-block-spacer" style="height: 35px;"></div>



<h2 class="wp-block-heading" id="h-why-conversational-apis-were-hard-for-us-to-evaluate" style="font-size: 28px;">Why conversational APIs were hard for us to evaluate</h2>



<p>Traditional search evaluation leans on three distinct areas: labeled relevance judgments, behavioral signals like clicks and dwell time, and well-understood ranking metrics like NDCG or MRR. For the ASK API, all three were strained.</p>



<p>The query surface was wider than we were used to. For example, “Italian restaurants near me” is trivial to label. Whereas, “somewhere fun for a first date that is not too loud and has good cocktails in the Mission” is not. Labeling at scale for the latter class was expensive, and we watched labels age quickly as the system evolved.</p>



<p>The answer was not a single item. The user experience depends on intent interpretation, ranking, breadth of coverage, whether the system explains itself, and whether it stayed in the right geography. We saw plenty of cases where a system got the top result right and still produced a bad experience by missing everywhere else.</p>



<p>And the definition of correct was contextual. For example, “emergency plumber tonight”, urgency dominated. Whereas, “cocktail lounge in Miami”, breadth and variety mattered more than any single best answer. A metric that treated these the same would have systematically misled us.</p>



<p>All of this required that the framework we build have the ability to handle all of this nuance while being both cost-effective enough to run on every model or retrieval change, and rigorous enough that we’d trust its verdicts.&nbsp;</p>



<p>With that context, let’s dive into our solution and learnings along the way.</p>



<h2 class="wp-block-heading" id="h-we-compare-we-don-t-score" style="font-size: 28px;">We compare, we don’t score</h2>



<p>The first (and most vital) decision was to structure the evaluation as pairwise comparisons between two versions of the system, not as absolute scores against an ideal.</p>



<p>Pointwise scoring (“rate this result list from 1 to 5”) initially sounded simpler, but was noisier than we wanted. Judges—human or model—drifted in their calibration across sessions, across query types, and across days. Small but real improvements got lost in that drift. Pairwise comparison sidestepped the problem: the judge didn’t need to agree with itself on an absolute scale, only on which of two lists was better for a given query, and that turned out to be a much more stable task.</p>



<p>Pairwise also mapped more cleanly to the question we actually needed to answer. Instead of abstractly asking, “is this good”, we were asking, “Is this better than what we had before?” Pairwise evaluation answered that question directly.</p>



<h2 class="wp-block-heading" id="h-how-we-use-the-judge" style="font-size: 28px;">How we use the judge</h2>



<p>For anything above a modest scale, we needed an automated judge. We use an LLM as the judge, which trades away some per-case nuance for the ability to evaluate thousands of queries repeatably. That tradeoff has proven worthy due to our discipline in how we configure the judge—and we learned to be disciplined by making the mistakes first.</p>



<p>Three things have ended up mattering:</p>



<ul class="wp-block-list">
<li><strong>Determinism.</strong> Temperature set low, seeds fixed, prompts version-controlled. If we rerun the same evaluation tomorrow with no code changes, we want the same verdicts. Early on, we weren’t careful enough about this, and we couldn’t tell whether a change in the numbers came from the system under test or from judge variability. Once we tightened up the config, we could track progress over time without second-guessing it.</li>



<li><strong>Blinding.</strong> The judge never sees which system produced which result list. We shuffle the two sides on every query and strip any metadata that could leak the source. None of this is novel—it’s standard—but we underestimated early on how strong the bias toward the new one can be, and it was worth the engineering cost to do it properly.</li>



<li><strong>Calibration against human judgments.</strong> We check that the judge’s verdicts agree with thoughtful human reviewers on a sampled subset. When the judge has systematically over-weighted one dimension or misread a class of queries, we’ve wanted to know before we used it to make product decisions. We’ve caught issues this way, and they’ve always been worse than we expected going in.</li>
</ul>



<div class="wp-block-foursquare-multipurpose-gutenberg-block container" style="background-color: #d3eaff; padding-top: 40px; padding-right: 40px; padding-bottom: 40px; padding-left: 40px; margin-left: 0px;">
<p id="h-when-to-use-both-together" style="font-size: 18px;"><strong><strong><strong><strong><strong><strong>What worked for us. </strong></strong></strong></strong></strong></strong></p>



<div class="wp-block-spacer" style="height: 15px;"></div>



<p style="font-size: 14px;">We ask the judge for its confidence on every call, not just the winner. That one change unlocked one of the most useful views we built (more on that below). A judgment is only as useful as the confidence behind it, and the models we’ve used are able to produce a reasonable self-reported confidence when prompted for it.</p>
</div>



<h2 class="wp-block-heading" id="h-why-we-score-multiple-dimensions" style="font-size: 28px;">Why we score multiple dimensions</h2>



<p>A single “which is better” verdict is the goal, but this simplification hides the actual difference. So we ask the judge to score each list along five dimensions that, in our work on place search, have captured the real user experience well:</p>



<ul class="wp-block-list">
<li>Intent match — Do the results match what the user actually asked for?</li>



<li>Ranking quality — Are the best results surfaced at the top?</li>



<li>Coverage and diversity — Does the list cover the relevant space well without becoming repetitive?</li>



<li>Justification quality — Do supporting snippets or rationale help explain why each result is relevant?</li>



<li>Geo correctness — Are results correctly localized when geography is explicit in the query?</li>
</ul>



<p>These dimensions are specific to place search. What’s worked well for us is maintaining a small, fixed set of axes that collectively define what “good” looks like for our product—and scoring each one independently. This allows us to understand why a version performs better, not just that it does. For example, in our most recent ASK release, intent-match showed +5 deltas on queries like “family dentist near Sunnyvale, CA” and “zoo tickets in San Diego, CA”—cases where the previous system returned the wrong category of place entirely. Geo-correctness improvements were most noticeable in comparison queries such as “cheaper than X in city”, where staying anchored to the correct local market was critical. Those kinds of insights would have been difficult to capture using a single relevance score.</p>



<p>The dimensions also allow us to build a composite relevance score—an average of the per-dimension scores on a 0–1 scale. This approach smooths out the noise of binary win/loss counts and provides a more continuous quality signal for tracking performance over time.</p>



<h2 class="wp-block-heading" id="h-how-we-try-to-earn-our-conclusions" style="font-size: 28px;">How we try to earn our conclusions</h2>



<p>This was the hardest part for us to get right, and the one where we see teams (including past versions of ourselves) most often come up short. The trap is familiar: you produce a number, the number goes up, the team ships. But “went up” isn’t the same as “went up more than random variation would explain.”</p>



<p>Three statistical views we’ve come to rely on, and what they showed on our most recent ASK release:</p>



<figure class="wp-block-table is-style-stripes"><table class="has-background has-fixed-layout" style="background-color: #f9f9f9;"><thead><tr><th>View</th><th>What it tells us</th><th>Result</th></tr></thead><tbody><tr><td>Sign test on pairwise wins</td><td>Is the win margin unlikely to be random?</td><td><em>p</em> = 0.0017</td></tr><tr><td>Wilcoxon on composite deltas</td><td>Does the margin hold when we weight by size?</td><td><em>p</em> = 0.0001</td></tr><tr><td>Bootstrap 95% CI on composite delta</td><td>What’s the plausible range of the gain?</td><td>[+0.037, +0.099]</td></tr></tbody></table></figure>



<p>The sign test answers whether one version wins more often than chance. The Wilcoxon test uses the size of each win, not just the direction, so that a lot of small wins don’t outweigh a few big losses. The confidence interval tells us what the real improvement most plausibly is: if the interval excludes zero, we believe the effect is real; if the lower bound is only marginally above zero, we try to be honest about having a small effect dressed up in large-sample confidence.</p>



<p>Across these views, each answers a different question: does one version win more often, does it win by more, and is the average improvement itself meaningfully different from zero. We’ve learned to treat “yes” on all three as the bar for quality before defining an improvement as “better”. On the ASK release above, all three views agreed, and the lower bound of the CI sat comfortably above zero.</p>



<h2 class="wp-block-heading" id="h-why-we-slice-by-query-complexity" style="font-size: 28px;">Why we slice by query complexity</h2>



<p>An additional lesson we learned early: aggregate wins can hide asymmetric tradeoffs. A common failure mode in ranking systems is a change that makes hard queries much better, easy queries slightly worse, and still looks positive in aggregate. For a product where easy queries dominate real-world usage, that’s a silent regression we don’t want to miss.</p>



<p>Given this, we slice our benchmark into low, medium, and high complexity tiers and look at wins, composite deltas, and intent-match distributions in each. A change we’re comfortable shipping has to show meaningful gains at high complexity and not regress—ideally gain modestly—at low complexity. A rescue/loss analysis (how many easy queries moved from bad to good versus good to bad) has turned out to be a particularly sharp ship-safety check, because it surfaces movement that aggregate scores can mask.</p>



<p>On our most recent ASK release, the slice looked like this:</p>



<figure class="wp-block-table is-style-stripes"><table class="has-background has-fixed-layout" style="background-color: #f9f9f9;"><thead><tr><th>Complexity</th><th>Net win rate</th><th>Composite lift</th></tr></thead><tbody><tr><td>High</td><td>+27.5 pts</td><td>+0.101</td></tr><tr><td>Medium</td><td>+5.4 pts</td><td>+0.038</td></tr><tr><td>Low</td><td>+24.6 pts</td><td>+0.064</td></tr></tbody></table></figure>



<p>Within the low-complexity slice, the mean intent-match score rose from 4.23 to 4.54, and 13.1% of easy queries were rescued from a bad state while only 8.2% moved the other way. That shape—biggest gains on hard queries, no meaningful degradation on easy ones, and in fact net rescues there too—is what a change we’re ready to ship tends to look like for us.</p>



<h2 class="wp-block-heading" id="h-why-we-look-at-the-high-confidence-subset" style="font-size: 28px;">Why we look at the high-confidence subset</h2>



<p>Remember the confidence scores we ask the judge to produce? This is where they earn their keep.</p>



<p>After we compute headline results across the full benchmark, we rerun the same analysis on the subset of queries where the judge reported high confidence in its own call. There are two patterns we look for:</p>



<ul class="wp-block-list">
<li>If the improvement shrinks or disappears in the high-confidence subset, our overall win is being driven by borderline calls where the judge could plausibly have gone the other way. We treat that as a weak signal.</li>



<li>If the improvement gets stronger in the high-confidence subset, the clearest cases—the ones the judge was most certain about—are driving the result. That’s a strong signal.</li>
</ul>



<p>On our most recent ASK release, the full-benchmark net win rate was +18.5%, but when we restricted to the subset where the judge reported confidence of 0.80 or higher, the net win rate jumped to +34.0% with a 95.4% non-tie rate and 94.5% of the overall net wins came from that high-confidence subset. That told us the headline wasn’t a pile of coin-flip calls stacking in one direction by chance. The clearest judgments were pulling in the same direction as the aggregate.</p>



<h2 class="wp-block-heading" id="h-how-we-treat-regressions" style="font-size: 28px;">How we treat regressions</h2>



<p>Every pairwise comparison produces some number of cases where the new version is worse. Our instinct early on was to minimize that number, We’ve learned that the better instinct is not to explain them away. Handled carefully, the regression set has become the single most useful artifact the evaluation produces, because it tells us exactly what to work on next.</p>



<p>We categorize regressions along two axes: severity (does the user still get a usable answer, or did the product fail?) and reproducibility (does the failure happen every time, or only sometimes?). This gives us four quadrants, each with a different action we take:</p>



<figure class="wp-block-table is-style-stripes"><table class="has-background has-fixed-layout" style="background-color: #f9f9f9;"><thead><tr><th></th><th>Reproducible</th><th>Non-reproducible</th></tr></thead><tbody><tr><td>High severity</td><td>Prioritize fixing immediately</td><td>Treat as instability—underrated</td></tr><tr><td>Low severity</td><td>Backlog: edges of the envelope</td><td>Note and move on</td></tr></tbody></table></figure>



<p>In our most recent cycle of work, fewer than 9% of flagged regression cases landed in the critical-or-high-severity quadrant; the vast majority still produced usable results with varying degrees of quality degradation. The compound-concept pattern—where a query like “arcade bar in Los Angeles, CA” returns one strong match and then falls back to generic bars—sat in the reproducible quadrant and is now a named workstream on our roadmap. The non-reproducible cases—where severe misses on queries like “pizza places with gluten-free crust in Denver, CO” couldn’t be re-created on retest—pushed us to treat non-determinism as a distinct class of issue rather than filing it under regressions. Without the structured look, we’d have been working off anecdotes.</p>



<h2 class="wp-block-heading" style="font-size: 28px;">What this has given us</h2>



<p>The payoff from this kind of framework has been bigger for us than any one result it produces.</p>



<p>We iterate faster. When every model or retrieval change goes through the same benchmark, we spend less time trying to determine whether a change is actually producing better results and more time shipping the ones that are.</p>



<p>We make more quantitative product decisions. Rather than decision making led by opinion or intuition, our discussions have upleveled to those like, “We improved the composite score by 10% with a confidence interval that excludes zero. 94.5% of the net wins came from the judgments the evaluator was most sure about.” This changes how we talk about quality, both internally and with customers.</p>



<p>We get an honest read on work to be done. In practice, the regression set is one of the most useful outputs the framework produces. It tells us what the next cycle of work should be, and keeps us honest about the gap between where the product is and where we want it to be. The framework produces defensible evidence on data quality improvements for ourselves, customers, and partners alike – turning quality from a claim into a measurable metric.</p>



<div class="wp-block-foursquare-multipurpose-gutenberg-block" id="closing-CTA" style="background-color: #000024; background-position: center; padding-top: 50px; padding-right: 50px; padding-bottom: 50px; padding-left: 50px;">
<p class="has-text-align-left" id="h-"></p>



<p class="has-text-align-center has-white-color has-text-color has-link-color has-medium-font-size wp-elements-a58d55023f9cf8a30e656ac7cd0f18cb">The Foursquare ASK API is our natural language place search endpoint – powered by improvements validated with our evaluation framework.&nbsp; Working on something similar and want to trade notes?</p>



<div class="wp-block-spacer" style="height: 25px;"></div>



<div class="wp-block-spacer" style="height: 25px;"></div>



<div class="wp-block-foursquare-fsq-buttons fsq-buttons" id="fsq-buttons-3b266c03-3437-4d9f-8948-86e3d2c8dd95"><div class="container"><div class="button-row u-justify-content-center"><div class="button-item"><p class="btn-main"><a href="https://support.foursquare.com/hc/en-us/requests/new?ticket_form_id=13089705025180&amp;tf_subject=Ask%20Access&amp;tf_13089887414684=ask_enterprise_ga_dp">Contact us</a></p></div><div class="button-item"><p class="btn-main btn-secondary-white"><a href="https://foursquare.com/developers/login">Access Dev Console</a></p></div></div></div></div>




</div>



<p></p>
<p>The post <a href="https://foursquare.com/resources/blog/developer/how-we-test-a-conversational-api/">How We Test a Conversational API</a> appeared first on <a href="https://foursquare.com">Foursquare</a>.</p>
