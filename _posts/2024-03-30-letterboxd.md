---
layout: post
title:  "Enhancing your Letterboxd experience"
date: 2024-03-30
description: Letterboxd already is the favorite platform for movie lovers, what if we could make the experience even better and free?  
---
<p class="intro"><span class="dropcap">T</span>his post will describe how to create a blog post with the proper naming conventions, as well as tips for including images and links.  At the end there is a troubleshooting guide that can help with the most common problems .</p>

### Before, what is Letterboxd?

Letterboxd has emerged as a cultural phenomenon, captivating both avid moviegoers and casual viewers with its revolutionary platform. Its blend of practicality and charming format has reshaped the movie-watching experience. Nowadays, it's common to observe movie enthusiasts instinctively reaching for their phones post-screening to share their ratings and opinions. This behavior has not only garnered widespread attention but has also fueled the app's word-of-mouth promotion.

Beyond its functionality in tracking watched movies, ratings, and comments, Letterboxd serves as a vibrant social hub. Here, users can engage with friends and prominent content creators, gaining insights into their viewing habits and perspectives on films. In my view, Letterboxd stands as an indispensable tool for anyone passionate about cinema.


### Intro

As you might have guessed, I'm an avid fan of Letterboxd. And as a Data Science student, my love for statistics is undeniable. Surely, you've experienced the joy of receiving your personalized Spotify Wrapped at the year's end. Well, Letterboxd offers a similar treat. Every first week of the year, they unveil your personalized page brimming with stats from the past year. But wouldn't it be thrilling to track your movie-watching stats throughout the year? Absolutely! Regrettably, this feature is exclusive to paid subscribers.

However, fear not! Armed with Python skills and the art of web scraping, there's a workaround. While I won't divulge my own findings here, I'm more than willing to impart the knowledge of how to delve into your movie data. Let's embark on this journey together, unlocking the potential of your cinematic adventures.


### Step 1: Knowing what you want 
Letterboxd keeps your data in 3 main places: Films, Diary, Reviews
All 3 will lead to different paths depending in what data you want to collect for me I wanted to answer 3 questions: What decade has the highest rating? What decade has the lowest rating? and What day of the week I watch most movies?

Based off these 3 questions "Films" will attend best my necesseties since is where all the movies I've seen are storaged and I can answer these 3 questions.

*For this theme, the layout should stay as `post`.   All the other fields should be updated with the information for your particular blog post.*

3. If you add an image to the header information, the top banner picture will change for this post.  You can also add an optional field `display_image: true` if you want to display a larger image just below the header image.  If this happens, the banner image will return to the default.
  * The blog image should be a `.jpg` or `.png` file that can be found in `assets/img`.  Don't make it too large or the page will take longer to load (500-800 KB is a good size).  Leave the file path as `/assets/ing/` in the header area.* 
  * Adding a post image will also display the image in the "next" or "previous" post location.

4. Write the body of the blog using markdown.  There are a lot of references for markdown available.  I like the [Markdown Guide](https://www.markdownguide.org) because many of the examples show both the markdown and the html code.  There are separate pages for [basic syntax](https://www.markdownguide.org/basic-syntax/), [extended syntax](https://www.markdownguide.org/extended-syntax/), and a [cheatsheet](https://www.markdownguide.org/cheat-sheet/) for quick reference. 

---
---

### Links 

To create a link (internal or external), enclose the link text in brackets (e.g., [Statistics Department]) and then follow it immediately with the URL in parentheses (e.g., (https://statistics.byu.edu)).

For example:
```
{% raw %}My favorite department at BYU is the [Statistics Department](https://statistics.byu.edu).{% endraw %}
```
My favorite department at BYU is the [Statistics Department](https://statistics.byu.edu)


If you want external links to open in a separate window, you will need to use html code with `target="_blank"` inside the `a` tag. 

For example:
```
My favorite department at BYU is the <a href="https:statistics.byu.edu" target="_blank">Statistics Department</a>
```
My favorite department at BYU is the <a href="https:statistics.byu.edu" target="_blank">Statistics Department</a>


----
----

### Internal Links and Files

If you want to have a link that points to another location on your site or if you want to include a file (such as an image or video) you must use the `site.url` and `site.baseurl` variables when making the link reference.  For example, this link to pointing to the [About]({{site.url}}/{{site.baseurl}}/about) page is coded as:
```
[About]({% raw %}{{site.url}}/{{site.baseurl}}/about){% endraw %}
```
Paths to files should also be referenced with the `site.url` and `site.baseurl` variables (see the section on **Adding Images**).

---
---

### Adding Images
*In the examples below, if your image ends with `.png` or `.JPEG`, use the appropriate extension instead of `.jpg`.*  

Images for the blog will generally but put into the `assets/img` folder.  (You can also create a subfolder for images, but you will need to include the subfolder name in the reference link.) 

Markdown syntax for including images is `![Fig Name](path/to/image)`.  For example:
```
{% raw %}![Figure]({{site.url}}/{{site.baseurl}}/assets/img/image_name.jpg){% endraw %}
```
![Figure]({{site.url}}/{{site.baseurl}}/assets/img/image5.jpg)

---
---

### Resizing images

The image I added in the previous section seems a bit large for this post.  Unfortunately,
there isn't a good way to resize images with markdown, so if you need to resize an image, use html instead of markdown and specify the width in the style parameter as follows:

```
{% raw %}<img src="{{site.url}}/{{site.baseurl}}/assets/img/image_name.jpg" alt="" style="width:300px;"/>{% endraw %}
```

(Example with width set to 300 pixels)
<img src="{{site.url}}/{{site.baseurl}}/assets/img/image5.jpg" alt="" style="width:300px;"/>


(Example with width set to 100 pixels)
<img src="{{site.url}}/{{site.baseurl}}/assets/img/image5.jpg" alt="" style="width:100px;"/>




---

### Troubleshooting

Here are some things to keep in mind if your blog appearance isn't going as you planned:

**Problem:  The blog post that I created isn't appearing**

Possible Solutions: 
  - Check your date. GitHub pages won't display blog posts with future dates
  - Check the name of the post file.  It must be in the form "YYYY-MM-DD-title.md".  Make sure that you included the `.md` extension 
  - Check the yaml header.  If there are any special characters in any of the fields, you need to use quotes around the entire field entry.  The most common culprit is the description.  If you're having trouble, try putting quotes around the entire description

---

**Problem:  I know that I made changes to a blog post but the changes aren't appearing**

Possible Solution:
  - Check the header.  If there are any special characters in any of the fields, you need to use quotes around the entire field entry.  The most common culprit is the description.  If you're having trouble, try putting quotes around the entire description.

---

**Problem:  My entire blog has weird formatting**

Possible Solution:
  - Usually this is an address problem.  Double check your url and baseurl in the `_config` file
