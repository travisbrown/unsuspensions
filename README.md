# Elon Musk's suspension reversals

This repository contains data related to Twitter accounts that have been unsuspended since Elon Musk took over as owner and CEO in late October 2022.

## Warning! Please read this section

Every date given here for a suspension or suspension reversal indicates when the account status change was observed, not when it happened.
The Twitter API doesn't directly provide information about _when_ accounts are suspended or unsuspended, so we have to infer these dates by
querying user profiles and timelines, and these queries are governed by the Twitter API's rate limits.

This means that these dates are inherently inexact. If an account is listed as suspended on 15 November and unsuspended on November 29,
that means that it was suspended _no later_ than the 15th and was unsuspended between the 15th and the 29th.

For accounts that follow or are followed by many accounts on our seed list (see below for details),
the lag between account status change and observation is likely to be on the order of hours, not days.
For accounts that have a small number of followers or are outside of the networks we are tracking, the lag may be days, or even weeks
(at least in theory—I've not found observations that are off by weeks in my spot checking, but it's possible that they're in there).

These limitations mean that all numbers given here are estimates, and that the data should be considered a starting point for investigation,
not a definitive collection of unsuspended accounts.

## Table of contents

* [The report](report/README.md) (rendered tables for each day of unsuspensions, with links, etc.)
* [A CSV spreadsheet](data/timestamps.csv) with three columns: Twitter ID, suspension observation timestamp (if known), and reversal observation timestamp
* [A newline-delimited JSON file](data/profiles.ndjson) containing a profile snapshot for each unsuspended account

All timestamps are epoch seconds.

## Frequently asked questions

> Where's Jordan Peterson?

As far as I know there's no way to tell from the Twitter API whether an account is restricted because of rule violations.
That means that this report can only track permanent suspensions, not accounts that are "temporarily unavailable" or have
their activity locked. That means you won't find `@jordanbpeterson` or `@TheBabylonBee` on here.

> Where's Patrick Casey?

Casey seems to have deactivated [his original twitter account](https://twitter.com/PatrickCaseyUSA) almost
immediately after it was unsuspended, and [is now tweeting under his own name from one of his
ban evasion accounts](https://twitter.com/travisbrown/status/1597822538692055040).
In cases like this my software might not detect the unsuspension before the self-deactivation,
which means the account will not appear in these reports.

> Where's Andrew Anglin?

There's been [a lot of reporting](https://www.rollingstone.com/politics/politics-news/elon-musk-twitter-reinstates-neo-nazi-andrew-anglin-account-1234640390/)
about how Andrew Anglin was "booted off" the platform in 2013 and is now back,
but as far as I can tell, Anglin's 2013 account [has never been permanently suspended](https://twitter.com/murphtracks/status/1598937629504200704),
and his tweets from 2013 have been visible under a different screen name for at least most of this year.
(Like Peterson and the Babylon Bee, he may have been locked out of his account, but I have no way of confirming that.)

> Where are the "[62,000 accounts with more than 10,000 followers](https://www.platformer.news/p/why-some-tech-ceos-are-rooting-for)"?

I don't know. The reporting from earlier this week said this process has begun, but I haven't seen a timeline.
It doesn't seem to have started yet in any serious way.

> Will this software find all of the "[75 accounts with over 1 million followers](https://www.platformer.news/p/why-some-tech-ceos-are-rooting-for)"?

Currently it can only find unsuspensions for accounts that are included in a list of 955k known suspensions. We're working on raising that number.

> How often is this repository updated?

The system is continuously updating the dataset, but this repository is only updated when I check in to copy stuff over and publish it.

## How this data was collected

This repository is built on the same data collection system as [this project](https://github.com/travisbrown/twitter-watch),
which tracks Twitter suspensions and screen name changes.
The system monitors follower and friend lists for a set of seed accounts, and when an account either falls off of
or appears on one of those lists, it is evaluated as a potential suspension or suspension reversal.

This repository only tracks accounts that have been suspended (API status code 63), not self-deactivated (status code 50).

The seed account list currently contains just over five thousand Twitter accounts,
and is made up of selections from several sources, including the "promoter" community in the [VoterFraud2020](https://voterfraud2020.io/) dataset
from the [Jacobs Technion-Cornell Institute](https://tech.cornell.edu/jacobs-technion-cornell-institute/) at Cornell Tech.
The focus of the seed account list is far-right influencers, but it also includes anti-vaxxers, TERFs, Russian disinformation accounts,
accounts associated with the "Intellectual Dark Web", etc.

The system additionally scrapes recent tweets for accounts that are identified as suspension reversals.
In some cases this lets us find a more precise time of reinstatement, but it's slow (because of API rate limits),
which means that over time some dates provided here will change to become more precise. They will only move earlier, never later.

I have an additional dataset (not included here) currently containing around 6.5 million recent tweets from these unsuspended accounts.
I'm happy to share this data with anyone who has a use for it.

## Report format

The report provides two tables for each day. The first table includes the 50 accounts with the most followers observed to be unsuspended on that day.
The second table contains other "notable" accounts for that day, ranked by how closely they are connected (by following relationships) to the seed accounts.

In the "Status" column in these tables, :wave: indicates that the account has self-deactivated after unsuspension, :no_entry_sign: that it has already been suspended again, :heavy_check_mark: that it is verified (in the non-Blue sense), and :lock: that it is protected.

The follower count numbers in the report are from the last snapshot the data collection system took of each account, which means they may not be up to date.
If the snapshot was taken immediately after the suspension reversal, the number reported may be much lower than the actual number,
since Twitter seems to take some time to update these lists after an unsuspension.

## I'm back tweets

The table below lists a few examples of accounts announcing their unsuspension.
I don't have automation for this, and am unlikely to have time to add many. Contributions are appreciated.

|Twitter ID|Screen name|Date|Link|
|----------|-----------|----|----|
|28524726|amerika_blog|28 October 2022|https://archive.today/2022.11.28-052728/https://twitter.com/amerika_blog/status/1585817570116272128|
|757230184831524865|AlexJMcNabb|28 November 2022|https://archive.today/2022.12.01-161644/https://twitter.com/AlexJMcNabb/status/1597280844128784384|

## Credit

The data collection and indexing software used here was developed by me ([Travis Brown](https://twitter.com/travisbrown)) earlier this year with support from a grant from [Prototype Fund](https://prototypefund.de/en/).
