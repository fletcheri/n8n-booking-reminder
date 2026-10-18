# Booking Reminder (Lite): free n8n workflow for Australian businesses

One reminder SMS before each booking, so fewer customers forget you're coming.

- Webhook in (`/webhook/booking-lite/booking`) → one reminder SMS via Twilio `reminderHoursBefore` (default 24h) before the booking
- Booking times read in your timezone (AEST/AEDT handled), `YYYY-MM-DD HH:mm` or AU `DD/MM/YYYY HH:mm`; reminders that would land outside your sending hours are skipped
- Resending the same `bookingRef` updates the time instead of creating a duplicate
- AU number normalisation (04xx → +614xx); only texts mobiles
- STOP/START opt-out handling (business name + "Reply STOP to opt out" in every text)
- Daily SMS cap and optional webhook secret (`sharedSecret`)

## Setup (about 15 minutes)
1. n8n → **Import from file** → `workflow/booking-reminder-lite.n8n.json`.
2. Open **Decide** and set `businessName`, `timezone`, `sms.from` (your Twilio AU mobile in +61 format) and `sms.twilioAccountSid`.
3. On **Send SMS (Twilio)**, add an HTTP Basic Auth credential: User = Account SID, Password = Auth Token.
4. Activate. Set your Twilio number's **Messaging** webhook to `https://<your-n8n>/webhook/booking-lite/sms-inbound` (for STOP).
5. For each booking, POST `{"bookingRef":"B-1","name":"Jo","phone":"0491 570 006","start":"2026-11-05 09:00"}` to `https://<your-n8n>/webhook/booking-lite/booking`.

Costs (Twilio, Oct 2026): about US$0.0515 (≈A$0.074) per SMS and US$8.25/month (≈A$11.90) for an AU mobile number.

## Want more?
The **Pro pack** adds:
- instant booking confirmation, plus a same-day "see you in 2 hours" heads-up
- Y to confirm / R to reschedule / CANCEL replies, with owner alerts
- "your booking has moved" messages when a time changes, and cancellations
- a 6am owner run sheet showing who's confirmed
- arrival windows ("between 7am and 9am"), and confirmations queued until morning when booked late at night
- email with an Add to Google Calendar link and one-click unsubscribe
- ClickSend support, dry-run mode and a status page
- AU compliance notes, setup docs and test scripts

**Want the full Australian version?** The Booking Reminder Pack (Pro) adds confirmations, Y/R replies, the morning run sheet, email, ClickSend and full setup docs. Store link coming soon; questions to aussieflows@mail.grokbot.com.

[Notify me when Pro is out](mailto:aussieflows@mail.grokbot.com?subject=Notify%20me%20-%20Booking%20Reminder%20Pack).

## More free AU workflows
Other free n8n workflows for Australian small businesses (MIT, same setup style):
- [Missed-Call Text-Back](https://github.com/fletcheri/n8n-missed-call-textback): text back missed callers in seconds
- [Google Review Request](https://github.com/fletcheri/n8n-google-review-request): one honest Google review ask after each job
- [Quote Follow-Up](https://github.com/fletcheri/n8n-quote-follow-up): a polite follow-up SMS after you send a quote
- [Overdue Invoice Reminder](https://github.com/fletcheri/n8n-overdue-invoice-reminder): an overdue-invoice email a week after the due date

MIT licence. Not legal advice.
