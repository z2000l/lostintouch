# Google Play — Data Safety form draft

**DRAFT — for your review before submitting.** This is a legal declaration
you make to Google, not me — read it over, correct anything that doesn't
match your understanding, then fill in Play Console → your app → **App
content → Data safety** with these answers yourself.

Based on a direct read of the app's code as of this build (relationships,
posts, check-ins, invites, presence). Assumes propersh's Supabase project
is the only backend — Supabase is a data **processor** (infrastructure
you run, not an independent third party you share data with), so nothing
below is marked "Shared."

## Account deletion — resolved

Built: **Settings → Delete account** in the app, behind a confirmation
dialog. Calls a `delete_own_account()` database function that removes
the user's auth identity, which cascades to delete every related row
(relationships, posts, checkins, invites, invite_recipients, event
cards, presence, gate timestamps) in one transaction, plus their post
images from storage. Verified live against the production database with
a real test account before this was marked resolved.

## Data collection and security

- **Does your app collect or share any of the required user data
  types?** Yes
- **Is all user data collected by your app encrypted in transit?** Yes
  (Supabase client uses HTTPS/TLS throughout)
- **Do you provide a way for users to request their data be deleted?**
  Yes — in-app account deletion (Settings → Delete account). When this
  form asks *how*, answer "The app has a way for users to request that
  their data is deleted" and select the in-app option; no URL needed.

## Data types

### Personal info

| Type | Collected | Shared | Purpose | Optional? |
|---|---|---|---|---|
| Name | Yes | No | App functionality | Required (asked at signup) |
| Phone number | Yes | No | App functionality (account identity/OTP sign-in, finding people to connect with) | Required (it's the sign-in method) |

### Location

| Type | Collected | Shared | Purpose | Optional? |
|---|---|---|---|---|
| Approximate location | Yes | No | App functionality (shows in-touch connections roughly where you are) | Optional — declining the OS location permission just disables this one feature |

Not precise location: raw GPS coordinates are read on-device to look up
a city name and are never transmitted or stored — only the resulting
city/region string is sent to the backend.

### Photos and videos

| Type | Collected | Shared | Purpose | Optional? |
|---|---|---|---|---|
| Photos | Yes | No | App functionality (attaching a photo to a post) | Optional — posts don't require a photo |

### App activity

| Type | Collected | Shared | Purpose | Optional? |
|---|---|---|---|---|
| Other user-generated content (post text, check-in messages, invite details) | Yes | No | App functionality | Optional (using these features is optional; the app is unusable without *some* interaction, but no single field is mandatory beyond name/phone) |

### Not collected

No ads, no analytics/tracking SDKs, no crash reporting SDK, no financial
info, no health data, no contacts/calendar access, no precise location,
no device/advertising identifiers, no web browsing history, no audio.
(Confirmed by checking the app's actual dependencies — no analytics,
tracking, or ad libraries are present.)
