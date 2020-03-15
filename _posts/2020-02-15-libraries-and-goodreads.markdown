---
layout: post
title: "Libraries and Goodreads"
date: 2020-02-15 18:20:00
---

**tl;dr - I made a site to find books at the [Waterloo Public Library](https://wpl.ca) that are on my [Goodreads](https://goodreads.com) reading list: [Goodreads WPL](https://olivercardoza.com/goodreads-wpl/).**

Over the last few years I've gotten into the habit of visiting my local library. I've also collected a long list of books to read which I track on [Goodreads](https://www.goodreads.com/). One problem that started to bug me was finding books from my list that are available at the library. In this post I'll introduce a small project I built to connect my library with Goodreads.

## Goodreads

What is Goodreads? Well it’s a website owned by [Amazon](https://www.amazon.com/) which combines equal parts social network and book catalog. Among other things, it lets users share status updates, write book reviews, and track books they’d like to read in lists called *shelves*.

<img src="/images/goodreads.png">

I like to [organize data](https://olivercardoza.com/2016/06/18/the-path-to-ledger.html) and track my habits. I’ve found the act of recording my behavior and sometimes reflecting on it to be a useful practice. When it comes to my reading habits, I make no exception. When I hear about an interesting book, I add it to my Goodreads reading list. When I read a book I mark it on Goodreads. Sometimes I even write a review!

## The Problem

My Goodreads reading list has accumulated over 100 books. It seems like my rate of finding interesting books exceeds my ability to read them. In any case, when I head to the library looking for a book I often pull up my Goodreads list while I’m there. As I scroll through I haphazardly pick a book that fits my mood and use the library computer to check availability. A given library session would typically involve 5 - 10 cross lookups to find a book that is both on my list and available. This didn’t bother me too much at first, but over a few years it’s starting to annoy me. Ideally I should be able to easily see the intersection of books that are available and on my list without having to perform 100 library lookups.

<img src="https://docs.google.com/drawings/d/e/2PACX-1vSddM-xhy9MUc15iMEexSrhS7zo7I-g6c6buU3_mTIpXafQWkF-CJXbCTfZ_PfAZTSNG4kj41fP_jV9/pub?w=521&amp;h=341" style="display: block; margin: auto;">

## Existing Solutions?

I typically prefer not to reinvent the wheel so I started my search for existing solutions. I can’t be the only Goodreads user that also visits the library.

### Goodreads Book Links

A quick Google search shows that Goodreads has some functionality to check library availability. It’s described in detail in this blog post from 2017’s Library Week

*    [Library Week: The Goodreads Hack to Find a Book at Your Library](https://www.goodreads.com/blog/show/868-library-week-the-goodreads-hack-to-find-a-book-at-your-library)

<img src="/images/goodreads_book_links.png">

This allows any user to add a custom link on every Goodreads book page to look it up at your library. This isn’t actually useful for me as I would still need to open all 100 Goodreads book pages to then click this custom link.

### Library Extension

<img src="/images/library_extension.png">

Up next on the journey was a little browser extension aptly named [Library Extension](https://www.libraryextension.com/). At first glance I was very impressed at the extensive library support:

> We support over 3200 library systems and consortiums across the Australia, Canada, Germany, New Zealand, the United Kingdom and the United States.

Seeing that it supported my local library ([Waterloo Public Library](https://www.wpl.ca/)) I installed it to check it out. The extension is triggered when you visit Goodreads book pages where it extracts the book info and performs a lookup in your configured libraries. After it has found the availability it renders the info right on the Goodreads book page.

Unfortunately this suffers the same problem as Goodreads book links. In order to make this work I would have to open up each book on my reading list to check availability. Another major limitation is that, being a browser extension, it does not work on my phone. I typically don’t carry around a laptop at the library so I’d either have to check it in advance at home/work or rent out a computer at the library.

## Solution

More Googling did not return any other options so I started to imagine what the ideal solution would look like. At its core I’d like a website that could display my Goodreads reading list with library availability annotated on each book all on a single page. Then some nice-to-have features would be for it to work on my phone and allow filtering the results to only books that are available. Mobile support makes it super convenient if I spontaneously find myself at the library.

Next up, how hard is this going to be to build? If it’s going to take longer than a few hours to make something basic then it’s pretty much dead to me. I’ll live with the minor pain. This is where a couple of key pieces come together. First off, I check and see that Goodreads has an [API](https://en.wikipedia.org/wiki/Application_programming_interface): [Goodreads API](https://www.goodreads.com/api). This means it should be easy to get my reading list from Goodreads. The second piece is how to look up library availability. A quick email to my local library ([Waterloo Public Library](https://wpl.ca)) confirmed my suspicion that they have no API. I had suspected this would be the hard part. However, I was given hope by one fact: the Library Extension was able to solve this problem at scale.

There are 1000s of libraries, but probably only 10s of library software suites. Looking at the Library Extension code in a Chrome debugger illustrated this point. It was able to interface with a few key library suites in order to achieve compatibility with 1000s of libraries across several countries. This includes libraries without an API. When a system has no API one common solution is to use [web scraping](https://en.wikipedia.org/wiki/Web_scraping). In the case of Library Extension this is exactly what they did.
## Decision

I now felt confident that building a solution would not take too much effort. This is partially due to past experience working with APIs, scraping, and web development.

I’m going to split off the act of building the project into a separate post. But if you are anxious to see what I’ve built here you go:

*   [Goodreads WPL](https://olivercardoza.com/goodreads-wpl)

