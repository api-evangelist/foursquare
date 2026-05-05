---
title: "Introducing the Foursquare Ask API"
url: "https://foursquare.com/resources/blog/developer/introducing-the-foursquare-ask-api/"
date: "Tue, 10 Mar 2026 20:39:00 +0000"
author: "fkhan"
feed_url: "https://foursquare.com/feed/"
---
<h2 class="wp-block-heading" id="h-the-problem-when-search-systems-don-t-speak-human" style="font-size: 26px;">The Problem: When Search Systems Don&#8217;t Speak Human</h2>



<p>Place search has always struggled with a fundamental challenge: the gap between human intent and machine understanding. People describe what they want in natural, human terms, but most search systems speak in terms of categories, filters, and structured parameters like price ranges, and rating thresholds. Ask someone where to find &#8220;a cozy spot for a first date,&#8221; and they know exactly what you mean. Ask a traditional search API the same question, and it fails to understand the intent. These queries communicate atmospheric qualities and experiential expectations that cannot be easily translated into category and parameter constraints.</p>



<div class="wp-block-spacer" style="height: 25px;"></div>



<h2 class="wp-block-heading" id="h-the-solution-natural-language-search-that-actually-understands" style="font-size: 26px;">The Solution: Natural Language Search That Actually Understands</h2>



<p>Today we&#8217;re excited to announce the launch of the <strong>Foursquare Ask API</strong>, a natural language place search endpoint that fundamentally reimagines how developers build place discovery experiences. <strong>Instead of forcing users to translate intent into filters, the API speaks human.</strong> The API interprets the intent, identifies relevant concepts, and returns places that match what the user actually means.&nbsp;</p>



<p>The API delivers three transformative capabilities for developers:&nbsp;</p>



<ol class="wp-block-list">
<li><strong>Context-Aware Personalization Without Profiles</strong>: The optional context parameter enables query-specific personalization without requiring stored user profiles—a critical design decision for privacy and flexibility. Queries like &#8220;dinner near me&#8221; with context &#8220;allergic to gluten&#8221; deliver tailored results without user accounts or preference histories. The system treats contextual preferences as ranking signals, not absolute constraints—preserving serendipitous discovery while respecting stated needs.</li>



<li><strong>Transparent Justifications for Every Recommendation</strong>: Every recommendation comes with clear explanations of why a venue matched, citing specific user tips and relevant content rather than opaque similarity metrics. This transparency enables informed decision-making and establishes trust through complete auditability of the recommendation logic.</li>



<li><strong>Intelligent Adaptive Performance:</strong> The system automatically determines the appropriate execution strategy based on query complexity, without putting the onus on the end-user to make a decision between fast and thinking modes. Simple categorical searches execute in sub-200ms—matching traditional search performance. Complex multi-constraint queries like &#8220;somewhere my vegetarian friend and meat-loving husband will both enjoy&#8221; automatically trigger agentic reasoning, a multi-step process that breaks down competing requirements, evaluates trade-offs, and synthesizes balanced recommendations—completing in under 2 seconds.</li>
</ol>



<div class="wp-block-spacer" style="height: 25px;"></div>



<h2 class="wp-block-heading" id="h-real-world-example-finding-the-perfect-anniversary-dinner" style="font-size: 26px;">Real-World Example: Finding the Perfect Anniversary Dinner</h2>



<p>Traditional search APIs struggle with queries like &#8220;romantic Italian restaurant in Brooklyn for an anniversary dinner.&#8221; They might match &#8220;Italian&#8221; and &#8220;Brooklyn,&#8221; but miss the intent entirely. The Ask API recognizes what the user actually means: not just any Italian restaurant, but one with a specific atmosphere (romantic), suited to a special context (anniversary), in a particular location.&nbsp;</p>



<p>It first identifies concepts relevant to the user query like &#8220;date night,&#8221; &#8220;intimate setting,&#8221; and &#8220;candlelit atmosphere,&#8221; and finds the venues that genuinely match these characteristics. Then, it comes back with multiple recommendations ranked by relevance and each with a reason for why it is recommending that venue. Here’s what a typical recommendation looks like:</p>



<pre class="wp-block-code has-black-color has-text-color has-background has-link-color wp-elements-cebe3c8962809b307b3c02daf360c0b5" style="background-color: #f5f5f5;"><code>Locanda Vini &amp; Olli
- "Perfect for a romantic anniversary dinner. Known for its intimate candlelit atmosphere."

Why this venue?
- 15 users mention "romantic atmosphere" including: "The candlelit tables are incredibly romantic..."
- Ranked #2 for romantic places in Brooklyn</code></pre>



<p>Notice what makes this different: you&#8217;re not just getting a list of Italian restaurants. You&#8217;re getting venues that match the intent behind your query, with transparent explanations showing exactly why each place was recommended. Every recommendation cites real user experiences, not opaque algorithms.</p>



<div class="wp-block-spacer" style="height: 25px;"></div>



<h2 class="wp-block-heading" id="h-technical-deep-dive-building-explainable-natural-language-search" style="font-size: 26px;">Technical Deep Dive: Building Explainable Natural Language Search</h2>



<p>The capabilities demonstrated above—natural language understanding, transparent justifications, sub-200ms performance—required solving a fundamental architectural challenge. Traditional approaches force developers to choose between semantic intelligence and explainability. Vector search delivers nuanced understanding but operates as a black box. Structured search provides transparency but struggles with linguistic variation. We needed both. The obvious solution seemed straightforward: leverage modern LLMs by embedding all user tips as vectors, then perform similarity search at query time. This is how most semantic search systems work today—elegant, simple, and powerful. But this approach introduced three critical problems that would undermine our core requirements for transparency and performance.</p>



<h3 class="wp-block-heading" id="h-why-simple-vector-search-falls-short" style="font-size: 22px;">Why Simple Vector Search Falls Short</h3>



<p>Pure vector search fails for natural language place discovery in three fundamental ways:</p>



<ol class="wp-block-list">
<li><strong>Noise dilutes signal:</strong> Real user tips mix valuable information with conversational noise. Consider: <em>&#8220;Went here with my friend Sarah last Tuesday, the outdoor seating was nice but the waiter was slow”. </em>This tip contains one useful signal, &#8220;outdoor seating&#8221;, buried in irrelevant context.&nbsp;</li>



<li><strong>No aggregation:</strong> Venue characteristics emerge from consensus. Fifty independent users mentioning &#8220;outdoor seating&#8221; provides far stronger evidence than one eloquent description. But pure vector search treats each tip as an independent document. There&#8217;s no mechanism to recognize that 50 separate tips all reference the same attribute.</li>



<li><strong>Opaque black box:</strong> Vector similarity returns a score (e.g., 0.87) without explanation. Users can&#8217;t understand why a venue matched. Developers can&#8217;t debug unexpected results. The reasoning chain is completely opaque: you get a number, not an explanation citing specific tips and confidence levels.</li>
</ol>



<h3 class="wp-block-heading" id="h-our-hybrid-approach-knowledge-graph-vector-embeddings" style="font-size: 22px;">Our Hybrid Approach: Knowledge Graph + Vector Embeddings</h3>



<p>Pure vector search couldn&#8217;t deliver the transparency and performance we needed. Our solution combines two complementary layers:</p>



<p><strong>The Taste Graph</strong> extracts structured concepts from unstructured user content through statistical analysis. <strong>Statistically Interesting Phrase (SIP)</strong> identifies meaningful terms: &#8220;latte art&#8221; appearing 100x more in coffee shop tips than everyday conversation gets flagged as signal, while &#8220;went here last Tuesday&#8221; is filtered as noise. <strong>TF-IDF (Term Frequency-Inverse Document Frequency)</strong> then quantifies attribute strength: 25 mentions of &#8220;latte art&#8221; (rare and distinctive) score higher than 100 mentions of &#8220;good coffee&#8221; (common everywhere). </p>



<p><strong>Vector Embeddings</strong> translate natural language to structured concepts. We generate 1024-dimensional embeddings for every Taste Graph concept, enabling semantic matching: &#8220;romantic&#8221; maps to &#8220;Date Night&#8221; (0.89 similarity), &#8220;Intimate Atmosphere&#8221; (0.86), and &#8220;Candlelit&#8221; (0.83). Users express intent in their own words; the system translates to precise, verifiable concepts.&nbsp;</p>



<p>Together, the systems achieve what neither could alone: the Taste Graph provides statistical rigor, aggregation, and provenance; vector embeddings deliver semantic flexibility and linguistic understanding, creating explainable intelligence that operates at production speed.</p>



<figure class="wp-block-image size-full is-style-default"><img alt="" class="wp-image-56258" height="2081" src="https://foursquare.com/wp-content/uploads/sites/2/2026/03/Ask-API_Blog.png" width="1722" /></figure>



<h3 class="wp-block-heading" id="h-how-the-two-systems-work-together" style="font-size: 22px;">How the Two Systems Work Together</h3>



<p>Understanding this architecture becomes clearer by examining how these layers work together in practice. When a user searches for &#8220;romantic Italian restaurant in Brooklyn for an anniversary dinner&#8221; (our example from earlier), the system executes a four-step workflow that seamlessly integrates both components:</p>



<ol class="wp-block-list">
<li><strong>Extract concepts:</strong> The system identifies key concepts embedded in the user’s request: what makes a place romantic, what defines a dinner venue, what atmosphere they&#8217;re seeking.</li>



<li><strong>Map to attributes:</strong> Vector search finds semantically equivalent concepts in our Taste Graph: &#8220;romantic&#8221; maps to &#8220;Date Night&#8221; (0.91 similarity), &#8220;Intimate Atmosphere&#8221; (0.88), and &#8220;Candlelit&#8221; (0.83).</li>



<li><strong>Find matching venues:</strong> The system traverses the knowledge graph to find venues with strong affinities to these attributes, each carrying pre-computed TF-IDF confidence scores that quantify how distinctive these characteristics are for each venue.</li>



<li><strong>Generate justifications:</strong> Final results include contextual explanations citing specific user tips (&#8220;15 users mention &#8216;romantic atmosphere&#8217; including: &#8216;The candlelit tables are incredibly romantic&#8230;'&#8221;) combined with ranking signals like &#8220;#2 for Italian in Brooklyn.&#8221;</li>
</ol>



<p>See Ask API in action in the Foursquare Superlocal app in the video below:</p>



<figure class="wp-block-video"><video controls="controls" height="1080" src="https://foursquare.com/wp-content/uploads/sites/2/2026/03/superlocal-search-demo-1.mp4" width="1080"></video></figure>



<div class="wp-block-spacer" style="height: 25px;"></div>



<h2 class="wp-block-heading" id="h-the-future-of-conversational-place-discovery" style="font-size: 26px;">The Future of Conversational Place Discovery</h2>



<p>The Ask API represents our vision for conversational, contextually-aware place discovery—a system that understands not just what you ask for, but what you mean. We&#8217;re eliminating the cognitive burden of learning system vocabularies, moving toward a future where asking &#8220;where should we go?&#8221; becomes as natural as asking a knowledgeable local friend.</p>



<p>You can experience this natural language search in action through <strong>Superlocal</strong>, our consumer app built on the Ask API, where users are already discovering places through conversation rather than filters, or on <strong>Swarm</strong>, bringing natural language search to millions of users exploring their cities.</p>



<div class="wp-block-spacer" style="height: 25px;"></div>



<div class="wp-block-foursquare-multipurpose-gutenberg-block" id="closing-CTA" style="background-color: #000024; background-position: center; padding-top: 50px; padding-right: 50px; padding-bottom: 50px; padding-left: 50px;">
<h2 class="wp-block-heading has-text-align-center has-white-color has-text-color" id="h-get-started-with-the-ask-api" style="font-size: 26px;">Get Started with the Ask API</h2>



<div class="wp-block-spacer" style="height: 25px;"></div>



<p class="has-text-align-center has-white-color has-text-color has-link-color wp-elements-c8bcefa9254612692a1f6cd9cb948270">The Ask API is currently available to enterprise customers. Visit <a href="https://docs.foursquare.com/fsq-developers-places/reference/ask">our Docs site</a> to explore the Ask API documentation and <a href="https://support.foursquare.com/hc/en-us/requests/new?ticket_form_id=13089705025180&amp;tf_subject=Ask%20Access&amp;tf_13089887414684=ask_enterprise_ga_dp">contact our team</a> to request access for your application.</p>
</div>




<p>The post <a href="https://foursquare.com/resources/blog/developer/introducing-the-foursquare-ask-api/">Introducing the Foursquare Ask API</a> appeared first on <a href="https://foursquare.com">Foursquare</a>.</p>
