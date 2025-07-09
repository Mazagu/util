# 📦 Deployment Strategies – Quick Guide

## 🚀 1. Recreate Deployment

Shuts down the current version and deploys the new one.

```
Pros:
- Simple to implement

Cons:
- Downtime during deployment
```

---

## 🟢 2. Rolling Deployment

Replaces old instances with new ones *gradually*.

```
Pros:
- Minimal downtime
- Easy to rollback partially

Cons:
- Monitoring complexity
```

---

## 🔁 3. Blue-Green Deployment

Two environments: **Blue (current)** and **Green (new)**. Switch traffic to Green once validated.

```
Pros:
- Instant rollback
- No downtime

Cons:
- Doubles infrastructure temporarily
```

---

## 🌈 4. Canary Deployment

Releases the update to a small % of users first, then gradually expands.

```
Pros:
- Safer for user feedback
- Risk is minimized

Cons:
- Needs traffic routing and monitoring setup
```

---

## 🧪 5. A/B Testing

Deploys different versions to different user segments to test impact (e.g., UI or pricing models).

```
Pros:
- Ideal for experimentation and product optimization

Cons:
- Complex analysis and user segmentation required
```

---

## 🧯 6. Shadow Deployment

New version runs alongside production, but doesn’t serve users — just receives traffic.

```
Pros:
- Detects issues early
- Non-invasive validation

Cons:
- Doesn’t test user interaction
```

---

## 💣 7. Feature Flag Deployment

Deploy all code, but use feature flags to toggle features on/off per user or group.

```
Pros:
- High flexibility
- Enables rollback without redeploying

Cons:
- Can lead to technical debt if flags are not managed
```

---

## 🧠 Final Tips

- Always monitor performance and errors post-deploy.
- Use observability tools like **Prometheus**, **Grafana**, or **Datadog**.
- Automate rollbacks on error thresholds.
