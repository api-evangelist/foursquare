---
title: "Engineering the Spatial Foundation for FSQ OS Places — Part 2"
url: "https://foursquare.com/resources/blog/products/engineering-the-spatial-foundation-for-fsq-os-places-part-2/"
date: "Mon, 10 Nov 2025 17:10:00 +0000"
author: "fkhan"
feed_url: "https://foursquare.com/feed/"
---
<p class="has-medium-font-size" id="e143">In&nbsp;<a href="https://foursquare.com/resources/blog/products/engineering-the-spatial-foundation-for-fsq-os-places/" rel="noreferrer noopener" target="_blank">Part 1</a>&nbsp;of this series, we explored the first of three persistent spatial challenges facing Foursquare OS Places: fundamental inconsistencies in spatial attributes. We evaluated modern geocoding technologies, both open-source and commercial, to establish a transparent, verifiable foundation for resolving conflicts between a venue’s latitude/longitude and associated spatial attributes like zipcode, region, and locality.</p>



<p id="5553">That evaluation led us to partner with the team operating&nbsp;<a href="https://pelias.io/" rel="noreferrer noopener" target="_blank">Pelias</a>, replacing our legacy TwoFishes system with a solution that balances accuracy and transparency with a potential for community-driven improvement. That work answered one question: given a coordinate, what are the correct spatial attributes? But it left a harder question unresolved: given conflicting data sources, which coordinate is actually correct?</p>



<p id="01ff">This is what we call the&nbsp;<strong>micro-location</strong>&nbsp;challenge. When a place is situated within a shopping mall, a multi-tenant office building, or complexes with multiple entrances, the exact location of that place is hard to pinpoint because data sources often provide conflicting coordinates — some point to parking lots, others to building entrances, and still others to rooftop centroids. Traditional geocoding solutions often provide only broad building-level coordinates, which are insufficient for many modern applications that require higher precision. Foursquare’s Geosummarization model was developed to solve this challenge.</p>



<div class="wp-block-spacer" style="height: 35px;"></div>



<hr class="wp-block-separator has-text-color has-black-color has-alpha-channel-opacity has-black-background-color has-background is-style-default" />



<div class="wp-block-spacer" style="height: 35px;"></div>



<h2 class="wp-block-heading" id="95dc" style="font-size: 24px;">How Geosummarization Works</h2>



<p id="a2ef">Our Places Engine integrates diverse location data sources: user check-ins, web-crawled business listings, partner feeds, and real-time corrections from our global Placemaker network. Each source has its own methodology, precision level, and biases. A user might check in from a parking lot, a partner feed might provide rooftop coordinates, or a website could have outdated, manually placed pins. When these inputs cluster closely, consensus is straightforward. But when they scatter across a wider area, determining the correct location becomes complex. Geosummarization solves this by synthesizing conflicting or noisy location data into a single, definitive coordinate that accurately represents where a place physically exists.</p>



<h3 class="wp-block-heading" id="4fe8" style="font-size: 24px;">The Five Step Workflow</h3>



<p id="5c2f">The Geosummarization workflow operates in five steps: generate candidate locations, enrich them with spatial context, extract predictive features, score each candidate, and estimate confidence in the selected coordinate.</p>



<h3 class="wp-block-heading" id="b7bf" style="font-size: 20px;">Step 1: Candidate Generation — Mapping Inputs to H3 Cells</h3>



<p id="5026">We aggregate all geocode inputs — user check-ins, partner feeds, and web-crawled coordinates — and map each to its corresponding H3 cell. H3 is an open-source hierarchical indexing system that partitions the world into hexagonal cells at multiple resolutions. For every cell, we expand the candidate set by including cells from 5 neighboring concentric H3 rings from that cell. This allows the model to discover nearby locations that may be more accurate than any single input. For example, if sources cluster near a building’s corner, expansion ensures we also consider the building’s centroid.</p>



<h3 class="wp-block-heading" id="525c" style="font-size: 20px;">Step 2: Spatial Enrichment — Precomputing Context</h3>



<p id="d2ec">Each H3 cell is enriched with precomputed spatial attributes indexed to H3 cells from the&nbsp;<a href="https://foursquare.com/products/h3hub/" rel="noreferrer noopener" target="_blank">FSQ Spatial H3 Hub</a>:</p>



<ul class="wp-block-list">
<li>Building geometries: centroid locations, perimeters, interior/exterior relationships</li>



<li>Road networks: street segments, intersections, highway classifications</li>



<li>Address points: geocoded addresses cross-referenced with building parcels</li>



<li>Footfall estimates: aggregated mobile device telemetry indicating human activity</li>
</ul>



<p id="7894">By indexing spatial context to H3 cells in advance, we avoid expensive geometric operations at query time and make the workflow scalable across millions of places.</p>



<h3 class="wp-block-heading" id="92f2" style="font-size: 20px;">Step 3: Feature Generation — Five Questions That Define a Location</h3>



<p id="b4df">Before feeding data to the model, we transform each candidate cell into a feature vector by asking five concrete questions. The answers become the signals the model learns from:</p>



<p><strong><em>1. Is the cell physically where a place should be?</em></strong></p>



<p id="a1f6">We compute binary flags indicating whether the H3 cell intersects a building (hasBuilding), sits on a building centroid (hasCentroid), or lies within a roadway (hasRoad). If the place’s normalized address matches an address point inside that tile, we set the matchesAddress flag. Together, these features teach the model to distinguish “this looks like a rooftop” from “this is probably curbside parking.”</p>



<p id="a343"><em><strong>2. Does the venue’s popularity fit its surroundings?</strong></em></p>



<p id="122f">This feature combines monthly foot-traffic estimates (aggregated from mobile device telemetry) with the place’s own popularity. A high-traffic restaurant should fall in a busy tile; a quiet trailhead should not. This signal helps the model detect geocodes that are spatially implausible given a place’s visitation patterns.</p>



<p id="43cd"><em><strong>3. How concentrated are the third-party geocodes around this cell?</strong></em></p>



<p id="7a9c">As mentioned earlier, candidate cells enter the evaluation pipeline through two distinct mechanisms: direct citation by the participating sources, or inclusion via the spatial expansion process that generates concentric hexagonal rings (offsets 1–5) around each input. The critical distinction lies not in how a cell becomes a candidate, but in how we quantify the strength of spatial consensus converging upon it.</p>



<p id="5728">Each candidate cell receives a score based on how many data sources point to it and how close those sources are. When multiple sources point directly to the same cell (offset 0), that’s strong evidence the location is correct. When sources are farther away (offset 3–5), they provide weaker support — like a faint echo rather than a clear signal. The model learns to favor cells where inputs cluster tightly together over cells that only happen to be nearby scattered inputs. By tracking how inputs are distributed across these concentric rings, the model naturally learns that closer agreement matters more than distant proximity.</p>



<p id="3215"><em><strong>4. Which external sources vouch for this spot and how reliable are they here?</strong></em></p>



<p id="20be">For approximately 50 high-volume data providers, we set sparse binary flags. Because models are trained country-by-country, the system learns that certain sources excel in specific regions, for instance, source A might be highly accurate in France but noisy in Brazil, and adjusts weights accordingly.</p>



<p id="a249"><em><strong>5. What do the Placemakers say?</strong></em></p>



<p id="ce8a">Placemaker edits carry influence based on their level. They accumulate ‘voting energy’ as they prove their reliability with accurate edits over time. For each cell originating from a Placemaker edit, we track the number of votes and the aggregate voting energy by level (votesAtLevel1, votesAtLevel2, votingEneryAtLevel1, votingEnergyAtLevel2 and so on) and also capture whether the edit from the person who originally created the venue listing (has_creator) versus someone who reported a correction later (has_reporter). A candidate cell backed by multiple high-reputation contributors naturally outweighs one supported only by newcomers or low-trust accounts.</p>



<div class="wp-block-foursquare-cta-v9 cta-v9 block-align-left text-align-center has-overlay" id="cta-v9-949137c7-fd50-4f53-93de-b4b009ed6e9e"><div class="container"><div class="content-inner"><p class="description">When all five lines of inquiry are encoded, each [place, candidate cell] pair yields approximately 100 features.</p></div></div></div>



<h3 class="wp-block-heading" id="6f5d" style="font-size: 20px;">Step 4: Scoring and Selection — Ranking Candidates</h3>



<p id="e060">We model geosummarization as a pointwise regression task. A regression model predicts a quality score for each candidate cell.&nbsp;<em>The centroid of the highest-scoring cell is selected as the POI’s lat/lng</em>. Candidate-level training labels come from hand-annotated geocodes from trusted Placemakers. During training, we optimize not for overall regression loss, but for the score of the top-ranked prediction per place, a subtle but critical distinction that ensures the model learns to identify the best candidate, not just average ones.</p>



<h3 class="wp-block-heading" id="9ecf" style="font-size: 20px;">Step 5: Confidence Modeling — Ensuring a Safe Rollout</h3>



<p id="bf61">Identifying the optimal geocode is only half the challenge — the critical question remains whether that coordinate represents a genuine improvement worth publishing. To address this, we deploy a confidence meta-model that operates as a gating mechanism, estimating the probability that a newly selected geocode is both correct and superior to the existing coordinate for a given POI. This model uses various signals such as the selected cell score, directional alignment with input cluster, distance from the prior geocode value on the place and is trained on binary “win” labels from held out annotations. This secondary layer enables safe, precision-focused rollout at scale.</p>



<h2 class="wp-block-heading" id="c85c" style="font-size: 24px;">Evaluation and Impact</h2>



<p id="6417">While we measure decision quality through various model-centric metrics — tracking whether geocodes land on the correct building, place POIs on the proper side of the road, achieve rooftop precision, and maintain spatial consistency across our proprietary 0–1 quality score — the ultimate test is simpler and more direct: Are we actually closer to where the place really is? To answer this, we validate every geosummarizer output against held-out ground-truth, comparing distance (old_geocode, ground_truth) versus distance (new_geocode, ground_truth) and publishing only when new coordinates move demonstrably closer to reality.</p>



<p id="0ab9">When we rolled out our model in Germany, this translated to concrete gains: original geocodes averaged 14 meters from ground truth, while geosummarizer outputs averaged 9 meters — a 35% improvement in per-POI accuracy. This distance-based gateway complements our confidence meta-model by ensuring that internal model improvements — better feature representations, smarter consensus weighting — translate into real-world location accuracy that both our systems and stakeholders can trust. These results validate that the model doesn’t just score well internally; it demonstrably moves our coordinates closer to physical reality.</p>



<h2 class="wp-block-heading" id="d96f" style="font-size: 24px;">Seeing Geosummarization in Action</h2>



<p id="14c1">The best way to understand how geosummarization works is to see it visually. In this section, we will walk through a few examples.</p>



<h3 class="wp-block-heading" id="a293" style="font-size: 20px;">POI in a mall in Dresden</h3>



<p id="d5e1">Consider a place inside a mall in Bremen, Germany. Multiple data sources provide conflicting coordinates: crawled sources scatter across the rooftop and entrance, while a user check-in aligns with one crawled input. The model generates candidate cells around each input, scores them using spatial features (building centroids, address matches, input clustering), and selects the coordinate where signals converge most strongly — in this case, the building centroid where the majority of inputs align. (If you zoom into the image, you can see the blue outline corresponding to a crawl input on the pink dot related to the user input)</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-55648" height="822" src="https://foursquare.com/wp-content/uploads/sites/2/2026/01/POI-in-a-mall-in-Dresden.webp" width="1400" /></figure>



<h3 class="wp-block-heading" id="f6d9" style="font-size: 20px;">Standalone store in small town</h3>



<p id="7214">We’re geocoding a standalone pizzeria in Lüdinghausen, Germany. Crawled inputs cluster near one building, a user vote points to a neighboring building, and another crawled input sits across the street. The final geocode lands on the centroid of the building where most inputs converge and the address matches. The cell coloring shows the user input scores highly — the model rates a single first-party signal comparably to multiple third-party signals. Yet it doesn’t over-rely on that signal; consensus from other inputs remains decisive.</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-55650" height="822" src="https://foursquare.com/wp-content/uploads/sites/2/2026/01/Standalone-store-in-small-town.webp" width="1400" /></figure>



<h3 class="wp-block-heading" id="8e41" style="font-size: 20px;">User-Created Location in Crowded Urban Center</h3>



<p id="c47f">Here we’re geocoding a pediatric practice in Mannheim. The POI was created by an entry level Placemaker with low voting energy (no trust) who placed it at the building’s edge (pink dot near blue/yellow ones). A second Placemaker suggested the lower pink dot in the back alley — this was rejected by our community. The blue dot near the building center is the only web-crawled input. The model selects a cell near the third-party input, ignoring both first-party inputs. This demonstrates how the model prioritizes trustworthy third-party data when first-party inputs lack credibility in our system.</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-55652" height="822" src="https://foursquare.com/wp-content/uploads/sites/2/2026/01/User-Created-Location-in-Crowded-Urban-Center.webp" width="1400" /></figure>



<h2 class="wp-block-heading" id="156c" style="font-size: 20px;">The Path Forward</h2>



<p id="a2c6">By augmenting heuristic consensus with supervised machine learning, Geosummarization has transformed how Foursquare determines the definitive location of a place. The system synthesizes noisy, conflicting inputs into precise, trustworthy coordinates — and it does so transparently, with clear data lineage and explainable feature importance. This approach directly addresses the micro-location challenge we outlined in Part 1. Where traditional geocoding stops at building-level resolution, Geosummarization leverages spatial context — building centroids, address matches, road networks, and contributor trust — to pinpoint exact locations. The result is a spatial foundation that moves beyond the limitations of conventional geocoding.</p>



<p id="b3a4"><strong>Author: Vikram Gundeti</strong></p>



<p id="cb29"><strong>Contributors: </strong>Tele Onabajo, Kumar Somnath, James Bogart, Srdjan Radjocic, Steve Vitali.</p>
<p>The post <a href="https://foursquare.com/resources/blog/products/engineering-the-spatial-foundation-for-fsq-os-places-part-2/">Engineering the Spatial Foundation for FSQ OS Places — Part 2</a> appeared first on <a href="https://foursquare.com">Foursquare</a>.</p>
