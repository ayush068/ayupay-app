# Configuration & Secret Management Guide

## 1. Environment Parameters Reference

| Variable | Default Value | Description |
| :--- | :--- | :--- |
| `NODE_ENV` | `development` | Runtime environment (`development` or `production`) |
| `CONTROL_CENTER_PORT` | `8090` | Host port for the Local Process Supervisor |
| `PAYMENT_API_PORT` | `6100` | Host port for the central Payment Core REST API |
| `CHECKOUT_PORTAL_PORT` | `7000` | Host port for the customer-facing Checkout page |
| `MERCHANT_ADMIN_PORT` | `6001` | Host port for the Merchant Admin Dashboard |
| `HELP_PORTAL_PORT` | `3000` | Host port for the Customer Payment Help Portal |
| `GMAIL_OBSERVER_PORT` | `6003` | Host port for the Gmail Polling Service & OAuth Callback |
| `SMS_OBSERVER_PORT` | `6002` | Host port for the Google Messages / SMS Web Automator |
| `DATABASE_URL` | `postgresql://...` | PostgreSQL connection URL |
| `USE_SQLITE_FALLBACK` | `true` | When true, uses embedded SQLite if PostgreSQL is unreachable |
| `SQLITE_DB_PATH` | `./data/youpi_pg.sqlite`| Path to local SQLite database file |
| `ENCRYPTION_MASTER_KEY` | `64-hex chars` | 256-bit key for encrypting OAuth credentials and device keys |
| `JWT_SECRET` | `32+ chars` | Secret key for customer and admin session tokens |
| `CLIENT_PORTAL_ADMIN_KEY` | Required for issuing client accounts | Private server-side key requested when an admin creates or resets a merchant client login |
| `PAYMENT_ACTIVE_WINDOW_MS` | `300000` | Duration of active payment slot (5 minutes = 300,000ms) |
| `PAYMENT_PROCESSING_WINDOW_MS` | `30000` | Duration of verification slot (30 seconds = 30,000ms) |
| `LATE_OBSERVATION_WINDOW_MS` | `1800000` | Safety window for matching late observations (30 minutes) |
| `ANDROID_ENROLLMENT_SECRET` | `32+ chars` | Ephemeral secret token used to authorize device QR pairing |
| `ANDROID_HEARTBEAT_TIMEOUT_MS` | `180000` | Inactivity window before Android device is marked DISCONNECTED |

---

## 2. Cryptographic Secret Generation

Generate strong 256-bit encryption and HMAC keys using Node.js native crypto:

```bash
# Generate 256-bit Master Key (64 hex characters)
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

Never commit production `.env` files to source code repositories.

## 3. Separate Merchant Client Portal

Set a unique random `CLIENT_PORTAL_ADMIN_KEY` in the payment API server environment. In the Merchant Admin panel, select a merchant and choose **Client Login**. Enter the server key when prompted; the portal shows the merchant's login URL and a generated temporary password once. Share the URL, client code, contact email, and password with that merchant through a private channel.

Clients sign in at `/client?client=<merchant-slug>`. The client portal is read-only and scoped to its signed-in merchant. Sessions expire after 12 hours; issuing a replacement password revokes that merchant's existing client sessions. Serve the portal over HTTPS in production.
