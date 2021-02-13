# Google Dorking

**Date:** 15, January, 2021

**Author:** Dhilip Sanjay S

---
[Click Here](https://tryhackme.com/room/googledorking) to go to the TryHackMe room.

## Crawlers
- Crawlers discover content through various means.
    - Pure Discovery - URL visited by the crawler and information regarding the content type fo the website is returned to the **Search Engine**.
    - Following URLs found from previously crawled sites.

---
### Name the key term of what a "Crawler" is used to do
- **Answer:** index

### What is the name of the technique that "Search Engines" use to retrieve this information about websites?
- **Answer:** Crawling

### What is an example of the type of contents that could be gathered from a website?
- **Answer:** Keywords

---


## Search Engine Optimisation
- SEO ranking - Search Engines will priortise those domains that are easier to index.
- Many factors:
    - How responsive your website is to the different browser types.
    - How easy it is to crawl your website.
    - What kind of keywords your websit has.
- [Google Site Analyser](https://web.dev/)

## Robots.txt
- This text file defines the permissions the Crawler has to the website.
- It can specify what files and directories that we do or don't want to be indexed by the Crawler (like admin panel).

|Keyword    | Function        |
|-----------|-----------------|
|User-agent |	Specify the type of "Crawler" that can index your site (the asterisk being a wildcard, allowing all "User-agents"|
|Allow      |	Specify the directories or file(s) that the "Crawler" can index|
|Disallow   |	Specify the directories or file(s) that the "Crawler" cannot index|
|Sitemap    |	Provide a reference to where the sitemap is located (improves SEO as previously discussed, we'll come to sitemaps in the next task)|

- You can use **Regex** to allow/disallow contents to be indexed by Crawlers.
- Generally, Sitemap is located at `/sitmeap.xml`.
---
### Where would "robots.txt" be located on the domain "ablog.com"?
- **Answer:** ablog.com/robots.txt

### If a website was to have a sitemap, where would that be located?
- **Answer:** /sitemap.xml

### How would we only allow "Bingbot" to index the website?
- **Answer:** User-Agent: Bingbot
- Other bots like Googlebot, msnbot won't be allowed to index the site.

### How would we prevent a "Crawler" from indexing the directory "/dont-index-me/"?
- **Answer:** Disallow: /dont-index-me/

### What is the extension of a Unix/Linux system configuration file that we might want to hide from "Crawlers"?
- **Answer:** .conf

---

## Sitemaps
- Sitemaps are indicative resources that are helpful for crawlers - they specify the necessary routes to find content on the domain.
- It is in **XML** file format.
- They show the route to the nested content.
- Sitemaps are favourable for search engines - because all the necessary routes to content are already provided in this file.

### What is the typical file structure of a "Sitemap"?
- **Answer:** XML

### What real life example can "Sitemaps" be compared to?
- **Answer:** Map

### Name the keyword for the path taken for content on a website
- **Answer:** route

---

## Google Dorking
- Using google for advanced searching.
- Refer [Google Hacking on Wikipedia](https://en.wikipedia.org/wiki/Google_hacking)

---
### What would be the format used to query the site bbc.co.uk about flood defences?
- **Answer:** site:bbc.co.uk flood defences

### What term would you use to search by file type?
- **Answer:** filetype:

### What term can we use to look for login pages?
- **Answer:** intitle:login

---