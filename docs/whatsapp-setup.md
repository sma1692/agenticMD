# WhatsApp Bridge Setup (Meta WhatsApp Business Cloud API)

Chat with `warden` from WhatsApp and get reminders there. Uses the official Cloud API, not whatsapp-web.js.

## 1. Create the Meta app

1. Open https://developers.facebook.com/apps and click **Create app**.
2. Pick use case **Other**, then app type **Business**. Link (or create) a Business portfolio.
3. In the app dashboard, find **WhatsApp** and click **Set up**.

## 2. Test number and recipient

1. Go to **WhatsApp > API Setup**.
2. Meta gives you a free **test phone number**. Copy its **Phone number ID** (not the phone number itself).
3. Under **To**, click **Manage phone number list** and add your own WhatsApp number. Enter the code WhatsApp sends you.
4. Send the sample `hello_world` template from that page to confirm delivery.

## 3. Access token

- **Quick start:** copy the **Temporary access token** from API Setup. It expires after about 24 hours.
- **Permanent:** go to https://business.facebook.com/settings/system-users and do the following:
  1. Add a **System User** with the **Admin** role.
  2. Click **Assign assets**, select your app, and grant **Full control**.
  3. Click **Generate new token**, select the app, set expiry to **Never**, and tick `whatsapp_business_messaging` and `whatsapp_business_management`.
  4. Copy the token. Meta shows it only once.

## 4. App secret

Go to **App settings > Basic** and copy the **App secret**. The bridge uses it to verify webhook signatures.

## 5. Environment variables

Add these to `.env` at the Commando root:

```bash
WHATSAPP_API_TOKEN=EAAG...                 # temp or System User token
WHATSAPP_PHONE_NUMBER_ID=123456789012345   # from API Setup
WHATSAPP_CEO_NUMBER=919812345678           # your number, digits only, country code first; comma-separate extras
WHATSAPP_VERIFY_TOKEN=pick-any-long-random-string
WHATSAPP_APP_SECRET=abc123...              # App settings > Basic
WHATSAPP_GRAPH_VERSION=v26.0               # optional
WHATSAPP_REMINDER_TEMPLATE=commando_reminder   # optional, see section 9
WHATSAPP_TEMPLATE_LANG=en_US               # optional
```

Generate a verify token with `openssl rand -hex 24`. Restart the orchestrator after editing `.env`.

## 6. Expose port 4000

```bash
brew install cloudflared
cloudflared tunnel --url http://localhost:4000
```

Copy the `https://<random>.trycloudflare.com` URL it prints. The ngrok alternative is `ngrok http 4000`.
Quick tunnels change URL on every restart. Update the webhook URL in Meta each time.

## 7. Configure the webhook

1. Go to **WhatsApp > Configuration > Webhook** and click **Edit**.
2. Set **Callback URL** to `https://<tunnel>/webhook/whatsapp`.
3. Set **Verify token** to the exact value of `WHATSAPP_VERIFY_TOKEN`.
4. Click **Verify and save**. The orchestrator log shows `[WHATSAPP] webhook verified by Meta`.
5. Under **Webhook fields**, click **Subscribe** next to `messages`.

Check verification locally:

```bash
curl "http://localhost:4000/webhook/whatsapp?hub.mode=subscribe&hub.verify_token=$WHATSAPP_VERIFY_TOKEN&hub.challenge=ok"
# prints: ok
```

## 8. Test

1. From your phone, send `hi warden` to the test number.
2. The message gets blue ticks. If warden takes more than 5 seconds, you get an "On it" reply first, then the real answer.
3. Messages from numbers outside `WHATSAPP_CEO_NUMBER` are ignored.

## 9. The 24-hour window and reminder templates

- WhatsApp delivers free-form text only within 24 hours of your last message to the business number.
- Outside that window, Graph returns error `131047`. The bridge logs it and reports `outsideWindow: true`.
- Fix for reminders: send any message to the number each day, or use an approved template.

Create a reminder template:

1. Go to **WhatsApp Manager > Message templates** (https://business.facebook.com/wa/manage/message-templates) and click **Create template**.
2. Pick category **Utility**, name `commando_reminder`, and language **English (US)**.
3. Set the body to `Commando reminder: {{1}}` and give a sample value.
4. Submit the template. Approval usually takes minutes.
5. Set `WHATSAPP_REMINDER_TEMPLATE=commando_reminder` and `WHATSAPP_TEMPLATE_LANG=en_US`.

When free-form text fails with `131047`, the bridge resends it through this template, with the text (up to 1000 characters, on one line) as `{{1}}`.

## Going beyond the test number

- The test number can message only up to 5 verified recipients.
- To use a real number, add it under **WhatsApp > API Setup > Add phone number** and complete business verification.
