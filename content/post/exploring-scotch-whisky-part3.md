---
title: "Exploring Scotch Whisky, Part 3: Mapping Distilleries & Regions"
date: 2021-06-18
draft: false
featuredImg: ""
tags: 
  - analysis
  - r
---

{{< figure src="/images/exploring-scotch-whisky-part3/isle-of-skye.jpg" alt="On the Isle of Skye during our honeymoon in Scotland." class="normal" caption="On the Isle of Skye during our honeymoon in Scotland." >}}

# Introduction
Welcome to the next installment of **Exploring Whisky: The Journey to My Next Bottle of Scotch**, a multi-part project in which I leverage data to share fun facts about whisky as I continue my hopeful search to discover delicious scotch. 

Previously in [Part 1]({{< ref "/post/exploring-scotch-whisky-part1" >}} "Exploring Scotch Whisky, Part 1"), we built our dataset from scratch by scraping scotch whisky reviews, prices, brands, regional locations, and more from the internet. Then in [Part 2]({{< ref "/post/exploring-scotch-whisky-part2" >}} "Exploring Scotch Whisky, Part 2"), we explored the distributions of and relationships between variables including whisky `type`, `age`, `ABV`, `price`, and rating `points`.

Next, Part 3 takes a closer look at whisky brands and where distilleries are located across the six different scotch regions of Scotland.

## Project Goals
As a reminder, the overarching goals of this mutli-part project are threefold:

* To augment my learning about whisky brands, regions, flavors, and options
* To understand the breadth, depth, and quirks of the scotch whisky industry
* To leverage data to determine which whiskies I ought to try myself

# Part 3: Mapping Distilleries & Regions
Much of a scotch's uniqueness depends heavily on the region from where it hails in Scotland. In Part 3, we take the first step towards understanding the regionality of whisky by exploring differences across brands (`distillery`) and `region`. Plus, we can make use of `lat` and `long` coordinates to create an interactive map of distilleries across Scotland to observe any geographical clustering.

## Guiding Questions
* Which regions release the most whiskies?
* Which regions produce the most highly-rated whiskies?
* Which brands release the most expensive and/or highly-rated whiskies?
* Which brands are the most prolific?
* How clustered or distributed are distilleries across Scotland?

## Regions
{{< newtabref href="https://www.whiskyadvocate.com/scotch-regions-list/" title="Scotch whisky regions are not exactly standardized" >}} or commonly recognized across the industry, but many consider there to be six different regions: **Speyside**, **Highland**, **Lowland**, **Islay**, **Islands**, and **Campbeltown**. Each `region` boasts a distinct whisky style and sensibility. In future installments of this project, we'll explore how flavor profiles vary between regions and distilleries within them. For now, let's check out a regional breakdown of the whisky releases in our data.

{{< figure src="/images/exploring-scotch-whisky-part3/region_treemap.png" alt="Treemap of regional distilleries in Scotland." class="big" width="75%" >}}

Where regional information was available, a significant proportion of the whiskies included originate from the Speyside and Highland regions, followed by the Islay region to a lesser extent. This treemap plot (made using the `treemapify`[^1] package) highlights just how prolific brands such as Highland Park, Bruichladdich, and Macallan are, each rivaling the number of releases coming from the entire Lowland or Campbeltown regions alone.

Volume is one thing, but what about quality? The violin plot below illustrates the distribution of whisky release rating `points` segmented by region and sorted by each region's highest-rated scotch. For reference, Whisky Advocate uses a tiered, 100-point rating scale as described below: 

* 95-100 points — **Classic**: *a great whisky*
* 90-94 points — **Outstanding**: *a whisky of superior character and style*
* 85-89 points — **Very Good**: *a whisky with special qualities*
* 80-84 points — **Good**: *a solid, well-made whisky*
* 75-79 points - **Mediocre**: *a drinkable whisky that may have minor flaws*
* 50-74 points - **Not Recommended**

{{< figure src="/images/exploring-scotch-whisky-part3/region_violin.png" alt="Distribution of whisky rating points by region." class="big" width="75%" >}}

In general, Islay whiskies appear to slightly edge out Speyside and Highland whiskies when it comes to quality. Anecdotally, I've always felt that whiskies from the Islay region, which are known for their peatiness, carry a favorable reputation amongst scotch connoisseurs. 

{{< newtabref href="https://en.wikipedia.org/wiki/Peat#Scotland" title="Peated whisky" >}} is created by harvesting local earthy moss and burning it to dry the malted barley before the distillation process begins, which infuses the whisky with a distinct smoky flavor. The peated earths nearby each distillery have their own quirks and qualities, thereby resulting in unique smoky flavor notes across different scotch whiskies. I myself typically favor sherried, fruity Speyside whiskies over the smoky notes of an Islay, but I'd love to develop more of a taste for peaty whiskies, and perhaps that's where this project will lead.

Rating points across Lowland whiskies vary fairly little, but the region certainly appears a tier below its peers in quality. My sense (from the internet) is that Lowland whiskies are more delicate with less audacious flavors than other regional varieties, and while their mildness works well for scotch beginners, it is perhaps slightly too ho-hum for experts. In contrast, Speyside, Highland, and Island whiskies vary widely in quality. Recalling some of the lowest scoring whiskies from [Part 2]({{< ref "/post/exploring-scotch-whisky-part2" >}} "Exploring Scotch Whisky, Part 2") helps to explain the long negative tails of these regional distributions. 

## Distilleries & Brands
So far, we've learned that Highland, Speyside, and Islay regions are generally home to the most prolific brands and the highest-rated scotch, with a few exceptions. But affordability matters too, and I'd like to know which brands are the best bang for my buck. 

Let's roll `price`, `points`, `region`, `distillery`, and releases into a single bubble plot by comparing each brand's median release price to its average rating across those releases. Because we know from [Part 2]({{< ref "/post/exploring-scotch-whisky-part2" >}} "Exploring Scotch Whisky, Part 2") that `price` is heavily skewed due to exorbitantly lavish price points, we can use the median here as a more stable and skew-resistant statistic compared to the mean. 

I also noticed that Ladyburn's lone release in the data was priced at a hefty $1,750 (at least 3x larger than the next higher median price). With that in mind, I omitted Ladyburn as an outlier here to create a more legible plot. ***Click on the image below to explore the full interactive plot on Tableau.***

{{< figure src="/images/exploring-scotch-whisky-part3/distillery-scatterplot.png" alt="Bubbleplot of median price by average rating points." class="big" width="75%" caption="Click to explore the interactive dashboard" link="https://public.tableau.com/views/MappingScotchDistilleriesRegions/ScotchPricevs_RatingPoints_1?:language=en-US&:display_count=n&:origin=viz_share_link" target="_blank" >}}

Observing distilleries across all regions together, most brands cluster together between a median price of \$50-200 and an average rating points of 84-90, indicating again that there are plenty of affordable "Very Good" scotches out there to explore. Hooray! Some brands notably outside of the cluster that struck my interest include: 

* **Knockando**, a Speyside brand with only 2 releases, but a dreadfully low average rating
* **Balvenie**, a highly prolific Speyside distillery with expensive, well-rated scotch
* **Glenfarclas**, a super pricey Speyside brand with over 30 releases and a median price over \$500
* **Brora**, a well-regarded premium Highland brand with a stunning average rating of 92.7 points across 15 releases
* **Ailsa Bay**, a fascinating new Lowland distillery with only one experimental, cheap, and extremely well-rated scotch

Further can be gleaned from isolating particular regions across Scotland. The plot is unsurprisingly dominated by entries from the Speyside and Highland regions, from where my [benchmark scotch]({{< ref "/post/exploring-scotch-whisky-part2" >}} "Exploring Scotch Whisky, Part 2") brands Glenfiddich and Glengoyne originate respectively. Filtering out other brands makes either region's most prolific, reasonably-priced brands (Glenlivet, Macallan, Glenmorangie) easier to locate. Generally, Speyside distilleries seem to skew a bit more expensive than Highland brands.

The remaining four regions (Islay, Island, Lowland, and Campbeltown) only have a few entries each. Islay is a remarkably consistent region with 8 distilleries included, each of which have at least 30 releases, median prices between \$90-$130, and average ratings between 86-91 points. There may not be many square acres of land in the Islay region, but every distillery is fairly prolific and very well-regarded, including Lagavulin (the scotch famously enjoyed by Parks & Recreation's {{< newtabref href="https://www.youtube.com/watch?v=frbsZ8TGsX8" title="Ron Swanson" >}} and real life's {{< newtabref href="https://www.youtube.com/watch?v=jNOye7_9qag" title="Nick Offerman" >}}.)

{{< figure src="/images/exploring-scotch-whisky-part3/ron-swanson-scotch.gif" alt="Gif of Ron Swanson from Parks & Recreation bobbing his head with a glass of scotch." class="right" >}}

The most prolific brands in the Lowlands region such as Auchentoshan and Bladnoch are tightly clustered around 85 average points and a \$100 median price, while other boutique brands such as Daftmill, Ailsa Bay, and Leven have 1-2 releases each at the extremes of price and rating. Meanwhile, in the Islands region, we can easily find the extremely prolific Highland Park (located way up on the Orkney Islands and by far the most northern distillery), but Abhainn Dearg also stands out from the pack with one whisky release that is far more expensive (\$239) than its rating of 81 would suggest. 

Lastly, the once bustling scotch region of Campbeltown is now typically forgotten. Led by Springbank, the Campbeltown region only has three remaining brands to carry forward its history after several misguided distillers {{< newtabref href="https://www.thewhiskyexchange.com/c/316/campbeltown-single-malt-scotch-whisky" title="reportedly opted to cut corners" >}} and peddle mass-produced, subpar scotch for a few decades, leading to the majority of distilleries getting shuttered nearly a century ago. 

It's also worth acknowledging that several notable brands in these data aren't associated with a specific region on {{< newtabref href="https://www.whisky.com/whisky-database/database.html" title="whisky.com" >}}. Some are {{< newtabref href="https://en.wikipedia.org/wiki/Independent_bottler" title="independent bottlers" >}} (Adelphi, Ballantine's, Gordon & MacPhail) that create partnerships with distilleries and release niche versions of their scotch under different branding. Others are well-known blended whisky brands that don't have a brick and mortar distillery of their own, but rather they share facilities with single malt distillers (Johnnie Walker & Cardhu, Dewar's & Aberfeldy, Chivas Regal & Strathisla, and the list goes on). While these brands are very lucrative businesses that might invite consumers to their swanky liquor shops or experiential visitor centers, they don't inherently carry the same authentic regionality as single malt scotches, regardless of where they physically produce their whisky.

## Interactive Map
We have a sense of which regions collectively produce the most whisky (Speyside, Highland) and which produce the best whisky according to Whisky Advocate (Islay, followed by Speyside and Highland). However, it's difficult to fully grasp these findings without layering distillery locations over a map of Scotland to better grasp the geographical spread within and across regions. It's time to make use of our `lat` and `long` coordinate variables, which were scraped from whisky.com in {{< newtabref href="https://github.com/bengreenwald/whisky/blob/main/code/scrape-scotch-distilleries.R" title="Part 1" >}}. I believe that maps are best explored interactively, so ***feel free to click on the image below to explore the full interactive plot on Tableau.***

{{< figure src="/images/exploring-scotch-whisky-part3/distillery-map.png" alt="Map of distilleries by region." class="big" width="75%" caption="Click to explore the interactive dashboard" link="https://public.tableau.com/views/MappingScotchDistilleriesRegions/DistilleryMap_1?:language=en-US&:display_count=n&:origin=viz_share_link" target="_blank" >}}

The first thing that jumps out from the map is how self-contained and densely populated the Speyside region is. No region produces more scotch, and yet Speyside is described as smaller than the size of Rhode Island. According to {{< newtabref href="https://scotchwhisky.com/magazine/around-the-world/whisky-travel/10840/dufftown/" title="scotchwhisky.com back in 2016" >}}, the charming parish of Dufftown alone produces over 40 million liters of spirit annually across its six distilleries. Because Speyside is so compact and home to many legendary distilleries such as Glenlivet, Glenfiddich, Macallan, Balvenie, and Glenfarclas, it has become a favorite for visitors who aspire to bounce between several distilleries in a single day. My wife and I made our own day-long pilgrimage from Inverness to the Speyside region to visit Glenfiddich and Aberlour during our honeymoon. My only regret was not spending more time in Speyside; it's beautiful, and the spirit flows endlessly. 

{{< figure src="/images/exploring-scotch-whisky-part3/river-spey.jpg" alt="Walking along the River Spey, our bellies full of scotch." class="left" caption="Walking along the River Spey, our bellies full of scotch." >}}

But the Speyside region is not isolated; in fact, it sits within the expansive Highland region, which in the world of scotch encompasses pretty much everything just north of Edinburgh and Glasgow on Scotland's primary land mass. Compared to Speyside, the spread here is much greater. Glengoyne, which sits directly upon the {{< newtabref href="https://en.wikipedia.org/wiki/Highland_Boundary_Fault" title="Highland Line" >}} that separates the Highland and Lowland regions in Scotland, is over 250 miles away from Wolfburn and Pulteney distilleries, yet all three are considered technically part of the Highlands. 

Meanwhile, the Lowlands comprises a much smaller area just south of Scotland's largest metro areas. Most of its distilleries hover around Edinburgh or Glasgow, but one exception is Bladnoch, which is still in Scotland but further south than Newcastle in England. 

One interesting trend revealed by the map is that many distilleries seem to be located nearby the coastline. Perhaps that's a hint towards why islands tend to be such popular spots for distilling scotch, such as in the Islay, Campbeltown, and Islands regions. Whereas Islay and Campbeltown are self-contained islands that contain all of their regions' whisky, the Islands region is more of a catch-all expanse encompassing several islands all over Scotland including Skye, Arran, Jura, Orkney, and more. 

True to their coastal locales, scotch from these island regions tend to celebrate maritime flavors, with many whiskies tasting salty, briny, or even with notes of seaweed. Turns out that these flavors are often by design; many distillers deliberately expose their barrels to salty sea air for many years, which imbues the aging scotch inside with fascinating characteristics of the sea. This map also exemplifies how impressively the Islay region punches above its tiny size; it's incredible how so much of the world's highest rated whisky originates from a place smaller than New York City in land area.

# Key Takeaways
Some of the major takeaways from this analysis:

* The Speyside region is just the size of Rhode Island, yet it produces more scotch than any other region in Scotland. Meanwhile, the Highland region is by far the largest in land area, and together, **the Speyside and Highland regions account for nearly two thirds of whisky releases in our data** (*when regional data was available*).

* **The tiny Islay region produces the highest-rated scotch**. Each of its eight distilleries are prolific and well-regarded, and their whiskies are famously known for their distinct characteristics of smoke and peat.

* In these data, **Highland Park and Bruichladdich each produce an amount of whisky releases on par with all distilleries combined across either of the entire Lowlands or Campbeltown regions**, reinforcing that islands and coastlines are particular hotspots for scotch production.

* **Glenfarclas, Brora, and Balvenie are the most prolific, yet expensive distilleries** with median release prices soaring between \$360-\$540. However, **only Brora seems possibly worth the extra cash**; it easily boasts the highest average rating for a distillery, while Glenfarclas and Balvenie are similarly rated to their peers that are priced 3-4x cheaper.

* **The vast majority of brands cluster together between a median price of \$50-200 and an average rating of 84-90 points**, suggesting once again that there are plenty of affordable, "Very Good" scotches out there to discover. I'm excited to try them! 

{{< newtabref href="https://github.com/bengreenwald/whisky/blob/main/analysis/scotch-analysis-part3.R" title="Code for Part 3" >}}

# Next Steps
We've dipped our toes into the regionality of scotch, and location seems to play a definitive role in a whisky's identity and flavor profile. 

Next, Part 4 will dive into the text of Whisky Advocate review descriptions and qualitative feedback to determine whether we can parse out common flavors and themes across scotch releases that help us better understand the complexity and taste of whisky.

[^1]: treemapify package: {{< newtabref href="https://wilkox.org/treemapify/index.html" title="https://wilkox.org/treemapify/index.html" >}}