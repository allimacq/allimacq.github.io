---
layout: post
title:      "Project 1: Trail Accessibility"
date:       2020-12-16 18:06:19 +0000
permalink:  project_1_trail_accessibility
---


For my first project at FlatIron, I was required to build a CLI Application in Ruby. I decided to do one based on trail accessibility throughout the United States. The inspiration behind this came from one of my friends who uses a wheelchair. We have gone places before where the website says that it is accessible, only to find out that it's not.
So I found [this](/https://www.traillink.com/activity/wheelchair-accessible-trails/) website to scrape.

The overall idea is that a user is given all 50 States and DC to choose to look for accessible trails. They then see all the accessible trails in that state, and they are given the option to view trails only made of asphalt. From there, they can either choose a particular trail to see more information about it, or thye can go back up to view the States again and start over. If they choose to see a particular trail, they can then view a different trail in the selected state, they can start again, or they can exit.

I used Nokogiri and Open-uri to get all the information I required. I scraped a total of three different pages: the first, to get the States and the link to their pages; the second to scrape the selected State's accessible trail page; and the third to scrape the additional information about the user's selected state trail.

I definitely feel that I challenged myself on this project and learned a lot in the process. I used quite a few of the techniques that are taught in the first couple of modules. Things like: recurrsion, mass assignment, regex, and many staple's of Ruby OOP.
