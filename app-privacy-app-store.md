# App Store Connect — App Privacy ("Nutrition Label") draft

**DRAFT — for your review before submitting.** Fill this in yourself at
App Store Connect → your app → **App Privacy**, in the "Data Used to
Track You" and "Data Linked to You" sections. Based on a direct read of
the app's code as of this build.

## Account deletion — resolved

Built: **Settings → Delete account** in the app, behind a confirmation
dialog, satisfying Guideline 5.1.1(v). Removes the user's auth identity
(cascades to delete every related row: relationships, posts, checkins,
invites, invite recipients, event cards, presence, gate timestamps) plus
their post images from storage. Verified live against the production
database with a real test account before this was marked resolved.

## Data Used to Track You

**None.** Answer "No" to "Do you or your third-party partners collect
data from this app to track users?" — there's no advertising SDK, no
cross-app/cross-site tracking, no data broker sharing. This also means
you don't need an App Tracking Transparency (ATT) prompt.

## Data Linked to You

Everything below is linked to the user's account (tied to their `auth`
identity), not anonymous, and used only for the app's own functionality
— nothing here is used for tracking, third-party advertising, or sold.

| Category | Data type | Purpose |
|---|---|---|
| Contact Info | Name | App Functionality |
| Contact Info | Phone Number | App Functionality (sign-in identity) |
| Location | Coarse Location | App Functionality (ambient presence shown to in-touch connections) |
| User Content | Photos or Videos | App Functionality (post attachments) |
| User Content | Other User Content | App Functionality (post text, check-in messages, invite details) |
| Identifiers | User ID | App Functionality (account/session identity) |

**Not** "Precise Location": the app reads GPS coordinates on-device only
to resolve a city name via reverse geocoding: the raw coordinates never
leave the device or get stored — only the resulting city/region string
is sent to the backend. Apple's own definition of "collection" is data
that's transmitted off the device, so the momentary on-device GPS read
doesn't count.

## Data Not Collected

No: precise location, financial info, health/fitness, browsing history,
search history, purchases, usage data (analytics), diagnostics/crash
data, contacts, or any identifiers used for advertising. No third-party
analytics, crash-reporting, or ad SDKs are present in the app's
dependencies.

## Data used for App Functionality only

Every row above should be marked "App Functionality" as its purpose —
none should be marked Analytics, Advertising, or Product Personalization,
since none of that is actually happening in this app.
