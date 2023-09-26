---
title: licensing example
---
https://rocicorp.dev/terms.html

```
License pings
Per Replicache Pricing, we charge post-funding/revenue commercial customers based on Monthly Active Browser Profiles, meaning unique browser instances that instantiate Replicache in a calendar month. The way we accomplish this is to send a ping to our servers containing your license key and a unique browser profile identifier when Replicache is instantiated, and every 24 hours that it is running. We also check at instantiation time that your license key is valid, and complain loudly to the console if it is not. We may in the future add a feature to disable Replicache in the event that the license key is not valid.

The licensing pings explain why you want to pass TEST_LICENSE_KEY to Replicache in automated tests: so that you're not potentially charged for large numbers of Replicache instances used when running tests. (Not to mention network calls are typically undesirable in unit tests).

Disabling Replicache's pings other than via the TEST_LICENSE_KEY is against our Terms of Service. If the pings are a problem for your environment, please get in touch with us at hello@replicache.dev.
```