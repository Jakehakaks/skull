# Silver City - Setup Guide

## Prerequisites

- Node.js 18+
- npm or yarn
- SQLite (local dev) or PostgreSQL (production)
- Stripe account (test mode for development)

## Quick Start

```bash
# Clone the repository
git clone [repo-url]
cd silver-city

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local
# Edit .env.local with your values

# Set up the database
npx prisma generate
npx prisma db push

# Create an admin account
# The default ADMIN_EMAILS in .env.local defines who can log in as admin.

# Run the development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## Stripe Setup (Required for Checkout)

1. Create a Stripe account at [https://dashboard.stripe.com](https://dashboard.stripe.com)
2. Go to Developers > API keys
3. Copy the **Publishable key** and **Secret key**
4. Add them to `.env.local`:
   ```
   STRIPE_SECRET_KEY=sk_test_...
   STRIPE_PUBLISHABLE_KEY=pk_test_...
   ```
5. For webhook testing, in a separate terminal:
   ```
   npx stripe listen --forward-to localhost:3000/api/webhook
   ```
   Copy the webhook secret and add it:
   ```
   STRIPE_WEBHOOK_SECRET=whsec_...
   ```

## Admin Access

Admin credentials are defined via environment variables:
- `ADMIN_EMAILS` - comma-separated list of admin emails
- `ADMIN_PASSWORD` - password for admin login

To log in:
1. Visit [http://localhost:3000/admin/login](http://localhost:3000/admin/login)
2. Enter your admin email and password
3. The system will create/upgrade your user to ADMIN role

## Uploading Products

1. Log in as admin
2. Go to `/admin/products/new`
3. Drag product images into the **DROP PRODUCT IMAGES HERE** area
   or click to browse files
4. Fill in product details: name, price, description, category, variants
5. Click **Preview** to see how it looks
6. Click **Save Draft** or **Publish**

## Database Schema

The database includes:
- **Users** - Customers and admins
- **Products** - With variants (color/size), images, categories, collections
- **Categories** & **Collections** - Product organization
- **Drops** - Limited releases with countdown
- **Cart** & **CartItems** - Persistent shopping cart (session-based for guests)
- **Orders** & **OrderItems** - Order records with payment/fulfillment status
- **Addresses** - Shipping and billing addresses
- **Wishlists** - Customer wishlists
- **NewsletterSubscribers** - Email subscribers
- **DiscountCodes** - Promotional codes
- **FaqItems** - FAQ content
- **ShippingSettings** & **SiteSettings** - Configurable settings
- **CitySignal** - Brand stories/content

## Environment Variables Reference

| Variable | Required | Description |
|---|---|---|
| `DATABASE_URL` | Yes | Database connection string |
| `NEXTAUTH_URL` | Yes | App URL (e.g. http://localhost:3000) |
| `NEXTAUTH_SECRET` | Yes | Random secret for session encryption |
| `STRIPE_SECRET_KEY` | Yes | Stripe secret key (test mode) |
| `STRIPE_PUBLISHABLE_KEY` | Yes | Stripe publishable key (test mode) |
| `STRIPE_WEBHOOK_SECRET` | Yes | Stripe webhook signing secret |
| `ADMIN_EMAILS` | Yes | Comma-separated admin emails |
| `ADMIN_PASSWORD` | Yes | Admin login password |
| `NEXT_PUBLIC_SITE_URL` | Yes | Public site URL |
| `UPLOAD_DIR` | No | File upload directory |
| `MAX_FILE_SIZE` | No | Max upload size in bytes |
| `EMAIL_FROM` | No | Sender email for notifications |
| `SMTP_HOST` | No | SMTP server host |
| `SMTP_PORT` | No | SMTP server port |
| `SMTP_USER` | No | SMTP username |
| `SMTP_PASS` | No | SMTP password |
| `GOOGLE_CLIENT_ID` | No | Google OAuth client ID |
| `GOOGLE_CLIENT_SECRET` | No | Google OAuth client secret |
| `GITHUB_ID` | No | GitHub OAuth client ID |
| `GITHUB_SECRET` | No | GitHub OAuth client secret |
