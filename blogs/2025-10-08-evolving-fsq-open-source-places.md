---
title: "Evolving FSQ Open Source Places"
url: "https://foursquare.com/resources/blog/data/evolving-fsq-open-source-places/"
date: "Wed, 08 Oct 2025 18:51:17 +0000"
author: "nmiller"
feed_url: "https://foursquare.com/feed/"
---
<p class="has-medium-font-size"><em>In November 2024, we took a bold step in open-sourcing our Places dataset with a hypothesis that a community driven approach is the only way to create a sustainable and robust places dataset that can serve the needs of diverse problem domains. Our thesis has been validated with the tremendous progress we have been able to make in improving this open dataset since its inception.</em></p>



<ol class="wp-block-list">
<li><strong>Increased adoption:</strong> Our S3 listing is downloaded from more than 5000 unique IPs every month, a steady increase from 500 unique IPs when we first launched. More than 250k queries are executed on our Snowflake listing every month. And we have 3000+ monthly downloads of our dataset on HuggingFace.</li>



<li><strong>Accelerated data improvements:</strong> We continuously refined our dataset adding more than a million places since launch with a monthly high of 160k+ new places just in September. More than 27 million edits have been proposed by our Placemaker community in the last year out of which close to 17 million have been resolved.</li>



<li><strong>Diversified Placemaker community:</strong> We have been able to diversify our community from just the users of our apps to a broader user base with around 2000 Placemaker sign-ups from the open-source community using our Placemaker tools. We have also seen business owners gravitating towards our Placemaker Tools to update their own listings.</li>
</ol>



<p>Today, we’re taking the next step in that journey with updates that will strengthen our collaborative ecosystem and unlock even greater potential for data improvement.</p>



<h3 class="wp-block-heading" id="h-the-power-of-placemaker-tools">The Power of Placemaker Tools</h3>



<p>At the heart of our community-driven approach are our <a href="https://opensource.foursquare.com/placemaker/" rel="noreferrer noopener" target="_blank">Placemaker Tools</a>, which enable anyone to contribute to and improve the OS Places dataset. Through these tools, contributors can:</p>



<ul class="wp-block-list">
<li><strong>Add new places</strong>: Submit businesses, landmarks, and points of interest missing from the dataset</li>



<li><strong>Update existing information:</strong> update addresses, phone numbers, business hours, and other details</li>



<li><strong>Verify and validate: </strong>Confirm the accuracy of place information through community consensus</li>



<li><strong>Enhance place details:</strong> Add categories, attributes, and rich contextual information</li>



<li><strong>Report closures: </strong>Flag businesses that have permanently closed or relocated</li>
</ul>



<p>These tools have proven that when given accessible ways to contribute, people eagerly improve the data that powers their favorite applications. The 27 million edits proposed through Placemaker Tools demonstrate the power of community-driven data maintenance at scale. As we evolve our approach, Placemaker Tools remain central to our strategy. By creating stronger connections between dataset consumers and the improvement ecosystem, we can channel community efforts where they matter most, whether that’s improving coverage in underserved regions, updating time-sensitive information, or enriching place details for specific use cases.</p>



<h3 class="wp-block-heading" id="h-unveiling-the-next-nbsp-chapter">Unveiling the Next&nbsp;Chapter</h3>



<p>Our October release transitions FSQ OS Places from public S3 bucket access to our new Places portal. Users can register, generate an access token, and retrieve the data through an Iceberg catalog. The data remains <strong>completely free</strong> with proper attribution under our Apache 2.0 license.</p>



<h3 class="wp-block-heading" id="h-why-this-evolution-matters">Why This Evolution Matters</h3>



<p>Over the past year, we’ve seen incredible adoption of our dataset across diverse industries and use cases. But despite the widespread adoption of our OS Places dataset, the end users of applications built on this dataset rarely know it is even sourced from Foursquare or that they have an opportunity to improve it using our Placemaker Tools. In our current anonymous distribution model, there’s no path from “using an app with great location data” to “helping improve that location data.”</p>



<p><strong>A root cause here is a lack of awareness</strong>. When users discover that their favorite applications are powered by a community-driven dataset, many become eager contributors through our Placemaker Tools. This creates a virtuous cycle: better data enables better applications, which reach more users, who contribute more improvements, creating even better data.</p>



<p>That is why we’re introducing this new approach to build connections with the direct consumers of our dataset and work together to spread awareness of FSQ OS Places and empower their user communities to contribute through Placemaker Tools.</p>



<h3 class="wp-block-heading" id="h-what-this-means-for-nbsp-you">What This Means for&nbsp;You</h3>



<h4 class="wp-block-heading" id="h-accessing-the-nbsp-dataset">Accessing the&nbsp;dataset</h4>



<p>FSQ OS Places will now be accessible through three primary channels:</p>



<ol class="wp-block-list">
<li><strong>New Places Portal: </strong>Visit our new <a href="http://places.foursquare.com" rel="noreferrer noopener" target="_blank">portal</a> to create your free account and generate an access token. Use the access token to retrieve OS Places data from our Iceberg catalog.</li>



<li><strong>Snowflake: </strong>Access OS Places data from the <a href="https://app.snowflake.com/marketplace/listing/GZT0ZHT9O0H/" rel="noreferrer noopener" target="_blank">Snowflake</a> marketplace with the same ease as before.</li>



<li><strong>HuggingFace: </strong>Get approved for access to the OS Places data through <a href="https://huggingface.co/datasets/foursquare/fsq-os-places" rel="noreferrer noopener" target="_blank">HuggingFace</a> by providing your contact information.</li>
</ol>



<h4 class="wp-block-heading" id="h-attributing-to-foursquare">Attributing to Foursquare</h4>



<p>The data remains completely free with proper attribution under the Apache 2.0 license. To ensure required attribution, we recommend the following:</p>



<ul class="wp-block-list">
<li>If using/distributing the dataset in flat file form as-is or after making changes/modifications: include this <a href="https://opensource.foursquare.com/places-notice-txt/" rel="noreferrer noopener" target="_blank">NOTICE.txt</a> file, which may be modified to include an additional notice of your changes/modifications, if any.</li>



<li>If using/distributing the data in API form as-is or after making changes/modifications: include a copy of the content from this <a href="https://opensource.foursquare.com/places-notice-txt/" rel="noreferrer noopener" target="_blank">NOTICE.txt</a> file prominently in your developer documentation for such API, which may be modified to include an additional notice of your changes/modifications, if any.</li>
</ul>



<h4 class="wp-block-heading" id="h-connecting-with-the-community">Connecting With The Community</h4>



<p>This new approach creates direct connections between you, the dataset consumers, and our Placemaker community. When we know which applications are powered by FSQ OS Places, we can help you surface improvement opportunities to your end users, empowering them to become contributors who enhance the data that powers their favorite applications.</p>



<p>When we understand what matters to you — accurate business closures, comprehensive category coverage, and more — we can channel the community’s efforts where they will have the greatest impact. Your needs inform community priorities, their contributions power your applications, and your users become their fellow contributors. The cycle strengthens itself. Join our <a href="https://discord.gg/SYgqsUcJ" rel="noreferrer noopener" target="_blank">Placemaker Discord</a> to connect directly with the community and collaborate on data improvements.</p>



<h3 class="wp-block-heading" id="h-transition-support">Transition Support</h3>



<p>We are committed to ensuring a smooth transition for everyone. You can access the OS Places data from the past releases through our public S3 bucket but all future releases (starting from October 2025) will only be accessible through the three supported channels.</p>



<p><strong>The September 2025 release is already available through our new portal,</strong> giving you immediate access to our latest data while you plan your transition. If you have production workloads built on top of our public S3 dataset, please join our <a href="https://foursquare-os.slack.com/ssb/redirect" rel="noreferrer noopener" target="_blank">open source slack</a> and reach out to us at <a href="mailto:os-places@foursquare.com" rel="noreferrer noopener" target="_blank">os-places@foursquare.com</a> and we will work with you to ensure continuity of business operations.</p>



<h3 class="wp-block-heading" id="h-the-road-nbsp-ahead">The Road&nbsp;Ahead</h3>



<p>This next chapter strengthens everything that makes FSQ OS Places powerful: community collaboration, open access, and continuous improvement. By understanding real-world usage patterns, we can identify data gaps and connect users directly with Placemaker Tools. Every application becomes a gateway for new contributors, and every contribution amplifies the value for all users.</p>



<p>Together, we’re building not just a dataset, but a sustainable ecosystem where location data gets better every day through the collective efforts of a global community.</p>



<p>Visit the new Places Portal to <a href="https://places.foursquare.com/" rel="noreferrer noopener" target="_blank">get started</a>.</p>
<p>The post <a href="https://foursquare.com/resources/blog/data/evolving-fsq-open-source-places/">Evolving FSQ Open Source Places</a> appeared first on <a href="https://foursquare.com">Foursquare</a>.</p>
