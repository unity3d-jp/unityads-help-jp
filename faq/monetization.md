/*  
Title: Monetization FAQs  
Sort: 2  
*/

#### Integration FAQs

* [Which version of Unity should I be using?](#which-version-of-unity-should-i-be-using-)
* [Which game engines or frameworks are supported?](#which-game-engines-or-frameworks-are-supported-)
* [Which platforms are supported by Unity Ads?](general#which-platforms-are-supported-with-unity-ads-)
* [Why am I not able to see any ads in game?](#why-am-i-not-able-to-see-any-ads-in-game-)
* [Why am I only seeing 2 or 3 ads at a time?](#why-am-i-only-seeing-2-or-3-ads-at-a-time-)
* [What are some games that currently use Unity Ads?](#what-are-some-games-that-currently-use-unity-ads-)
* [Is it possible to block certain types of ads from my game?](#is-it-possible-to-block-certain-types-of-ads-from-my-game-)
* [Is it possible to block ads from certain developers?](#is-it-possible-to-block-ads-from-certain-developers-)
* [Can the daily number of available ads be limited?](#can-the-daily-number-of-available-ads-be-limited-)
* [Can Unity Ads be implemented through mediation?](#can-unity-ads-be-implemented-through-mediation-)

#### Stats FAQs

* [What do the fields in the monetization stat reports mean?](#what-do-the-fields-in-the-monetization-stat-reports-mean-)
* [Is it possible to have impressions, but have 0 _ad requests_?](#is-it-possible-to-have-impressions-but-have-0-_ad-requests_-)
* [Is it possible to automatically generate stat reports?](#is-it-possible-to-automatically-generate-stat-reports-)
* [How often are monetization stats updated?](#how-often-are-monetization-stats-updated-)

#### Revenue FAQs

* [Why am I not seeing any revenue, even with 100 impressions?](#why-am-i-not-seeing-any-revenue-even-with-100-impressions-)
* [Is revenue earned by completed views, or based on installs?](#is-revenue-earned-by-completed-views-or-based-on-installs-)
* [How much money can I expect to make with my game?](#how-much-money-can-i-expect-to-make-with-my-game-)

#### Payment FAQs

* [How does the payment process work?](#how-does-the-payment-process-work-)
* [Can you send payments to my PayPal account?](#can-you-send-payments-to-my-paypal-account-)
* [What information is required in the invoice?](#what-information-is-required-in-the-invoice-)
* [Can I be paid as an individual instead of as a comnpany?](#can-i-be-paid-as-an-individual-instead-of-as-a-comnpany-)
* [Do I need to pay taxes on payments as an individual?](#do-i-need-to-pay-taxes-on-payments-as-an-individual-)
* [Do I need to provide a VAT number?](#do-i-need-to-provide-a-vat-number-)
* [When can I expect to be paid?](#when-can-i-expect-to-be-paid-)
* [My earnings balance was deducted by the amount invoiced, but I have yet to receive payment. What is the reason for this?](#my-earnings-balance-was-deducted-by-the-amount-invoiced-but-i-have-yet-to-receive-payment-what-is-the-reason-for-this-)

----

#### Which version of Unity should I be using?

We always recommend using the [latest release][1] or [patch release][2] version of Unity. However, we advise against using Unity Ads with beta releases of Unity since they can be unstable.

If you're using Unity 5.2 or later, it's strongly recommend that you use the in-engine version of Unity Ads through the [Services window in Unity][3].

If for whatever reason you prefer not to use the Services window, you can still use the latest [Unity Ads asset package][4] instead, available for free through the Unity Asset Store. The Unity Ads asset package supports Unity 4.7 and later.

In any case, it's always recommend to use the latest version of Unity Ads.

> _**Note:** Unity isn't a requirement for using Unity Ads. If you're using a different engine or framework, you can still use the Unity Ads SDK._

[⇧ Back to top](#integration-faqs)

----

#### Why am I not able to see any ads in game?

1. Have you initialized Unity Ads? Are you using the correct game ID? Note that these are platform-dependent, so use an iOS game ID on an iOS game.
1. Are you waiting for Unity Ads to be ready before attempting to show the ads? It usually takes a few seconds to download the ad and before this - IsReady() returns false.
1. Is your device connected to the Internet?
1. Have you shown too many ads for the day? There's a hard limit of 25 ads per day per user. Mark your device as a test device from the [dashboard][150] if you want to see more - and use test ads.


----

#### Why am I only seeing 2 or 3 ads at a time?

Please be aware, the availability of ads is not guaranteed. This is why we provide methods to first check if ads are ready, and can be shown.

**Users are limited by default to 25 ads per day (on a rolling 24hr basis) per app. Users are also limited to seeing the same ad only once per hour.**

For most of Europe and North America, this typically is not an issue. In countries where fill may be somewhat limited, the effects of this can be more obvious. For instance, if there are only 12 ad campaigns available in a user's country, they will only be able to see up to 12 ad campaigns per hour.

In any case, if the user exhausts their daily limit of 25 ads, they won't see any more ads until the next day. During development, it's recommended that you enable test mode to ensure you will always have test ads available.

[⇧ Back to top](#integration-faqs)

----

#### What are some games that currently use Unity Ads?

There are many games successfully monetizing with Unity Ads. These are just a few good examples:
* _**Crossy Road**_ ([iOS][5], [Android][6]) -- [Made with Unity][7].
* _**Temple Run 2**_ ([iOS][8], [Android][9]) -- [Made with Unity][10].
* _**Sonic Dash**_ ([iOS][11], [Android][12]) -- Made with Unity.
* _**4 Pics 1 Word**_ ([iOS][13], [Android][14]) -- Native SDK integration.
* _**Dumb Ways to Die**_ ([iOS][15], [Android][16] -- Native SDK integration with Adobe AIR.

[⇧ Back to top](#integration-faqs)

----

#### Is it possible to block certain types of ads from my game?

You can select to filter out genres in your [Unity Ads dashboard][150].

Please [contact us][100] if this feature is not available at the moment.

[⇧ Back to top](#integration-faqs)

----

#### Is it possible to block ads from certain developers?

Yes you can. Contact unityads-support@unity3d.com and tell what you want to block and we will block them for you.

[⇧ Back to top](#integration-faqs)

----

#### Can the daily number of available ads be limited?

Yes you can. Contact unityads-support@unity3d.com and we will configure this for you.

[⇧ Back to top](#integration-faqs)

----

#### Can Unity Ads be implemented through mediation?

Yes of course. Some supported are MoPub, Fyber. We have adapters ready from them in our Github.

[⇧ Back to top](#integration-faqs)

----

#### What do the fields in the monetization stat reports mean?

Unity ads reports use the following terms:

* **started**: Recorded when a video begins playing.
* **views**: Recorded when a video completes.
* **Ad requests** Recorded during SDK initialization.
* **Available** Recorded when an **Ad Request** is successful.
* **Fill rate** is the number of **available** over the number of **ad requests**. (**Available/Requests**)

There are a few things to be aware of:

1. Unity Ads will provide a list of up to 25 video ads when initialized.

2. As long as there is at least one ad available, the response is only counted as 1 **available** for each **ad request** regardless of the number of ads available.

3. For performance reasons, only every 23rd **ad request** and **available** is counted and multiplied by 23 to obtain it's value. This is why you'll notice values for **ad request** and **available** are multiples of 23.

4. After showing 10 videos, an **ad request** is sent to refresh the list of available ads.

[⇧ Back to top](#stats-faqs)

----

#### Is it possible to have impressions, but have 0 _ad requests_?

Yes, and the reason for this is because **ad requests** are a sampled value.

Impressions are counted each time a video is **started**. Only every 23rd **ad request** is recorded due to sampling. If you have only a handful of ad requests per day, only every 13th of them is added to the statistics.

[⇧ Back to top](#stats-faqs)

----

#### Is it possible to automatically generate stat reports?

You can get automated reports from the Publisher admin panel to be sent to your email. Go to Reports => Automated Reports, configure the report and select the frequency and the emails to send it to.

[⇧ Back to top](#stats-faqs)

----

#### How often are monetization stats updated?

Hourly statistics are updated a few minutes after the hour and daily statistics 15 minutes past UTC midnight.

The report you can access on the admin panel is not realtime. Report is updated in hourly, but the revenue information from the ads can take up to seven days to be reported back to our system.

[⇧ Back to top](#stats-faqs)

----

#### Why am I not seeing any revenue, even with 100 impressions?

Typically it takes 5000 impressions or more to determine the quality of the users you are sending. You will be getting revenue based on the quality your users and the impressions you are showing.

[⇧ Back to top](#revenue-faqs)

----

#### Is revenue earned by completed views, or based on installs?

Unity Ads generates revenue through generating views and traffic from your game, and you get paid on that.

There's no "one unified way" of pre-calculating the revenue generated to you as we run different types of campaigns (with different billing points, ie. completed views, clicks or installs).

Our system always selects the best (ie. in terms of money) possible campaigns for your users.

[⇧ Back to top](#revenue-faqs)


----

#### How much money can I expect to make with my game?

The revenue for your game depends on many factors - which platform are you on (iOS or Android), from which country your players are from, what kind of placements you do, etc., but most important is the number of players you have.

Typically the revenue delivered per 1000 video starts is between 6 .. 12 USD, depending on the country.

[⇧ Back to top](#revenue-faqs)

----

#### How does the payment process work?

Unity Ads currently only offers payments by wire transfer. To request payment, you must submit an [invoice][17] to [accountspayable-fi@unity3d.com][200]. The minimum earnings balance required to submit an invoice is $100 USD.

When you submit an invoice for the first time, you need to include your bank account information using the [Bank Info Form][18].

Please see the [Payment Request Documentation][19] for more details.

[⇧ Back to top](#payment-faqs)

----

#### Can you send payments to my PayPal account?

No, unfortunately Unity Ads does not use PayPal for earnings payments at this time. However, we are working to provide additional methods of payment.

[⇧ Back to top](#payment-faqs)

----

#### What information is required in the invoice?

The basic things we require are listed on Unity Ads payment request instructions (see above). For payment your banking details are needed, including a proper SWIFT code. Our financial system cannot process payments to bank accounts without a SWIFT code. The items you have to present in your invoice depend on your country’s finance laws, so we can’t provide a general template. You can find many templates with search engines.

[⇧ Back to top](#payment-faqs)

----

#### Can I be paid as an individual instead of as a company?

Yes we can pay you. However we handle private persons a bit differently:

- When asked to enter a company name, simply enter your own name instead.

- As for VAT, EU members are only required to enter a VAT number if they currently have one. If you are a EU member, you don't need to apply for a VAT number to use Unity Ads. If you do not have a VAT number, simply enter "none" in the field when prompted.

For more information about payment requests, please review the following document:
https://secure.applifier.com/unityads/files/UnityAds_payment_request_october.pdf

[⇧ Back to top](#payment-faqs)

----

#### Do I need to pay taxes on payments as an individual?

In general, there are no taxes to be deducted from payments to companies or similar entities. However, if you are a private person, the payments are governed by tax agreements between your country and Finland.

These agreements are in place to reduce any possible double taxation and sets guidelines for personal taxation rules. In general, the taxes for ad income are usually between 0 and 15%, but can also go higher for certain countries.

You should consult your own tax office for further guidance or ask our support for specific tax percentages.

**We won't deduct any taxes from your payment. Handling taxes for your revenue is on your response.**

[⇧ Back to top](#payment-faqs)

----

#### Do I need to provide a VAT number?

VAT (Value Added Tax) number is required in European Union. The local legislation in each country specifies when you need to apply for a VAT number.

If you do not have it, just state ”No VAT” to the field. If you have one, please state it to the corresponding field and also to each invoice in EU format.

[⇧ Back to top](#payment-faqs)

----

#### When can I expect to be paid?

Once you have invoiced your earnings from us, it will take 30 days for us to release the funds (upon the receipt of the invoice). Please add few business days for the wire to come through. Please send the invoices to the email address specified in this document, not to any personal email accounts. The invoices are thus recorded correctly and properly processed in our system.

[⇧ Back to top](#payment-faqs)

----

#### My earnings balance was deducted by the amount invoiced, but I have yet to receive payment. What is the reason for this?

When you see the funds deducted from your account, you can be sure the payment has been checked, processed and moved to payment.

It does not mean the funds have been paid, it means they have been cleared. The payment date is net 30 days upon receipt of the invoice. The balance won’t necessarily change immediately after you have sent the invoice to us.

[⇧ Back to top](#payment-faqs)


[1]: https://unity3d.com/get-unity/download/archive
[2]: https://unity3d.com/unity/qa/patch-releases
[3]: https://docs.unity3d.com/Manual/UnityAdsHowTo.html
[4]: https://www.assetstore.unity3d.com/en/#!/content/21027
[5]: https://itunes.apple.com/us/app/crossy-road-endless-arcade-hopper/id924373886?mt=8
[6]: https://play.google.com/store/apps/details?id=com.yodo1.crossyroad
[7]: https://madewith.unity.com/games/crossy-road
[8]: https://itunes.apple.com/us/app/temple-run-2/id572395608?mt=8
[9]: https://play.google.com/store/apps/details?id=com.imangi.templerun2
[10]: https://madewith.unity.com/games/temple-run-2
[11]: https://itunes.apple.com/us/app/sonic-dash/id582654048?mt=8
[12]: https://play.google.com/store/apps/details?id=com.sega.sonicdash
[13]: https://itunes.apple.com/us/app/4-pics-1-word/id595558452?mt=8
[14]: https://play.google.com/store/apps/details?id=de.lotum.whatsinthefoto.us
[15]: https://itunes.apple.com/fi/app/dumb-ways-to-die/id639930688?mt=8
[16]: https://play.google.com/store/apps/details?id=air.au.com.metro.DumbWaysToDie2
[17]: https://secure.applifier.com/unityads/files/Invoice_example-UnityAds.xlsx
[18]: https://secure.applifier.com/unityads/files/WireTransferForm-UnityAds.pdf
[19]: https://secure.applifier.com/unityads/files/PaymentRequest-UnityAds.pdf
[100]: ../help/contact
[150]: https://dashboard.unityads.unity3d.com
[200]: mailto:accountspayable-fi@unity3d.com
