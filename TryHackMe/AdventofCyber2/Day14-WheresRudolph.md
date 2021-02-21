# Day 14 - Where's Rudolph?

**Date:** 14, December, 2020

**Author:** Dhilip Sanjay S

---

## OSINT
- OSINT investigations mostly start with only a `username`.
- User's post history in social media + Google - used to get more information.

### User account Search
- [Namechk](https://namechk.com/)
- [Whatsmyname](https://whatsmyname.app/)
- [Namecheckup](https://namecheckup.com/)
- [WhatsMyName github](https://github.com/WebBreacher/WhatsMyName)
- [Sherlock Project](https://github.com/sherlock-project/sherlock)

### Reverse Image Lookup
- [Yandex Image Search](https://yandex.com/images/)
- [TinEye](https://tineye.com/)
- [Bing Visual Search](https://www.bing.com/visualsearch?FORM=ILPVIS)
- And yeah [Google](https://www.google.co.in/imghp?hl=en&tab=wi&ogbl)!

### Image Metadata
- [Jeffrey's Image Metadata Viewer](http://exif.regex.info/exif.cgi)
    - **Tip:** If you find any website having images with metadata, you can report in bug bounty platforms (Information Disclosure).

### Breached Data
- [Have I been Pwned](https://haveibeenpwned.com/)
- [Syclla.sh](https://scylla.sh/)
- [Dehashed](https://dehashed.com/)

---

## Solutions

### 1) What URL will take me directly to Rudolph's Reddit comment history?
- **Answer:** https://www.reddit.com/user/IGuidetheClaus2020/comments

---

### 2) According to Rudolph, where was he born?
- **Answer:** Chicago

---

### 3) Rudolph mentions Robert.  Can you use Google to tell me Robert's last name?
- **Answer:** May

---

### 4) On what other social media platform might Rudolph have an account?
- **Answer:** Twitter
- **Steps to Reproduce:** 
    - He mentions about `Twitter` in one of his Reddit Comments.
    - Search for `IGuideClaus2020` in [NameCheckup](https://namecheckup.com/) (`the` is removed from username in this case).
    - [I Guide the Claus 2020](https://twitter.com/IGuideClaus2020) Twitter Profile

---

### 5) What is Rudolph's username on that platform?
- **Answer:** IGuideClaus2020

---

### 6) What appears to be Rudolph's favorite TV show right now?
- **Answer:** Bachelorette
- **Steps to Reproduce:** Check twitter posts.

---

### 7) Based on Rudolph's post history, he took part in a parade.  Where did the parade take place?
- **Answer:** Chicago
- **Steps to Reproduce:** Use Google Reverse image search

---

### 8) Okay, you found the city, but where specifically was one of the photos taken?
- **Answer:** 41.891815, -87.624277 
- **Steps to Reproduce:**
    - Visit [Jeffrey's Image Metadata Viewer](http://exif.regex.info/exif.cgi) and upload the Higher Resolution image from the Twitter post.
---

### 9) Did you find a flag too?
- **Answer:** {FLAG}ALWAYSCHECKTHEEXIFD4T4

---

### 10) Has Rudolph been pwned? What password of his appeared in a breach?
- **Answer:** spygame
- **Steps to Reproduce:** 
    - Search for the Email ID mentioned in the twitter profile in [Syclla.sh](https://scylla.sh/)
    - Search Query : `email:rudolphthered@hotmail.com`

---

### Based on all the information gathered.  It's likely that Rudolph is in the Windy City and is staying in a hotel on Magnificent Mile.  What are the street numbers of the hotel address?
- **Answer:** 540
- **Steps to Reproduce:** 
    - Go to the specified co-ordinates in Google Maps. Click on `Chicago Marriott Downtown`.

---