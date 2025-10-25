# SendGrid Dynamic Template Setup Guide

This application uses SendGrid for sending transactional emails. You can use either **plain HTML emails** (default fallback) or **SendGrid Dynamic Templates** for professionally designed emails.

## Quick Start

### Option 1: Use Plain HTML Emails (Current Default)
The app works out of the box with basic HTML emails. Just set:
```
SENDGRID_API_KEY=your_api_key
FROM_EMAIL=your_email@domain.com
ADMIN_EMAIL=admin@domain.com
```

### Option 2: Use SendGrid Dynamic Templates (Recommended)
For branded, professional emails, create templates in SendGrid and configure the template IDs.

## Setting Up SendGrid Dynamic Templates

### Step 1: Create Templates in SendGrid

1. Log in to [SendGrid Dashboard](https://app.sendgrid.com)
2. Navigate to **Email API** → **Dynamic Templates**
3. Create the following 5 templates:

#### Template 1: Welcome Email
- **Name**: SamosaMan - Welcome Email
- **Dynamic Fields**:
  - `{{employeeName}}` - Employee's name
  - `{{username}}` - Their username
  - `{{appUrl}}` - Application URL
- **Subject**: Welcome to SamosaMan!
- **Content Example**:
  ```
  Hi {{employeeName}},

  Welcome to SamosaMan! Your account has been created successfully.

  Username: {{username}}
  
  Your account is pending admin approval. You'll receive an email once approved.

  Best regards,
  SamosaMan Team
  ```

#### Template 2: Approval Email
- **Name**: SamosaMan - Account Approved
- **Dynamic Fields**:
  - `{{employeeName}}` - Employee's name
  - `{{loginUrl}}` - Login page URL
  - `{{appUrl}}` - Application URL
- **Subject**: Your SamosaMan Account is Active!
- **Content Example**:
  ```
  Hi {{employeeName}},

  Great news! Your SamosaMan account has been approved.

  You can now log in at: {{loginUrl}}

  Best regards,
  SamosaMan Team
  ```

#### Template 3: Low Inventory Alert
- **Name**: SamosaMan - Low Inventory Alert
- **Dynamic Fields**:
  - `{{inventoryList}}` - Array of objects with `type`, `current`, `threshold`
  - `{{dashboardUrl}}` - Admin dashboard URL
- **Subject**: ⚠️ Low Inventory Alert
- **Content Example**:
  ```
  Low Inventory Alert

  The following items are running low:

  {{#each inventoryList}}
  - {{this.type}}: {{this.current}} (threshold: {{this.threshold}})
  {{/each}}

  View dashboard: {{dashboardUrl}}

  SamosaMan System
  ```

#### Template 4: Report Submitted
- **Name**: SamosaMan - Report Submitted
- **Dynamic Fields**:
  - `{{employeeName}}` - Employee name
  - `{{marketName}}` - Market name
  - `{{reportDate}}` - Report date
  - `{{status}}` - "Submitted"
  - `{{dashboardUrl}}` - Dashboard URL
- **Subject**: New Report Submitted by {{employeeName}}
- **Content Example**:
  ```
  New Report Submitted

  Employee: {{employeeName}}
  Market: {{marketName}}
  Date: {{reportDate}}
  Status: {{status}}

  View in dashboard: {{dashboardUrl}}

  SamosaMan System
  ```

#### Template 5: Draft Report Saved
- **Name**: SamosaMan - Draft Report Saved
- **Dynamic Fields**: Same as Report Submitted
- **Subject**: Draft Report Saved by {{employeeName}}
- **Content Example**: Similar to Report Submitted

### Step 2: Get Template IDs

After creating each template:
1. Click on the template name
2. Copy the **Template ID** (format: `d-1234567890abcdef`)
3. Save all 5 template IDs

### Step 3: Configure Environment Variables

Add these to your Replit Secrets or `.env` file:

```bash
# Required for all email functionality
SENDGRID_API_KEY=SG.your_actual_api_key_here
FROM_EMAIL=noreply@yourdomain.com
ADMIN_EMAIL=admin@yourdomain.com
APP_URL=https://your-app.replit.app

# Optional: SendGrid Template IDs (if not set, falls back to HTML emails)
SG_TEMPLATE_WELCOME=d-your_welcome_template_id
SG_TEMPLATE_APPROVAL=d-your_approval_template_id
SG_TEMPLATE_LOW_INV=d-your_low_inventory_template_id
SG_TEMPLATE_REPORT_SUBMITTED=d-your_report_submitted_template_id
SG_TEMPLATE_DRAFT_SAVED=d-your_draft_saved_template_id
```

### Step 4: Test Your Setup

**Admin Authentication Required**: The debug endpoint requires admin login.

1. Log in as admin at `/login`
2. Visit the debug endpoint to send a test email to the admin email:
   ```
   GET /api/debug/send-test-email
   ```

The response will show:
- Whether the email was sent successfully
- Whether it used a dynamic template or HTML fallback
- The email address it was sent to (always ADMIN_EMAIL for security)

## Email Types & Triggers

| Email Type | Trigger | Recipient | Template Variable |
|------------|---------|-----------|-------------------|
| Welcome Email | Employee signs up | Employee | `SG_TEMPLATE_WELCOME` |
| Approval Email | Admin approves employee | Employee | `SG_TEMPLATE_APPROVAL` |
| Report Submitted | Employee submits report | Admin | `SG_TEMPLATE_REPORT_SUBMITTED` |
| Draft Saved | Employee saves draft | Admin | `SG_TEMPLATE_DRAFT_SAVED` |
| Low Inventory | Stock falls below threshold | Admin | `SG_TEMPLATE_LOW_INV` |

## Fallback Behavior

**Important**: If template IDs are not configured, the system automatically falls back to basic HTML emails. This means:
- ✅ Emails will still be sent
- ✅ All functionality works
- ⚠️ Emails will use simple HTML instead of branded templates

## Inventory Alert Thresholds

The system checks inventory after each report submission and sends alerts when stock falls below:
- **APP** (Apple): 150
- **VEG** (Veggie): 225
- **PJB** (Punjabi): 225
- **VSP** (Vermont Spicy Potato): 225
- **SCH** (Spicy Chicken): 300
- **CHC** (Chicken & Cheese): 300
- **STC** (Steak & Cheese): 300
- **STP** (Steak & Potato): 225

## Troubleshooting

### Emails not sending?
1. Check `SENDGRID_API_KEY` is set correctly
2. Verify sender email in SendGrid (Single Sender Verification)
3. Check server logs for error messages

### Template not working?
1. Verify template ID is correct (starts with `d-`)
2. Check template is published/active in SendGrid
3. Ensure dynamic field names match exactly (case-sensitive)

### Still using HTML emails?
- Template IDs are optional
- If not set, system uses HTML fallback
- Check environment variables are set correctly

## Design Tips

When creating templates in SendGrid:
- Use SamosaMan branding colors (Deep Teal: #2A9D8F)
- Include logo if available
- Keep mobile-friendly (max width 600px)
- Test emails before going live
- Use Handlebars syntax for dynamic data: `{{variableName}}`
- For arrays, use: `{{#each arrayName}} {{this.field}} {{/each}}`

## Support

For SendGrid-specific help:
- [SendGrid Documentation](https://docs.sendgrid.com)
- [Dynamic Templates Guide](https://docs.sendgrid.com/ui/sending-email/how-to-send-an-email-with-dynamic-templates)
