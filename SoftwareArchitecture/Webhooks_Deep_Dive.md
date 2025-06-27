# 🔔 Webhooks Deep Dive: Real-Time Communication Between Services

Webhooks are a lightweight, efficient way for applications to notify each other of events in **real-time** by sending **HTTP POST** requests to predefined URLs.

They are **event-driven**, push-based alternatives to polling, commonly used in APIs like Stripe, GitHub, Slack, and many others.

---

## 🚀 What is a Webhook?

A **webhook** is a mechanism where one system (sender) automatically sends an HTTP request (usually POST) to another system (receiver) when a certain event occurs.

### 🔁 How It Works

1. **Client registers a webhook URL** with the provider.
2. **Event happens** (e.g., payment succeeded).
3. Provider **sends HTTP POST** to the webhook URL with event payload.
4. Client **processes** the event.

---

## 🧪 Webhook Example: Stripe Payment

### 📤 Stripe Sends Webhook

```
POST /webhook
Content-Type: application/json
Stripe-Signature: t=162890...

{
  "id": "evt_123",
  "type": "payment_intent.succeeded",
  "data": {
    "object": {
      "id": "pi_456",
      "amount": 5000
    }
  }
}
```

### 📥 Your App Receives It

```
@app.route("/webhook", methods=["POST"])
def handle_webhook():
    event = request.get_json()
    if event["type"] == "payment_intent.succeeded":
        handle_success(event["data"]["object"])
    return "", 200
```

---

## 🔐 Webhook Security

Since webhooks are open endpoints, they need to be protected.

### ✅ Best Practices

1. **Verify signatures** (e.g., HMAC or API provider secret)
2. **Use HTTPS** only
3. **Restrict by IP** or firewall if possible
4. **Rate-limit** and throttle
5. **Do not trust user input**

### 🔹 HMAC Signature Verification (Node.js Example)

```
const crypto = require("crypto");

function verifySignature(payload, header, secret) {
  const hmac = crypto.createHmac("sha256", secret);
  hmac.update(payload, "utf8");
  const digest = hmac.digest("hex");
  return digest === header;
}
```

---

## 📦 Payload Format

- JSON is the standard format.
- Typically includes:
  - `event` or `type`
  - `timestamp`
  - `data` object
  - `id` of the event

Always refer to the provider’s docs for exact payload structure.

---

## 📈 Reliability & Best Practices

Webhooks are **fire-and-forget** by design. To ensure robustness:

### ✅ Best Practices for Consumers

- **Return 2xx quickly** (e.g., 200 OK) even if async processing
- **Queue webhook events** for durable background jobs
- **Handle duplicates** (idempotency is key)
- **Implement retries** and **dead-letter queues**
- **Log and monitor** events received

### ✅ Best Practices for Providers

- **Retry failed requests** with exponential backoff
- **Document webhook structure and behavior**
- Provide **test utilities** (e.g., GitHub/Stripe simulators)
- Allow **secret rotation**

---

## 🧰 Tools for Working with Webhooks

| Tool         | Purpose                                 |
|--------------|------------------------------------------|
| [ngrok](https://ngrok.com/)        | Expose local server to receive webhooks |
| [Webhook.site](https://webhook.site) | Test and inspect HTTP requests         |
| [RequestBin](https://requestbin.com/) | Temporary endpoint for debugging       |
| Postman      | Create mock endpoints & simulate payloads |
| [Svix](https://www.svix.com/)       | Hosted webhook management platform     |

---

## 🔄 Webhooks vs Polling vs Pub/Sub

| Feature       | Webhooks       | Polling        | Pub/Sub (e.g., Kafka) |
|---------------|----------------|----------------|------------------------|
| Push-based    | ✅              | ❌              | ✅                      |
| Real-time     | ✅              | ❌              | ✅                      |
| Easy to set up| ✅              | ✅              | ❌                      |
| Scalable      | ⚠️ (needs infra) | ✅              | ✅                      |
| Durable       | ❌              | ✅              | ✅                      |

> Use **webhooks for simple integrations**, and **Pub/Sub or message queues** for high-volume or critical systems.

---

## 🧩 Real-World Use Cases

- Stripe: `payment_intent.succeeded`
- GitHub: `push`, `pull_request`, `issues`
- Slack: custom slash commands and bots
- Shopify: order creation, cart updates
- Zapier: webhook-based integrations

---

## ✅ Checklist: Webhook Consumer

- [x] HTTPS endpoint
- [x] Signature verification
- [x] Return 2xx response immediately
- [x] Deduplicate events using `id`
- [x] Use retry + logging mechanism
- [x] Queue events for background processing

---

## 📚 Resources

- [Stripe Webhook Docs](https://stripe.com/docs/webhooks)
- [GitHub Webhooks](https://docs.github.com/en/webhooks)
- [Twilio Webhooks](https://www.twilio.com/docs/usage/webhooks)

---
