# The "podcast:bonusItem" Specification

<small>Version 1.0 by [Franco Solerio](https://github.com/francosolerio)</small><br>
<small>January 10th, 2022</small>

## Purpose

With Value4Value podcasters can give their audience the possibility to send payments in response to the action of listening to a show. After a few months from its invention many shows already demonstrated that audiences embrace this possibility and reward podcasters, creating a virtuous loop that favours the production of more and better content. It is also evident that single listeners contribute in very different degrees, from big amounts to nothing. This can be attributed to many reasons, just to name a few: use of apps that still don't support Podcasting 2.0, the listener's financial condition, culture ad good will, the podcaster's ability to stimulate contribution both by improving the quality of their show and istigating listeners to contribute.

The purpose of the `<bonusItem>` block is to give podcasters another way to stimulate active contribution from the listeners by publishing bonus content on the same RSS feed, and making it available on the same app the listener already uses to consume regular episodes.

Being available only on apps that support the specification, other than encouraging the financial contributions from the listeners, bonus content would give a competitive advantage and stimulate the adoption of the apps that support Podcasting 2.0 features.


## Process

When the app parses the RSS feed, it stores and displays both the content of the usual `<item>` blocks available in the standard RSS specification and the bonus items contained in `<bonusItem>` blocks. The former are displayed as usual, while bonus content is displayed in a way that marks it as available for playing only when some conditions are met.

The conditions can be specified in the `<bonusItem>` block in terms of amount of money contributed in a specific timeframe. More than one alternative condition can be specified.

Examples:
- A bonus episode can be played by listeners who contributed with at least 10'000 sats in the last month.
- An hystoric archive of episodes from the previous years is available for those who sent at least 100'000 sats in their lifetime.


## Structure:

The structure of a `<bonusItem>` block is identical to that of a regular `<item>` block, with the addition of a `<condition>` sub-item:

```xml
<podcast:bonusItem>
    <title>...</title>
    <pubDate>...</pubDate>
    <guid isPermaLink="false">...</guid>
    <enclosure url="..." length="..." type="..." />
    ...
    <condition 
        minimumAmount="[payed amount(integer)]" 
        timeInterval="[seconds(integer)]" 
    />
</podcast:bonusItem>
```

### Sub-elements of \<bonusItem\>
 - `<condition>` (required) This specifies the mandatory condition to make the bonus item available to the user.
    It has one required and one optional attribute. 

#### Attributes of \<condition\>
 - `minimumAmount` (required) specifies the minimum amount the user has to contribute to have access to the bonus item. The type of payment is the one specified in the \<podcast:value\> block contained at the \<channel\> level. 
 -  `timeFrame` (optional) specifies the time, in seconds, to be considered for reaching the specified amount. If not present the time interval to be considered valid to reach the minimum amount is infinite.
