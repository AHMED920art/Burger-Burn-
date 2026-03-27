# Email Setup Guide

This burger barn website now has a functional contact form with email integration using **Nodemailer** (for sending) and **IMAP** (for receiving/retrieving emails).

## Installation

1. **Install dependencies:**
```bash
npm install
```

2. **Create a `.env.local` file** in the root directory (copy from `.env.local.example`):
```bash
cp .env.local.example .env.local
```

3. **Configure environment variables** in `.env.local`:

### For Gmail (Recommended)

If using Gmail:

1. Enable **2-Factor Authentication** on your Google Account
2. Generate an **App Password**:
   - Go to [Google Account Security](https://myaccount.google.com/security)
   - Click "App passwords"
   - Select Mail and Windows (or your OS)
   - Copy the generated password
3. In `.env.local`:
   ```
   EMAIL_USER=your_email@gmail.com
   EMAIL_PASSWORD=your_app_password
   IMAP_EMAIL=your_email@gmail.com
   IMAP_PASSWORD=your_app_password
   ```

### For Other Email Services

Update the `service` in `src/app/api/contact/route.ts` and configure accordingly:
- **Outlook:** service: 'outlook'
- **Yahoo:** service: 'yahoo'
- **Custom SMTP:** Update host, port, and auth

## How It Works

### Contact Form Submission
1. User fills out the contact form on the website
2. Form data is sent to `/api/contact` endpoint
3. An email is sent to the restaurant
4. A confirmation email is sent to the user

### Retrieving Emails (Admin)
1. Use the `/api/emails` endpoint to retrieve unread contact form emails
2. Requires bearer token authentication with `ADMIN_SECRET`

**Example request:**
```bash
curl -H "Authorization: Bearer your_secure_admin_secret_here" \
  http://localhost:3000/api/emails
```

## Running the Application

```bash
npm run dev
```

Then visit `http://localhost:3000` and test the contact form.

## Security Notes

- Never commit `.env.local` to git (it's in `.gitignore`)
- Use strong `ADMIN_SECRET` values
- For production, use environment variable management services
- Gmail's app passwords are more secure than storing your actual password

## Troubleshooting

- **"Failed to send email"**: Check your email credentials in `.env.local`
- **Gmail rejection**: Ensure you've enabled app passwords and 2FA
- **IMAP not retrieving emails**: Verify IMAP is enabled in your email service settings
