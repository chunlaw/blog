---
title: Email Migration to Google Workspace
date: 2022-10-20 11:11:27 +0800
categories: [Google, Email]
tags: [gmail, data migration]     # TAG names should always be lowercase
---

There is a recent task to import emails from [Zoho](https://www.zoho.com) to [Google Workspace](https://workspace.google.com/). In view of the ~40-year history of email and its popularity, it is prone to attack. Email service providers have constructed many different kinds of protection scheme to safe-guard their servers and users' emails. There are several approaches I have tried to migrate the emails, and finally I have get it done by Google Workspace Data Migration Service.

## Best solution

Using the [data migration service](https://admin.google.com/ac/dms) provided by Google Workspace, the migration will be processing in the Google-provided server, by-passing all the issues of rate limit, authentication, backup....

## Failed Approach - Thunderbird

[Thunderbird](https://www.thunderbird.net/) is one of the best email client I have used, although it deleted all my precious secondary school emails. By this approach, I have first connected both Zoho and Google email accounts with their IMAP & SMTP settings. After fixing the Google authentication issue by using OAuth login, I had started the email copying process from Zoho to Gamil through Thunderbird UI. The whole process worked well for the email download process from Zoho, while it then soon hanged in "Sending login information" to Gmail. Googled a bit, no surprise that there is promising solution. Give up sharply!
