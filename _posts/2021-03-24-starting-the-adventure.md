---
layout: post
read_time: true
show_date: true
title: "Web Scraping"
date: 2021-03-24
img: posts/20210324/starting_adventure.jpg
tags: [general blogging, scrap, web]
author: Alp Aygün
description: "Web scraping is the process of using bots to extract content and data from a website."
---

## What is web scraping?

**Web scraping is the process of using bots to extract content and data from a website.**
Unlike screen scraping, which only copies pixels displayed onscreen, web scraping extracts underlying HTML code and, with it, data stored in a database. The scraper can then replicate entire website content elsewhere.

Web scraping is used in a variety of digital businesses that rely on data harvesting. Legitimate use cases include:

- Search engine bots crawling a site, analyzing its content and then ranking it.
- Price comparison sites deploying bots to auto-fetch prices and product descriptions for allied seller websites.
- Market research companies using scrapers to pull data from forums and social media (e.g., for sentiment analysis).

## Web Scraping using axios and Cheerio

There are two really great tools to use when scraping websites with NodeJs: Axios and Cheerio. Using these two tools together, we can grab the HTML of a web page, load it into Cheerio (more on this later), and query the elements for the information we need.

![1_jPZdF7OOID3jLYPbMT9fCg.png](https://res.cloudinary.com/dbh0tmmir/image/upload/v1660651864/1_j_P_Zd_F7_OOID_3j_LY_Pb_MT_9f_Cg_c97a536f8e.png)

### Axios

Axios is a promise based HTTP client for both the browser, and for NodeJS. This is a well known package that is used in tons and tons of projects. Most of the React and Ember projects I work on use Axios to make API calls.

We can use axios to get the HTML of a website:

```

  import axios from 'axios';

  await axios.get('https://www.realtor.com/news/real-estate-news/');

```

### Cheerio

Cheerio is the most amazing package I never heard of until now. Essentially, Cheerio gives you jQuery-like queries on the DOM structure of the HTML you load! Its amazing and allows you to do things like this:

```
  const cheerio = require('cheerio')
  const $ = cheerio.load('<h2 class="title">Hello world</h2>')
  const titleText = $('h2.title').text();

```

With Axios and Cheerio, making our NodeJS scraper is dead simple. We call a URL with axios, and load the output HTML into cheerio. Once our HTML is loaded into cheerio, we can query the DOM for whatever information we want!

```
import axios from 'axios';
import cheerio from 'cheerio';

export async function scrapeRealtor() {
  const html = await axios.get('https://www.realtor.com/news/real-estate-news/');
  const $ = await cheerio.load(html.data);
  let data = [];

  $('.site-main article').each((i, elem) => {
    if (i <= 3) {
      data.push({
        image: $(elem).find('img.wp-post-image').attr('src'),
        title: $(elem).find('h2.entry-title').text(),
        excerpt: $(elem).find('p.hide_xxs').text().trim(),
        link: $(elem).find('h2.entry-title a').attr('href')
      })
    }
  });

  console.log(data);
}
```

**The output**

> [ { image:

     'https://rdcnewsadvice.wpengine.com/wp-content/uploads/2019/08/iStock-172488314-832x468.jpg',
    title:
     'https://rdcnewsadvice.wpengine.com/wp-content/uploads/2019/08/iStock-172488314-832x468.jpg',
    title:
     'One-Third of Mortgage Borrowers Are Missing This Opportunity to Save $2,000',
    excerpt:
     'Consumer advocates have an important recommendation for first-time buyers to take advantage of an opportunity to save on housing costs.',
    link:
     'https://www.realtor.com/news/real-estate-news/one-third-of-mortgage-borrowers-are-missing-this-opportunity-to-save-2000/' },

{ image:
'https://rdcnewsadvice.wpengine.com/wp-content/uploads/2019/08/iStock-165493611-832x468.jpg',
title:
'Trump Administration Reducing the Size of Loans People Can Get Through FHA Cash-Out Refinancing',
excerpt:
'Cash-out refinances have grown in popularity in recent years in tandem with ballooning home values across much of the country.',
link:
'https://www.realtor.com/news/real-estate-news/trump-administration-reducing-the-size-of-loans-people-can-get-through-fha-cash-out-refinancing/' },
{ image:
'https://rdcnewsadvice.wpengine.com/wp-content/uploads/2019/08/GettyImages-450777069-832x468.jpg',
title: 'Mortgage Rates Steady as Fed Weighs Further Cuts',
excerpt:
'Mortgage rates stayed steady a day after the Federal Reserve made its first interest-rate reduction in a decade, and as it considers more.',
link:
'https://www.realtor.com/news/real-estate-news/mortgage-rates-steady-as-fed-weighs-further-cuts/' },
{ image:
'https://rdcnewsadvice.wpengine.com/wp-content/uploads/2019/07/GettyImages-474822391-832x468.jpg',
title: 'Mortgage Rates Were Falling Before Fed Signaled Rate Cut',
excerpt:
'The Federal Reserve is prepared to cut interest rates this week for the first time since 2008, but the biggest source of debt for U.S. consumers—mortgages—has been getting cheaper since late last year.',
link:
'https://www.realtor.com/news/real-estate-news/mortgage-rates-were-falling-before-fed-signaled-rate-cut/' } ]

So we can scrape the data on the sites. Here are some resource suggestions to know more about this topic;

- [https://axios-http.com/docs/intro](https://axios-http.com/docs/intro)
- [https://cheerio.js.org/](https://cheerio.js.org/)
- [https://www.freecodecamp.org/news/how-to-scrape-websites-with-node-js-and-cheerio/](https://www.freecodecamp.org/news/how-to-scrape-websites-with-node-js-and-cheerio/)
- [https://dev.to/drsimplegraffiti/i-scraped-dev-to-using-axios-and-cheerio-26ko](https://dev.to/drsimplegraffiti/i-scraped-dev-to-using-axios-and-cheerio-26ko)

## References

1.  xxx, AAA, 2020, http://www.galaksiya.com
2.  xxx, AAA, 2021, http://www.galaksiya.com
