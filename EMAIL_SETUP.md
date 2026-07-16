# Email Configuration Setup

## Overview
The Strategic Exit application needs to send PDF reports via email. This guide explains how to configure email settings.

## Option 1: Gmail SMTP (Recommended for Development)

### Step 1: Enable 2-Factor Authentication
1. Go to your Google Account settings
2. Enable 2-Factor Authentication
3. Generate an App Password for this application

### Step 2: Configure Environment Variables
Add these to your `.env` file:

```bash
# Email Configuration
DJANGO_EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
DJANGO_EMAIL_HOST=smtp.gmail.com
DJANGO_EMAIL_PORT=587
DJANGO_EMAIL_USE_TLS=True
DJANGO_EMAIL_HOST_USER=your-email@gmail.com
DJANGO_EMAIL_HOST_PASSWORD=your-app-password
DJANGO_DEFAULT_FROM_EMAIL=Strategic Exit <your-email@gmail.com>
```

### Step 3: Test Configuration
Restart your Django application and test the email functionality.

## Option 2: SendGrid (Recommended for Production)

### Step 1: Create SendGrid Account
1. Sign up at [SendGrid.com](https://sendgrid.com)
2. Verify your domain or use single sender verification
3. Get your API key

### Step 2: Configure Environment Variables
```bash
# Email Configuration
DJANGO_EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
DJANGO_EMAIL_HOST=smtp.sendgrid.net
DJANGO_EMAIL_PORT=587
DJANGO_EMAIL_USE_TLS=True
DJANGO_EMAIL_HOST_USER=apikey
DJANGO_EMAIL_HOST_PASSWORD=your-sendgrid-api-key
DJANGO_DEFAULT_FROM_EMAIL=Strategic Exit <noreply@yourdomain.com>
```

## Option 3: Mailgun (Alternative)

### Step 1: Create Mailgun Account
1. Sign up at [Mailgun.com](https://mailgun.com)
2. Add your domain
3. Get your API key

### Step 2: Configure Environment Variables
```bash
# Email Configuration
DJANGO_EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
DJANGO_EMAIL_HOST=smtp.mailgun.org
DJANGO_EMAIL_PORT=587
DJANGO_EMAIL_USE_TLS=True
DJANGO_EMAIL_HOST_USER=postmaster@yourdomain.mailgun.org
DJANGO_EMAIL_HOST_PASSWORD=your-mailgun-password
DJANGO_DEFAULT_FROM_EMAIL=Strategic Exit <noreply@yourdomain.com>
```

## Testing Email Configuration

### Method 1: Django Shell
```python
python manage.py shell

from django.core.mail import send_mail
send_mail(
    'Test Email',
    'This is a test email from Strategic Exit.',
    'from@example.com',
    ['to@example.com'],
    fail_silently=False,
)
```

### Method 2: Application Test
1. Complete a business assessment
2. Click "Get PDF Report via Email"
3. Check if email is received

## PDF File Management

### How It Works
The application now uses a server-side approach for PDF handling:

1. **PDF Generation** - PDF is generated in memory
2. **Server Storage** - PDF is temporarily saved to server (`media/temp_pdfs/`)
3. **Email Attachment** - PDF is attached to email from server file
4. **Cleanup** - Temporary file is deleted after email is sent

### Cleanup Management
To clean up any orphaned temporary PDF files:

```bash
# Clean files older than 1 hour
python manage.py cleanup_temp_pdfs

# Force clean all temporary files
python manage.py cleanup_temp_pdfs --force
```

### Benefits
- ✅ **Better memory management** - Large PDFs don't stay in memory
- ✅ **Reliable email attachment** - File-based attachment is more stable
- ✅ **Automatic cleanup** - Temporary files are deleted after use
- ✅ **Fallback cleanup** - Management command for orphaned files

## Troubleshooting

### Common Issues:

1. **TLS/SSL Configuration Conflict**
   - **Error**: "EMAIL_USE_TLS/EMAIL_USE_SSL are mutually exclusive"
   - **Solution**: Only set one of these to True:
     - For Gmail: `DJANGO_EMAIL_USE_TLS=True`, `DJANGO_EMAIL_USE_SSL=False`
     - For SSL servers: `DJANGO_EMAIL_USE_TLS=False`, `DJANGO_EMAIL_USE_SSL=True`
   - **Note**: The application automatically handles this conflict in settings

2. **Authentication Failed**
   - Check username/password
   - Ensure 2FA is enabled for Gmail
   - Use App Password, not regular password

3. **Connection Timeout**
   - Check firewall settings
   - Verify SMTP port (587 for TLS, 465 for SSL)
   - Ensure TLS is enabled

4. **Email Not Received**
   - Check spam folder
   - Verify recipient email address
   - Check sender domain reputation

### Debug Mode
To see email details in console, temporarily set:
```bash
DJANGO_EMAIL_BACKEND=django.core.mail.backends.console.EmailBackend
```

## Security Notes

1. **Never commit email credentials** to version control
2. **Use environment variables** for all sensitive data
3. **Enable TLS/SSL** for secure email transmission
4. **Use App Passwords** instead of regular passwords
5. **Verify sender domain** to improve deliverability

## Production Recommendations

1. **Use dedicated email service** (SendGrid, Mailgun, AWS SES)
2. **Set up domain verification** for better deliverability
3. **Monitor email metrics** (delivery rates, bounces)
4. **Implement email templates** for consistent branding
5. **Set up email logging** for debugging

## Support

If you encounter issues:
1. Check the Django logs for error messages
2. Verify your email service credentials
3. Test with a simple email first
4. Contact your email service provider for support
