---
title: "Introducing FSQ Spatial Agent: Your Geospatial AI Analyst"
url: "https://foursquare.com/resources/blog/ad-tech/introducing-fsq-spatial-agent-your-geospatial-ai-analyst/"
date: "Wed, 11 Feb 2026 21:35:25 +0000"
author: "fkhan"
feed_url: "https://foursquare.com/feed/"
---
<p class="has-medium-font-size">When we launched the <a href="https://foursquare.com/resources/blog/news/introducing-the-fsq-spatial-product-family/">FSQ Spatial Product Family</a> last July, we made a deliberate choice to build specialized tools for distinct personas rather than a one-size-fits-all platform: FSQ Spatial Workbench for BI analysts who think in SQL, FSQ Spatial H3 Hub for data scientists who need analysis-ready datasets without months of data engineering, and FSQ Spatial Desktop for GIS professionals who need the full power of spatial operations on their laptops.&nbsp;</p>



<p style="font-size: 16px;">While we solved the tooling and data engineering challenges with our product suite, we encountered an expertise gap that hindered organizations from leveraging these tools and benefiting from the pre-processed data. A retail chain&#8217;s real estate team knows they need to understand foot traffic patterns around potential store locations, but they don&#8217;t write SQL. An insurance underwriter understands flood risk conceptually, but can&#8217;t construct the spatial joins required to combine FEMA data with property parcels and demographic information. An urban planner can articulate what makes a neighborhood &#8220;underserved&#8221;, but can&#8217;t translate that into queries across census data, infrastructure datasets, and points of interest.</p>



<p style="font-size: 16px;">Today, we&#8217;re announcing FSQ Spatial Agent, built to bridge this gap for both decision-makers and analysts. For domain experts, it translates questions directly into spatial analysis, eliminating the technical barrier between insight and action. For data analysts, it bootstraps complex analyses, handling the initial data exploration and query construction so they can focus on refinement and strategic problem-solving. Where our existing products provide the tools, FSQ Spatial Agent operates them, accelerating decisions for business users and jumpstarting workflows for technical teams.</p>



<div class="wp-block-spacer" style="height: 35px;"></div>



<hr class="wp-block-separator has-text-color has-black-color has-alpha-channel-opacity has-black-background-color has-background is-style-default" />



<div class="wp-block-spacer" style="height: 35px;"></div>



<div class="wp-block-foursquare-multipurpose-gutenberg-block container" style="background-color: #f9f9f9; padding-top: 40px; padding-right: 40px; padding-bottom: 40px; padding-left: 40px; margin-left: 0px; border-style: solid; border-radius: 10px;">
<h2 class="wp-block-heading" id="h-how-it-works-from-question-to-answer" style="font-size: 30px;"><strong>How It Works: From Question to Answer</strong></h2>



<div class="wp-block-spacer" style="height: 25px;"></div>



<p style="font-size: 16px;">FSQ Spatial Agent is a multi-agent orchestration system that accepts natural language questions and executes complete analytical workflows, from dataset selection through spatial joins to visualization. To understand how it works, let&#8217;s start with a real-world example, then examine the architecture and data foundation that make it possible.</p>



<div class="wp-block-spacer" style="height: 20px;"></div>



<p style="font-size: 16px;">An insurance underwriter asks: &#8220;Which populated areas in California are at highest risk from an insurance standpoint?&#8221;. Four specialized agents collaborate to answer:</p>



<ul class="wp-block-list">
<li style="font-size: 16px;"><strong>The Planner Agent</strong> surveys data across H3 Hub, Overture Maps, Foursquare, and the user&#8217;s workspace, identifying requirements: administrative boundaries, terrain data (NASA SRTM), vegetation classifications (ESA land cover), fire hazard zones (CAL FIRE), building exposure (USA Structures), and population metrics (FEMA National Risk Index). It would equally incorporate uploaded proprietary data—insurance portfolios, claims history, or custom risk models. It selects H3 resolution 8, generating 523,904 cells covering California, and confirms the approach.</li>



<li><strong>The Data Preparation Agent</strong> retrieves California&#8217;s boundary from Overture Maps, then examines H3 Hub schemas to identify relevant columns: elevation for slope calculations, land cover for vegetation types, fire hazard ratings, population exposure fields, and building counts. Simultaneously scanning the user&#8217;s workspace—perhaps insured properties with policy values and construction types—it loads everything as queryable tables, all H3-indexed for seamless joining of authoritative and proprietary data.</li>



<li><strong>The Analyst Agent</strong> constructs SQL queries joining datasets on H3 cell IDs, calculating terrain slope, weighting land cover by flammability, and computing composite risk scores. When user data exists, it blends seamlessly—joining insured properties to risk scores or comparing internal assessments against external data. H3&#8217;s structure eliminates complex spatial operations. Intermediate tables persist for refinement.</li>



<li><strong>The Cartographer Agent</strong> generates visualizations: risk maps highlighting critical Sierra Nevada foothill areas, charts showing 200,000+ at-risk buildings, analysis of 875,857 urban buildings averaging risk score 67, and scatter plots correlating density with risk. User data overlays—like insured properties on risk maps—reveal portfolio-specific exposure.</li>
</ul>



<p style="font-size: 16px;">A central orchestrator coordinates the agents, determining which agent to invoke based on the question and maintaining context across multi-step iterative workflows, seamlessly blending proprietary knowledge with authoritative spatial data.&nbsp;</p>



<div class="wp-block-spacer" style="height: 0px;"></div>



<div class="wp-block-spacer" style="height: 25px;"></div>



<h3 class="wp-block-heading" id="h-powered-by-specialized-tools" style="font-size: 22px;">Powered by Specialized Tools</h3>



<div class="wp-block-spacer" style="height: 15px;"></div>



<figure class="wp-block-image size-full blog-image-radius-none"><img alt="" class="wp-image-56012" height="712" src="https://foursquare.com/wp-content/uploads/sites/2/2026/02/Untitled-design-11.png" width="1024" /></figure>



<p style="font-size: 16px;">Each agent has access to a collection of specialized tools built on a combination of open source and proprietary data and technologies:</p>



<ul class="wp-block-list">
<li style="font-size: 16px;">H3 Hub (built on DataHub): A catalog of 50+ datasets pre-indexed to H3 cells.&nbsp;</li>



<li style="font-size: 16px;">FSQ Places &amp; Categories: Foursquare&#8217;s 100M+ POI dataset.&nbsp;</li>



<li style="font-size: 16px;">Base Map Data: Administrative boundaries and building footprints from Overture Maps Foundation.&nbsp;</li>



<li style="font-size: 16px;">DuckDB: High-performance single node SQL engine.&nbsp;</li>



<li style="font-size: 16px;">Geocoding &amp; Routing: Address resolution and spatial calculations</li>



<li style="font-size: 16px;"><a href="http://kepler.gl">Kepler.gl</a> &amp; Vega : Map &amp; Chart visualization with automatic visual encoding and dynamic tiling.&nbsp;</li>
</ul>



<div class="wp-block-spacer" style="height: 20px;"></div>



<h3 class="wp-block-heading" style="font-size: 22px;">Transparency by Design</h3>



<div class="wp-block-spacer" style="height: 15px;"></div>



<p style="font-size: 16px;">FSQ Spatial Agent exposes its reasoning at every step of the analysis workflow: which datasets were considered and why, what spatial operations were planned, which queries were executed, and how results were interpreted. When the wildfire analysis identifies critical risk in the Sierra Nevada foothills, users can trace this conclusion to specific data joins—CAL FIRE&#8217;s FHSZ classifications combined with terrain slope calculations, land cover showing vegetation density, and building footprint counts. The SQL queries are visible and editable. Intermediate datasets persist in the workspace. Visualizations can be modified. If the analyst disagrees with methodology or wants to refine composite risk score weights, they can see exactly where to intervene. FSQ Spatial Agent augments analysts rather than replacing them, handling mechanical data wrangling while keeping humans in control of analytical decisions.</p>



<div class="wp-block-spacer" style="height: 48px;"></div>



<h4 class="wp-block-heading" id="h-see-it-in-action">See it in action</h4>



<div class="wp-block-spacer" style="height: 20px;"></div>



<figure class="wp-block-video"><video controls="controls" height="2200" src="https://foursquare.com/wp-content/uploads/sites/2/2026/02/ca_risk_map-1.mp4" width="2976"></video></figure>
</div>



<div class="wp-block-spacer" style="height: 35px;"></div>



<div class="wp-block-foursquare-multipurpose-gutenberg-block container" style="background-color: #f9f9f9; padding-top: 40px; padding-right: 40px; padding-bottom: 40px; padding-left: 40px; margin-left: 0px; border-style: solid; border-radius: 10px;">
<h2 class="wp-block-heading" id="h-breaking-the-geospatial-barrier" style="font-size: 30px;">Breaking the Geospatial Barrier</h2>



<div class="wp-block-spacer" style="height: 25px;"></div>



<p style="font-size: 16px;">Spatial analysis has long been specialist knowledge, confined to those who understand coordinate systems, format conversions, and GIS software. FSQ Spatial Agent changes this by making geospatial intelligence accessible to domain experts across industries, bringing the same analytical rigor to retail strategists, insurance underwriters, urban planners, and supply chain managers that was previously available only to professionals with deep expertise with geospatial tools &amp; technologies.</p>



<div class="wp-block-spacer" style="height: 20px;"></div>



<h3 class="wp-block-heading" style="font-size: 22px;">Applications Across Industries</h3>



<div class="wp-block-spacer" style="height: 15px;"></div>



<p style="font-size: 16px;">The accessibility unlocks use cases wherever location matters. The agent translates domain questions into spatial operations across whatever data applies, enabling business experts to ask questions in their own language:&nbsp;</p>



<ul class="wp-block-list">
<li>Retail &amp; Real Estate: Optimal store locations based on commuter patterns and competitor proximity</li>



<li>Insurance: Portfolio exposure to flood zones or wildfire risk compared against industry benchmarks</li>



<li>Urban Planning: Transit gaps relative to job accessibility or infrastructure prioritization for projected growth</li>



<li>Supply Chain &amp; Logistics: Distribution center placement optimized for delivery coverage</li>



<li>Financial Services: Market penetration mapped against demographic opportunity</li>
</ul>



<div class="wp-block-spacer" style="height: 20px;"></div>



<h3 class="wp-block-heading" style="font-size: 22px;">Why Now</h3>



<div class="wp-block-spacer" style="height: 15px;"></div>



<p style="font-size: 16px;">This accessibility became possible only recently, requiring two breakthroughs to converge.</p>



<div class="wp-block-spacer" style="height: 15px;"></div>


<div class="wp-block-image blog-image-radius-none">
<figure class="aligncenter size-large"><img alt="" class="wp-image-55986" height="664" src="https://foursquare.com/wp-content/uploads/sites/2/2026/02/FSQ-Spatial-Agent_Diagram-1.png?w=1024" width="1024" /></figure>
</div>


<div class="wp-block-spacer" style="height: 15px;"></div>



<p style="font-size: 16px;">First, reasoning models advanced beyond summarization to plan multi-step workflows, recover from errors mid-execution, and select datasets based on loosely-phrased questions. Models that are now capable of decomposing complex problems, maintaining context across long workflows, and orchestrating dynamic tool calls didn&#8217;t exist two years ago.</p>



<div class="wp-block-spacer" style="height: 20px;"></div>



<p style="font-size: 16px;">Second, spatial data had to become accessible. Historically fragmented across incompatible formats (satellite imagery in GeoTIFF rasters, administrative boundaries in shapefiles, weather models in NetCDF grids), each required different parsing libraries and custom processing pipelines. Organizations spent 6-12 months building infrastructure before answering their first business question, with no universal join key to combine datasets. FSQ Spatial H3 Hub solved this by transforming diverse spatial datasets into tabular format indexed to H3 hexagonal grid, making census demographics, climate projections, infrastructure networks, and land cover classifications all queryable through SQL and joinable on a common H3 cell ID.</p>



<div class="wp-block-spacer" style="height: 20px;"></div>



<p style="font-size: 16px;">This convergence differentiates FSQ Spatial Agent from general-purpose AI assistants: reasoning models provide the intelligence, H3 Hub provides the data in a form that intelligence can actually use.</p>



<div class="wp-block-spacer" style="height: 20px;"></div>
</div>



<div class="wp-block-spacer" style="height: 35px;"></div>



<div class="wp-block-foursquare-multipurpose-gutenberg-block container" style="background-color: #c4edff; padding-top: 40px; padding-right: 40px; padding-bottom: 40px; padding-left: 40px; margin-left: 0px; border-style: solid; border-radius: 10px;">
<h2 class="wp-block-heading" id="h-available-now-in-fsq-spatial-desktop-coming-soon-to-workbench" style="font-size: 30px;">Available Now in FSQ Spatial Desktop, Coming Soon to Workbench</h2>



<div class="wp-block-spacer" style="height: 20px;"></div>



<p style="font-size: 16px;">FSQ Spatial Agent launches today in <a href="https://foursquare.com/products/spatial-desktop/" rel="noreferrer noopener" target="_blank"><span class="fsq-external-link">FSQ Spatial Desktop</span></a>, with availability in FSQ Spatial Workbench coming soon. Desktop users will find the Agent panel in their workspace, ready to accept natural language questions against loaded datasets and the full H3 Hub catalog.</p>



<div class="wp-block-spacer" style="height: 20px;"></div>



<p style="font-size: 16px;"><a href="https://foursquare.com/products/spatial-desktop/" rel="noreferrer noopener" target="_blank"><span class="fsq-external-link">FSQ Spatial Desktop</span></a> is free to download and use, giving you access to the full suite of spatial visualization and analysis tools. AI Agent capabilities are available through two subscription tiers: $25/month for individual practitioners and $100/month for power users requiring higher query volumes and priority processing. Download FSQ Spatial Desktop to explore your data with the core tools, then upgrade when you&#8217;re ready to let the agent handle the heavy lifting.</p>



<div class="wp-block-spacer" style="height: 35px;"></div>
</div>



<p></p>
<p>The post <a href="https://foursquare.com/resources/blog/ad-tech/introducing-fsq-spatial-agent-your-geospatial-ai-analyst/">Introducing FSQ Spatial Agent: Your Geospatial AI Analyst</a> appeared first on <a href="https://foursquare.com">Foursquare</a>.</p>
