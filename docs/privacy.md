# MuscleMetria Privacy Policy

**Last updated: 2026-11-06**

Version note: The locale-only startup and separate label-scan AI choice are
introduced in 3.1.110 (build 281). Until you install the released update, some
older versions may request location after login to set initial units, and may
automatically review uncertain nutrition labels when AI is enabled and eligible.
The separate-choice descriptions below apply to clients with the new controls;
this policy date alone does not update an installed client.

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
- **Optional AI inputs**: structured feature inputs, source text, permitted
  health/body context, and photos you choose to submit for AI recognition or
  plan generation. Nutrition-label scanning starts with on-device OCR. If an
  AI review is offered, a separate choice for that scan explains that its photo
  and recognized text will go through Cloudflare to Volcengine Ark / Doubao.
  Keeping the local result or dismissing the choice does not send that scan
  to the AI provider. AI availability alone is not consent for that scan.

All fitness and nutrition data is stored **locally on your device** by default.

## Region and Location

- At App startup, the system locale can initialize the region used for unit
  defaults. This does not request device location; existing region and custom
  unit preferences are preserved.
- In the App profile editor, choosing **Detect region** can request location
  permission and a low-accuracy position, then use the platform's geocoding
  provider to obtain a country/region. The provider may process the coordinates
  under its own platform terms. This flow does not track background location
  or store/upload the raw coordinates to MuscleMetria's backend.
- The country code saved with your unit preferences can sync to Supabase when
  signed in and may be included in training or nutrition AI requests you start.
  The region text you enter or detect in your profile is saved on your device.
  It can help select display units and may be sent when you request regional
  food suggestions or an AI nutrition plan. You can change the region manually.
- Coach Web can request browser location permission on opening to calculate
  local sunrise/sunset for its appearance. That calculation uses coordinates
  in the browser; this flow does not upload them to MuscleMetria's backend.
  If location is unavailable or denied, the Web app uses its time-based
  appearance fallback. Browser/platform location services have their own
  permission and processing rules.

## Photos, Camera, and Documents

You can choose the camera, selected photos or documents to scan barcodes or
nutrition labels, recognize food/equipment/plans, or submit progress and
form-check media. When a barcode is looked up online, the product code is sent
to Open Food Facts; this does not require sending a label photo to the AI
provider. Nutrition-label OCR runs locally first; its optional third-party AI review requires the scan
choice described above. Other AI-labelled recognition or plan features send
the inputs you choose for that task. When you submit a progress photo while
signed in, the App uploads it to private Supabase storage. A connected,
authorized coach can view only photos marked for coach sharing. Form-check
media is shared with the coach you submit it to. Choosing a private progress
photo does not mean the photo is stored only on your device.

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

- You can find data export and account deletion in Settings. You can also
  delete supported individual records, plans and media within their editors.
  Public help explains [account deletion](https://zuoyangjkpi.github.io/MuscleMetria-releases/delete-account)
  and [requests to delete specific data](https://zuoyangjkpi.github.io/MuscleMetria-releases/delete-data).
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
- **Device/browser location and geocoding services** (optional region detection
  and Coach Web sunrise/sunset appearance, as described above)

AI features are optional and require backend authorization and available quota.
When you use AI, your selected inputs are sent through Cloudflare Worker to
Volcengine Ark / Doubao to produce the requested result; permitted health
context is part of those inputs only as described above. External food database
queries are rate-limited and cached only in a privacy-preserving way. Vendor
health SDKs/OAuth sources require separate approval, consent and disclosure;
prepared or disabled adapters do not mean those services are connected.

For a nutrition-label AI review, the selected photo and its recognized OCR text
are AI inputs only after the separate scan choice. You can retain and edit the
local result instead. A reviewed result may still retain more trustworthy local
values; displaying local values does not mean no AI request was made if you
chose to send the scan for review.

## Contact

For privacy questions, contact: zuoyang0601@gmail.com

---

This app is for fitness tracking purposes. It does not provide medical advice.
