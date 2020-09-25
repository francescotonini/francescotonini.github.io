---
title: "My solution to the Google HashCode 2020 online round"
description: My C# solution to the "Book" problem of Google HashCode 2020 online round
author: "Francesco Tonini"
date: 2020-02-08T21:20:35+02:00
lastmod: 2020-04-20T16:55:00+02:00
tags: 
  - "google"
  - "hashcode"
  - "csharp"
slug: "hashcode2020"
---

Hi everyone! This post goes through the story behind the development of my solution for the Google HashCode 2020 online round. If you have never heard of Google HashCode, it is a team coding competition made by Google to solve engineering problems. There is no programming language constraints, just a problem to solve in a fixed amount of time. After the online round, the best teams will be invited by Google for the final round.

Yesterday's problem was to plan which books to scan from a set of libraries. Each book has its own score and the goal was to maximize the total score of scanned books. The full problem statement will be available on the Google HashCode website shortly.

Enough of that, let's make our hands dirty. This solution is available on [GitHub](https://github.com/francescotonini/hashcode-books) and it is written in C# (don't be scared by that, if you known a bit of Java and lambda you'll be fine).

## A naive approach
First of all, I had to find a metric to sort each library and pick the best ones. I decided to calculate the score of each library, then sort them in descending order, and output it.

![Oh boy, that's bad](https://dev-to-uploads.s3.amazonaws.com/i/plaeo5lm7rjkrljsv1kx.png)

Unsurprisingly, the total score was terrible. What I didn't take into consideration were two main facts: first, two or more libraries may have the same book, but I only need to scan it once; second, what should I do when two libraries have the same score? How should I prioritize one over the other?

## Less duplicates, more score?
So I was back to the drawing board. This time, when I am facing two or more libraries with the same score, I pick the one with the least signup time, so that to allow more books to be scanned in parallel. Also, the score of a library takes into account only books that haven't been scanned before. That should fix it!

## So, job done?
![Well yes, but actually no](https://dev-to-uploads.s3.amazonaws.com/i/5ou6vmoc7z6u7rvmht6u.jpg)

While I did improve the score on one dataset, the others were just like the first attempt. Also, the dataset "D" was really slow, but we'll get back to it later on.

So, as you may expect, I was back to the drawing board again; yet, something wasn't adding up. What if instead to sorting by score, then signup time, I do signup time first, then score?

{{<gist francescotonini 531ae2d1064179411844999b8969ab02 "Program.cs" >}}

Oh well, that was the kind of improvement I was looking for.

![That's better](https://dev-to-uploads.s3.amazonaws.com/i/3vafiffhkjq8hh35qv4f.png)

## About dataset "D - tough choices"
While I was happy with the overall result, I had to find a solution for dataset "D", which on the above implementation was painfully slow. While looking at the data I realized that all books have a score of 65, meaning that I didn't need to calculate the score, I just had to multiply the number of books that can be scanned every day by the number of days and 65. This was fundamental to keep the execution time at a reasonable level (remember that we have a limited amount of time).

{{<gist francescotonini 531ae2d1064179411844999b8969ab02 "Library.cs" >}}

This has been a hell of a ride, but it was worth it. It's not the perfect score, but I am more than happy about the result.

If you would like to see my implementation, head over to [GitHub](https://github.com/francescotonini/hashcode-books). If you like this article, please consider sharing it with friends and colleagues. Ciao!
