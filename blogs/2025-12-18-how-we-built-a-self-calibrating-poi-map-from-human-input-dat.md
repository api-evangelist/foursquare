---
title: "How We Built a Self-Calibrating POI Map from Human Input, Data Agents and AI"
url: "https://foursquare.com/resources/blog/products/how-we-built-a-self-calibrating-poi-map-from-human-input-data-agents-and-ai/"
date: "Thu, 18 Dec 2025 17:22:00 +0000"
author: "fkhan"
feed_url: "https://foursquare.com/feed/"
---
<p class="has-medium-font-size">One year ago, we introduced Foursquare’s new <a href="https://foursquare.com/resources/blog/products/foursquares-new-places-engine-crowdsourcing-redefined-with-human-ai-agent-collaboration/" rel="noreferrer noopener" target="_blank">Places Engine</a>, a unique crowdsourcing platform that brought together humans and agents to create a comprehensive POI (point-of-interest) dataset. We built this system to find <strong>consensus</strong> from conflicting inputs and anchored it with a strong <strong>spatial foundation</strong> to ensure every POI record in our database matches the physical reality. The result is something fundamentally different from a traditional POI system: a self-calibrating, living representation of POIs that continuously reasons about every input it receives. Rather than simply storing the data, the engine weighs new evidence against existing knowledge, calibrates the reliability of every contributor based on outcomes, and dynamically refines place records, all without manual intervention.</p>



<p>In this blog post, we explore how our <strong>central consensus engine</strong> operates as a meritocracy driven by inputs from three distinct types of contributors <strong>(Humans, Data Agents, and AI Agents)</strong>, and examine the <strong>workflows that enable continuous calibration and feedback</strong> between them.</p>



<div class="wp-block-spacer" style="height: 35px;"></div>



<hr class="wp-block-separator has-text-color has-black-color has-alpha-channel-opacity has-black-background-color has-background is-style-default" />



<div class="wp-block-spacer" style="height: 35px;"></div>



<h2 class="wp-block-heading" id="cfde" style="font-size: 24px;">The Three Types Of Contributors And The Consensus Engine</h2>



<p id="c94a">Our system receives proposals for new places or changes to existing places from three different types of contributors: a)&nbsp;<strong>Humans</strong>&nbsp;who use our apps and Placemaker tools to directly report changes to locations they know or vote on edits proposed by other users, b)&nbsp;<strong>Data Agents</strong>&nbsp;which monitor digital sources like websites, data feeds, and partner systems, match them against existing places in our database to understand differences and propose new places or edits to existing matched places and c)&nbsp;<strong>AI Agents</strong>&nbsp;which encapsulate various machine learning and large language models to either proactively report closures of places or vote on edits made by other contributors.</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-55671" height="667" src="https://foursquare.com/wp-content/uploads/sites/2/2025/12/How-We-Built-a-Self-Calibrating-POI-Map-from-Human-Input-Data-Agents-and-AI.webp" width="1400" /></figure>



<p id="675c">The central consensus engine evaluates these proposals (also known as ‘woes’) and decides whether to accept them based on alignment between different contributors. Rather than relying on a simple majority, the engine, built on a modified version of the Dawid-Skene algorithm, operates as a meritocracy: each contributor carries a trust score calibrated at the attribute level, giving greater weight to those with a proven track record of reliability. Once a decision has been made, the system calibrates every participant involved to either boost or penalize the trust score associated with that participant.</p>



<p id="47fa">Let’s say, a partner-feed reports that a neighborhood gym is “Open,” but a long-time Placemaker reports it as “Closed.” In a simple majority system, this might be a tie, but the resolution is a continuous loop of verification and grading:</p>



<ul class="wp-block-list">
<li><strong>First, the system acts as a judge</strong>. It weighs the conflicting testimony based on history. It recognizes that the Human Placemaker has a 99% accuracy rating for venue closures, while the automated feed has a history of lagging behind real-world changes. Based on this weighted evidence, the system determines that the gym is, in all probability, closed.</li>



<li>Once that truth is established, <strong>the system acts as a teacher</strong>. It effectively “grades” the contributors based on the ruling it just made. Since the Superuser was on the side of the truth, their history is reinforced, slightly boosting their influence for the next time. Conversely, the automated feed, having provided incorrect information against the consensus, takes a penalty to its reliability score. This ensures that if this specific feed continues to be wrong about closures, its voice will matter less and less in future decisions.</li>
</ul>



<p id="87fc">This judge-and-teacher loop serves as the foundation of the consensus engine. It also layers in additional calibration techniques to handle more nuanced scenarios detailed below:</p>



<ul class="wp-block-list">
<li><strong>Contextual Trust:</strong> First, it recognizes that trust is contextual rather than binary. Consider a business owner. They are the ultimate authority on their own Opening Hours, so the system assigns them a near-perfect trust score for that attribute. However, they are often unreliable when inputting their Venue Name, frequently cluttering the field with marketing slogans like “Best Pizza in NY” instead of the official registered name. The model accounts for this by maintaining separate trust scores for each contributor per attribute type, trusting the owner on logistics but verifying their strings against other sources.</li>



<li><strong>Cold Starting Data Agents:</strong> Second, the system solves the “cold start” problem for new Data Agents using Bayesian Priors. When a new data partner, such as a new Listing Syndicator, joins the network, we do not force them to start with a score of zero. Instead, we assign them a baseline reliability score (e.g., 60%) derived from similar existing agents and adjust it by validating the data against verified ground truth. This allows their data to be weighed immediately, while preventing them from overruling established Placemakers until they have proven their specific accuracy through repeated verification.</li>



<li><strong>Sparse Inputs:</strong> Finally, we prevent the system from becoming arrogant in sparse data environments. In a naive system, if only two agents vote on a venue closure and both say “Yes,” the system might claim 100% certainty. However, our system dynamically determines if that volume of evidence is sufficient to justify such a definitive conclusion by comparing it against the global baseline for similar decisions. If the data is sparse, the system anchors the probability to the global average, essentially saying, “Most businesses are open, and two votes aren’t enough to prove otherwise” and adjusts the confidence down to a more conservative level. As more votes accumulate, the evidence becomes sufficient to overcome this baseline, allowing the model to make a high-confidence decision. This ensures our dataset remains stable even when signals are few.</li>
</ul>



<p id="3477">With this foundation in place, we can now examine how each type of contributor interacts with the engine and how these feedback loops make them all smarter over time.</p>



<h2 class="wp-block-heading" id="aa98" style="font-size: 24px;">Human Contributors</h2>



<p id="2ee1">Humans serve as the definitive validators in our system, the source of truth when digital signals conflict or don’t exist yet. They are not merely another source in our system, they are the grounding anchors in our entire trust economy. Without reliable human judgment, the system would just be algorithms grading algorithms.</p>



<h3 class="wp-block-heading" id="17f4" style="font-size: 18px;"><strong>Hierarchy &amp; Privileges</strong></h3>



<p id="17f4">All human contributors, referred to as Placemakers, operate within a defined hierarchy ranging from Level 0 to Level 10. This progression reflects both the accuracy and volume of their contributions. A user typically begins as an apprentice (Level 0), interacting with the system by reporting new venues or voting on proposals from other Placemakers or Agents. As they accrue accurate contributions validated against the consensus model, they are promoted to higher levels. Reaching the upper echelons of this hierarchy (Level 8 and above) unlocks special privileges in an otherwise automated system. These senior Placemakers can unilaterally ban a destructive Data Agent or roll back a history of erroneous edits on a specific place. This structure effectively empowers our most trusted experts to act as system administrators for the physical world, creating a layer of human judgment that pure automation cannot replicate.</p>



<h3 class="wp-block-heading" id="9ee9" style="font-size: 18px;"><strong>Resolving Ambiguity</strong></h3>



<p id="9ee9">Beyond their administrative powers, Placemakers serve a critical function when the automated system reaches its limits. When the engine encounters ambiguous cases it cannot resolve confidently — a record that’s semantically similar to an existing place but lacks definitive proof of being the same location, or conflicting signals from multiple trusted sources — it routes the decision to humans for final judgment.</p>



<p id="dcc4">These edge cases are quite common. Two restaurants with similar names on the same street might be different locations, or one might be a rebrand of the other. A business listing with a slightly different address could be a duplicate entry, a recently moved location, or a separate branch. Rather than forcing a decision based on insufficient confidence, the system flags these cases for human review. Placemakers apply contextual knowledge that algorithms cannot access: local familiarity with the area, understanding of how businesses in that category typically operate, or even the ability to physically visit the location and verify the facts directly.</p>



<h3 class="wp-block-heading" id="1e9d" style="font-size: 18px;"><strong>Training the Agents</strong></h3>



<p id="1e9d">These human decisions do more than resolve individual cases — they actively improve the automated systems themselves. Inputs from senior Placemakers serve as high-confidence ground truth data for two primary purposes. First, they train and fine-tune our AI Agents, ensuring that human expertise is continuously embedded into the models themselves. Second, they help cold start the trust score of Data Agents by validating their inputs against the ground truth dataset.</p>



<p id="0086">Crucially, every time a Placemaker validates or rejects a proposal from a Data Agent or AI Agent, that decision feeds back into the system, recalibrating the agent’s trust score and sharpening its future performance. This creates a virtuous cycle where human judgment doesn’t just correct individual errors but actually teaches the system to make fewer errors over time.</p>



<h2 class="wp-block-heading" id="0716" style="font-size: 24px;">Data Agents</h2>



<figure class="wp-block-image"><img alt="" src="https://miro.medium.com/v2/resize:fit:1400/1*DCrowjy3oKrRpVoR5MRn8g.png" /></figure>



<p id="f89a">Keeping pace with the real world requires monitoring millions of external sources such as websites, listing syndicators &amp; open datasets and reconciling what they report against what we already know. Data Agents handle this at scale, but before any external data can enter the consensus engine, it must first be cleaned, normalized, and matched to our existing records.</p>



<h3 class="wp-block-heading" id="52d8" style="font-size: 18px;"><strong>Normalization</strong></h3>



<p id="52d8">Every external source has its own schema, update cadence, and quirks. A listing syndicator might structure addresses differently than a travel website; a partner feed might report coordinates that place a venue in the wrong zipcode. Data Agents create a unified abstraction layer over this heterogeneity — transforming messy, inconsistent inputs into standardized proposals that the rest of the system can reason about. This normalization process includes validating spatial attributes against open geocoders, ensuring that a venue’s latitude, longitude, address, and postcode actually align with each other and with the physical world. (See our recent post on <a href="https://foursquare.com/resources/blog/products/engineering-the-spatial-foundation-for-fsq-os-places/" rel="noreferrer noopener" target="_blank">fixing spatial inconsistencies through open geocoders</a> for more details on this topic).</p>



<p id="eae2">Once normalized, Data Agents systematically match this external data against our existing Places database, identifying any discrepancies between the source and our records. Based on these differences, they propose entirely new places if no match is found, or suggest edits to the attributes of existing places where new information is available.</p>



<h3 class="wp-block-heading" id="7d67" style="font-size: 18px;"><strong>Harmonization</strong></h3>



<p id="7d67">Matching records from different sources is difficult. Traditional approaches rely on fuzzy matching — drawing a radius around a coordinate and performing a keyword search on the name. This method breaks when digital feeds default a “New York” location to a city center five miles away, or when names lack keyword overlap like “JFK Airport” versus “John F. Kennedy International.”</p>



<p id="c87f">We address this through semantic entity resolution using Sentence Transformer models. These models convert a location’s entire record — name, address, and category — into an embedding that captures semantic meaning rather than just spelling. The system uses a two-stage process. First, a bi-encoder searches our entire database for places with similar embeddings, regardless of coordinate distance. Second, a cross-encoder examines each potential match more closely, catching subtle distinctions like “Walmart” the store versus “Walmart Bakery” inside it. This approach delivered a 10% increase in recall over fuzzy matching, capturing thousands of legitimate matches that would otherwise be missed.</p>



<p id="df3c">We recently packaged our harmonization algorithm as a standalone service. This&nbsp;<a href="https://foursquare.com/resources/blog/data/introducing-the-new-fsq-place-match-api/" rel="noreferrer noopener" target="_blank">blog post</a>&nbsp;covers both the service details and how the algorithm works under the hood.</p>



<h3 class="wp-block-heading" id="2bc8" style="font-size: 18px;"><strong>Resolution</strong></h3>



<p id="2bc8">Once the relationship between the incoming record and our database is determined, the system routes the data into one of three paths:</p>



<ul class="wp-block-list">
<li>Matched records trigger a differential analysis. If a partner feed matches a known bakery but lists a new phone number, the system isolates this specific difference and proposes it as an edit. This edit enters the consensus engine for voting.</li>



<li>Ambiguous records lack definitive proof of their identity. A “Starbucks” record with no address cannot be matched confidently. These records route to a holding queue where they await human verification. Placemakers review these edge cases to manually confirm or reject the match, and this ground truth data feeds back into the model to improve future predictions.</li>



<li>New records that show high confidence and clear distinction from all existing embeddings — like a trusted travel partner sending a complete record for a newly opened hotel — bypass the queue entirely. The system creates a new place immediately, assuming the source meets baseline trust criteria.</li>
</ul>



<p id="2201">This workflow functions as an automated quality control system. If a typically reliable source begins providing lower-quality data — consistently reporting outdated business hours for a specific retail chain, for example — the consensus model rejects the resulting proposals. This triggers the self-correcting feedback loop: the source’s trust score drops immediately. A lower score means the system no longer accepts the source’s inputs automatically. Future proposals from that source require significantly more corroboration from other high-trust agents or humans before entering the final dataset.</p>



<h2 class="wp-block-heading" id="911b" style="font-size: 24px;">AI Agents</h2>



<p id="cfca">Unlike Data Agents, which respond to what external sources report, AI Agents are proactive. They infer facts from indirect signals before any source explicitly confirms them. They operate in an active learning loop: proposing edits to be verified by Placemakers, then using those decisions to retrain their models regularly. Our AI Agents span multiple functions, from predicting closures to refining coordinates to filtering inappropriate content.</p>



<h3 class="wp-block-heading" id="a1ba" style="font-size: 18px;"><strong>Open/Closed Agent</strong></h3>



<p id="a1ba">The Open/Closed Agent predicts whether a venue has permanently closed by analyzing signals such as declining foot traffic or cessation of check-ins. It is calibrated against the imbalanced nature of reality, where most places remain open, and proposes “Closed” edits only when confidence exceeds a high threshold. When a Placemaker validates the closure, the model is reinforced. When they reject it — perhaps identifying a temporary renovation rather than permanent closure — that rejection becomes a training example that teaches the model to distinguish between temporary pauses and permanent closures.</p>



<h3 class="wp-block-heading" id="b609" style="font-size: 18px;"><strong>Geosummarizer Agent</strong></h3>



<p id="b609">This agent focuses on spatial precision, attempting to move map pins from street-level approximations to exact building rooftops. It evaluates coordinates based on a “geocode quality score” that rewards placement on the correct building footprint and side of the road. When the agent calculates that a new coordinate is significantly closer to the ground truth (e.g., moving 14m closer), it proposes a location update. Placemakers verify these moves from time-to-time, ensuring that the agent only scales up when it demonstrably improves physical accuracy. This work complements our broader effort to ground every record in spatial reality, as described in our post on <a href="https://medium.com/@foursquare/engineering-the-spatial-foundation-for-fsq-os-places-8d7dcf6e03fe">Engineering the Spatial Foundation for FSQ OS Places: Part 2</a>.</p>



<h3 class="wp-block-heading" id="1d0a" style="font-size: 18px;"><strong>Profanity Detection Agent</strong></h3>



<p id="1d0a">This agent polices the dataset for toxic content in names or comments. Leveraging open-source multilingual toxicity models (e.g., Kaggle Jigsaw, TextDetox), it scans incoming text and proposes deletions when toxicity is detected with high precision (>90% AUC). While currently based on general language models, the feedback loop is critical here: Placemaker decisions on borderline cases (such as a bar with a provocative name) provide the nuanced ground truth needed to fine-tune a domain-specific toxicity model that understands the difference between a slur and a valid venue name.</p>



<h3 class="wp-block-heading" id="d057" style="font-size: 18px;"><strong>Reprompt Agent</strong></h3>



<p id="d057">Reprompt represents a new generation of AI Agent: our first LLM-based Placemaker. Unlike the specialized ML models above, it can reason about unstructured text — automating the extraction of data from websites and social media sources that would be opaque to traditional models. It uses a comprehensive index of 10 million sites to verify POI attributes with 97% accuracy, requiring multi-source corroboration before proposing edits. We specifically leverage Reprompt as a high-fidelity signal for Open/Closed determinations, utilizing its ability to parse social media posts (e.g., “permanent goodbye”) that traditional models miss. Like human Placemakers, Reprompt earns trust over time — it started as an apprentice at Level 0 and climbed to Level 3 through verified accuracy. Human feedback on its proposals directly fine-tunes the LLM, improving its handling of regional nuances.</p>



<p id="4733" style="font-size: 18px;"><strong>Why This Matters</strong></p>



<p id="4733">Traditionally, ML models in this space are integrated as “last mile” measures — filters that silently clean data just before it is published. This approach is static; if the model makes a mistake, the error is either buried or requires manual engineering to fix. By deploying our models as Agents in the Loop, we transform them from passive filters into active participants in the consensus process. They propose edits that are visible, vote-able, and verifiable. This unique architecture means every interaction — every acceptance and every rejection — feeds directly back into the model’s training set. And because the highest-confidence training signal comes from senior Placemakers, the models inherit human expertise at scale. The result is a self-reinforcing cycle of improvement that static filters cannot replicate.</p>



<figure class="wp-block-image"><img alt="" src="https://miro.medium.com/v2/resize:fit:1400/1*-r7ifIqBF91xVgwT7ofajg.png" /></figure>



<h2 class="wp-block-heading" id="6c42" style="font-size: 24px;">Moving Beyond Traditional Conflation</h2>



<p id="52e9">Traditional location databases rely on static conflation, a rigid hierarchy where specific sources are designated as “trusted” based on business contracts rather than actual performance. In these systems, mistakes persist indefinitely because there is no mechanism to catch them retroactively without manual engineering intervention. When a trusted provider sends bad data, that data overwrites the database, and the error remains until someone notices and an engineer writes a patch to fix it. When two sources disagree, the winner is determined by hierarchy, not accuracy.</p>



<p id="756e">Our system operates differently. Errors are caught by Placemakers in the consensus loop. When a Placemaker corrects a mistake — rejecting a bad merge or fixing incorrect coordinates — the system immediately downgrades the trust score of the source that provided the bad data. This automatic calibration protects the database against future errors from that provider without requiring any code changes. The system learns from its mistakes and adjusts accordingly.</p>



<p id="46af">This architecture offers a critical advantage for businesses building on location data: resilience. In a static system, data quality depends entirely on the quality of input providers. If one feed degrades, the product degrades with it. In the Places Engine, the database is self-correcting. The system automatically isolates unreliable signals and amplifies accurate ones, ensuring that the data reflects a verified, high-fidelity representation of the physical world. This distinction separates a static database from a living system that improves continuously.</p>



<h2 class="wp-block-heading" id="906e" style="font-size: 24px;">Conclusion</h2>



<p id="1e2c">The Foursquare Places Engine represents a shift from static conflation based systems to a self-correcting architecture. By converting every input — whether from a digital source, a machine learning model, or a human user — into a proposal that requires verification, we’ve built a system that reasons about data rather than simply ingesting it. The database learns from every interaction. When a Placemaker corrects an agent, the agent adjusts its behavior. When a source degrades, the system automatically reduces its influence. These feedback loops protect against bad data and maintain accuracy at scale.</p>



<p id="262f">This architecture addresses one critical challenge: defending against errors. The second challenge is acceleration — increasing the volume and speed of feedback flowing into the system. In a subsequent post, we will explore how our investments in tooling, community, and automation are designed to accelerate the system’s evolution while maintaining its accuracy.</p>



<div class="wp-block-foursquare-multipurpose-gutenberg-block" id="closing-CTA" style="background-color: #000024; background-position: center; padding-top: 50px; padding-right: 50px; padding-bottom: 50px; padding-left: 50px;">
<h2 class="wp-block-heading has-text-align-center has-white-color has-text-color" id="h-learn-more-about-foursquare-places-or-get-started-in-our-places-portal" style="font-size: 26px;">Learn more about Foursquare Places, or get started in our Places Portal.</h2>



<div class="wp-block-spacer" style="height: 25px;"></div>



<div class="wp-block-foursquare-fsq-buttons fsq-buttons" id="fsq-buttons-3b266c03-3437-4d9f-8948-86e3d2c8dd95"><div class="container"><div class="button-row u-justify-content-center"><div class="button-item"><p class="btn-main"><a href="https://foursquare.com/products/places/">FSQ Places</a></p></div><div class="button-item"><p class="btn-main btn-secondary-white"><a href="https://places.foursquare.com/">Places Portal</a></p></div></div></div></div>




</div>
<p>The post <a href="https://foursquare.com/resources/blog/products/how-we-built-a-self-calibrating-poi-map-from-human-input-data-agents-and-ai/">How We Built a Self-Calibrating POI Map from Human Input, Data Agents and AI</a> appeared first on <a href="https://foursquare.com">Foursquare</a>.</p>
