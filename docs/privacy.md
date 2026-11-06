# MuscleMetria Privacy Policy

**Last updated: 2026-11-05**

## Data Collection

MuscleMetria stores the following data to provide its services:

- **Workout logs**: exercises, sets, reps, weights, dates
- **Training plans**: plan names, day structures, exercises
- **Nutrition data**: food logs, meal plans, recipes, nutrition targets, diet execution status
- **Account information**: email address and Supabase user id if you create an account
- **Store subscription data**: server-verified App Store / Google Play
  transaction identifiers, subscription status, period dates, account-link
  identifiers, and purchase-token hashes used only for billing verification,
  support, renewals, refunds, and revocations
- **Coach-client data**: invite codes, plan/diet-plan assignments, planned-vs-actual execution snapshots, check-ins, form checks, coach notes, tags, and enhanced athlete records
- **Health and progress data**: optional Apple Health / Health Connect records
  from the categories you select: basic activity/body measurements and energy,
  workouts/distance/speed, body composition/metabolism, sleep, heart/recovery,
  nutrition/hydration, other health measurements (such as blood pressure,
  glucose, oxygen saturation or temperature), and optional menstrual flow.
  Availability depends on the platform, source records and your permissions.
  Each record retains measurement time, source, unit and data quality. Other
  optional progress data includes body measurements, progress photos and
  form-check photos/videos; location-derived region hints are separate from
  health data. We do not read workout GPS routes or ECG waveforms in this flow.
- **Optional AI inputs**: structured feature inputs, source text, health/body context, and photos you choose to submit for AI recognition or plan generation

All fitness and nutrition data is stored **locally on your device** by default.

## Health Choices and Purpose

- Reading health records, using categories in AI suggestions, and sharing them
  with a connected coach are separate choices. Additional categories start off.
  System permissions remain under your control in Apple Health / Health Connect.
- We read a bounded recent window (up to 30 days; shorter for frequent samples).
  Unavailable or denied data remains unknown. We do not infer that missing data
  is zero, or combine overlapping devices as extra activity.
- AI health context is sent only when you choose to use AI with health data,
  for the categories permitted for that purpose. Activity/nutrition choices may
  also include recent App training execution and food-log summaries. The input
  distinguishes observed records, estimates, plans and actual logs.
- When signed in, selected coach-sharing categories and sharing preferences may
  be uploaded to Supabase so an approved, connected coach can follow them.
  Other optional health categories are not uploaded merely because they can be
  read locally. The coach Web app does not directly read your device health store.
- We do not sell health data or use it for advertising or marketing. Health data
  and AI suggestions are not intended for diagnosis, treatment, emergencies or
  real-time medical monitoring.
- Turning off reading stops future reads through this flow; disabling AI use
  stops future AI submissions of that category. Changing coach access needs a
  network connection to take effect on the server. It does not withdraw data
  already viewed by a coach or already processed for a prior request. Existing
  local/cloud records are not automatically claimed deleted when a switch is
  changed; export and verified account deletion remain available below.

## Cloud Sync

If you choose to sign in, your data is synced to **Supabase** (a secure cloud database). This enables:
- Data backup and recovery
- Coach-client features (sharing plans with your coach)
- Multi-device sync

Coach media is stored in a private Supabase Storage bucket. The app stores
private storage paths, and the coach Web dashboard uses short-lived signed URLs
only when database RLS allows that coach to see the referenced row.

## Data Sharing

- Your data is **never sold** to third parties
- If you use the coach feature, your assigned coach can view your workout logs, nutrition data, and planned-vs-actual execution snapshots
- Accepted, approved coaches can also view submitted check-ins, form checks,
  coach-visible progress photos, health categories you permit for sharing,
  body measurements, notes/tags they create, and enhanced athlete records they
  create for that coaching relationship. Server policies enforce health
  category access as well as the active relationship.
- You can revoke coach access at any time

## Data Export & Deletion

- Cloud export is requested through the backend-owned `export_my_data()` RPC,
  with `export_user_data` reserved as an optional Edge Function wrapper. The
  mobile app must not assemble a partial hard-coded export locally.
- Cloud deletion requires recent verification through the backend. Password
  users use `POST /v1/account/delete`; OAuth-only and passwordless-email users
  use `POST /v1/account/delete-email/start` followed by
  `POST /v1/account/delete-email/confirm` with the email code sent by Supabase
  Auth. The backend stores only a hash of the short-lived deletion token, binds
  it to the authenticated Supabase user id, calls
  `delete_my_account(p_delete_token)` to delete related app rows and the
  Supabase Auth user, then deletes private `coach-media` objects through the
  Supabase Storage API. Direct SQL deletes from `storage.objects` are blocked
  by Supabase Storage, and an existing app session alone is never sufficient for
  deletion.
- Deleting your MuscleMetria account does not cancel an App Store or Google
  Play subscription. You can manage or cancel renewal in the store subscription
  settings before or after account deletion.
- After account deletion, MuscleMetria removes app profile, health, nutrition,
  training, coach, AI usage, and Auth rows associated with the account. It keeps
  only minimal anonymized billing rows such as store transaction identifiers,
  purchase-token hashes, subscription status/period, and payload hashes needed
  to process future renewals, refunds, revocations, duplicate-restore checks,
  and support requests. These retained billing rows are detached from the
  deleted Supabase user id and do not include email, profile, health, nutrition,
  workout, coach, or raw receipt/JWS/purchase-token data.
- Export/deletion failures return an explicit unavailable/error result. The app
  does not claim that all data was deleted unless the backend confirms success.

## Third-Party Services

MuscleMetria uses the following third-party services:
- **Supabase** (database & authentication)
- **Cloudflare Workers** (AI backend and request authorization)
- **OpenFoodFacts** (barcode food lookup)
- **USDA FoodData Central** (food database lookup)
- **Volcengine Ark / Doubao** (optional AI generation and photo recognition)
- **SMTP2GO configured in Supabase Auth** (account email delivery)
- **Apple App Store / Google Play** (subscription payment platforms)

AI features are optional and require backend authorization and available quota.
When you use AI, your selected inputs are sent through Cloudflare Worker to
Volcengine Ark / Doubao to produce the requested result; permitted health
context is part of those inputs only as described above. External food database
queries are rate-limited and cached only in a privacy-preserving way. Vendor
health SDKs/OAuth sources require separate approval, consent and disclosure;
prepared or disabled adapters do not mean those services are connected.

## Contact

For privacy questions, contact: zuoyang0601@gmail.com

---

This app is for fitness tracking purposes. It does not provide medical advice.
