---
title: "How to Start a Blog, For Under 2$ A Month"
date: 2020-02-25T20:47:53-05:00
slug: how-to-start-a-blog
---
So this is my blog. I wanted to create one, but I'm also a bit of a nerd, so I decided to do this in the most effecient way I could, which as is often the case, involves messing around with technologies that I'm familiar enough with, but need some serious hands-on time. I figured if we're gonna be here a while, I should at least document how this blog came into existence.

### Step 1. Acquire a Domain Name
Originally I bought the domain name about a month ago so that I could cash in on a deal that a friend of mine was doing with [ProtonMail](http://protonmail.com) so that I could get a secure email that I could use for whatever I so please. The only caveat was that this would require a personal domain name so that the appropriate routing could be applied. I chose [Namecheap](http://namecheap.com) as my registrar because I was familiar enough with how their DNS configuration worked and the steps I would need to get the email account setup. 

So far I haven't had any problems with Namecheap, but if you go about doing this yourself be sure to have a provider you are familiar enough with and that is giving you a good deal.

### Step 2. Hugo
![Hugo Logo](/img/hugo.png)

I chose to use Hugo as my static content generator of choice. I knew I wanted a static content blog, because I didn't want the hassle of managing a multi-part system like Wordpress, and the security nerd in me saw the incredible benefits of the minimal attack surface of static HTML code. Among the myriad of different blog "generators" I find that Hugo is the most straightforward to use for my purposes, since all I'm really looking for is a place to throw text and have it look nice enough. 

One of the more recent versions the `deploy` command was added, which was the final thing I needed to jump into Hugo. After I'm done writing or making an edit, I simply have to run `hugo` to build the site (which is pretty fast) and `hugo deploy` to upload the entire site to my S3 bucket (more on that in a second). I'm not entirely sure how the upload and build processes will scale as the site grows, but it appears to only upload the changes to the site, which will severely cut down on bandwidth needs.

### Step 3. S3 Bucket
![S3 Logo](/img/s3.png)

To host the site, really any of the major cloud providers will do. I chose AWS S3 because I was previously familiar with it and knew that there was a simple feature to turn on public site hosting. Using the Hugo CLI and the AWS CLI I have Hugo directly access the bucket content is placed in, and push only the generated static-content to the bucket.

### Step 4. Cloudfront Distrubution and ACM
![Cloudfront Logo](/img/cloudfront.png)

This next step proved to be fairly simple, once I learned the patience required. In order to apply an SSL certificate to my site, I would need to go through Cloudfront as a way to cache the site and serve it from an area where AWS could have their certificate manager could sign the origin of the site. The biggest lesson I learned was to wait the extra few hours while the DNS validation occured, and not simply assuming that somehow I broke the validation (which can happen when using a registrar like Namecheap). 

After acquiring the appropriate SSL certificate and attaching that to the Cloudfront Distrubution, my site was now not only accessible from anywhere in the world lightning fast, but also had a cache that kept the site alive in the event something happened with my bucket. This does sometimes backfire, as the cache takes a few minutes to rebuild. Thus, a post might not show for a few minutes after being committed to the above-mentioned S3 bucket. 

### Step 5. Content
At the time of writing, this part is still kinda in fluctuation. Currently I'm using [NeoVim](http://neovim.io) as my editor of choice (as part of a larger project I'll probably write about at some point). My dotfiles can be found on my Github page, and are honestly a better way to understand my writing setup than me showing you how I'm writing now. 

### Price
Here's the part that's a bit misleading. At the time of this writing, my AWS account falls under the category of "Free for 12 months" which means that I'm not currently paying for the few megabytes of S3 storage and the minimal calls to my Cloudfront distrubution. Other than that the only cost I'm incurring is the domain name I bought back in December of 2019, meaning that this entire blog is being run for the price of the name on it, which comes out to roughly 2$ a month. While this may change in the future, it seems that the goal is to keep this static site's costs to as minimal as can be found.

### Conclusion
Overall I'm fairly happy with how this blog has turned out. There are definitely places to improve. Currently I've only scratched the surface on Hugo, and my Vim configuration is definitely not suited for writing articles, and I am still referencing the Markdown documentation on my second screen, but overall this turned out rather successful, and hopefully can turn into a pretty sweet spot on the internet. 

### References (People who explain it way better than me)
<https://medium.com/@channaly/how-to-host-static-website-with-https-using-amazon-s3-251434490c59>

<https://medium.com/@sbuckpesch/setup-aws-s3-static-website-hosting-using-ssl-acm-34d41d32e394>

<https://daringfireball.net/projects/markdown/syntax>

<https://favicon.io/favicon-generator/>

<https://gohugo.io>

Update 2/26/2020:
Major shoutout to <https://simpleit.rocks/golang/hugo/deploying-a-hugo-website-to-aws-the-right-way/#41-lambdaedge-function-installation> who helped make prettyURLs a reality, using Lambda@Edge functions and a little bit of NodeJS. All while still under budget!
