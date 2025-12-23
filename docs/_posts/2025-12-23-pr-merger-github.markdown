---
layout: default
title:  "Pr Merger to Auto Squash Commits to readable message"
date:   2025-12-23 12:00:00 +0530
categories: github
---

# Automating My GitHub Squash Workflow

I love squash commits, but I hate formatting them.

Every PR merge involved the same tedious dance: copying the Jira ticket numbers, formatting the title to match our `feat(scope):` convention, and manually pasting it all into GitHub's merge box.

So, I built a Chrome extension to do it for me.

### The Problem

GitHub's default squash message is just a list of every commit message in the branch. It's noisy and often useless. I wanted a clean, standardized format:

```text
feat(billing): Fix race condition in invoice generation

Tickets:
- https://jira.company.com/browse/BILL-123
- https://jira.company.com/browse/BILL-456

PR: #1024
```

### The Solution: PR Merger ⚡️

I wrote a simple extension that injects an **"✨ Auto Squash"** button right into the GitHub UI.


When clicked, it scans the page DOM, grabs the branch name (to determine if it's a `feat`, `fix`, or `chore`), extracts all ticket references from the description, and formats perfectly according to my preferences.

Instead of fighting with GitHub's hidden form inputs, I built a non-intrusive **Sidebar UI** that slides in from the right. It gives me a single "Copy All" box, so I can review the message and paste it with one click.

![Screenshot of the Sidebar UI showing the formatted message]

### How it Works

It's a lightweight `content.js` script < 200 lines. No API tokens, no complexity. It just observes the DOM and helps you out when you need it.

You can check out the source code (and install it yourself) here:

[**git-mitra/pr_merger on GitHub**](https://github.com/git-mitra/pr_merger)
