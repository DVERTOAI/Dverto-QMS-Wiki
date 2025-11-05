# Email Implementation Summary

This document provides a comprehensive overview of all email notifications implemented across the QMS system modules.

## Email Sending Modules

### 1. Incident Management Module
**Service:** `IncidentEmailService`
**Location:** `app/Services/Tenant/Incident/IncidentEmailService.php`

**Email Notifications:**
- **Incident Assigned** - Notifies assigned users, department level users, department head, and QMS users
- **Incident Created** - Notifies QMS users and reporter
- **Incident Accepted** - Notifies QMS users and reporter
- **Incident Declined** - Notifies QMS users and reporter with decline reason
- **RCA Submitted** - Notifies QMS users and reporter when Root Cause Analysis is submitted
- **Incident Reassigned** - Notifies QMS users, reporter, and new department users
- **Incident Postponed** - Notifies QMS users about postponement
- **Incident Closed** - Notifies QMS users and reporter

**Email Templates:**
- `emails.incidents.incident-assigned`
- `emails.incidents.incident-assigned-qms`
- `emails.incidents.incident-created`
- `emails.incidents.incident-accepted`
- `emails.incidents.incident-declined`
- `emails.incidents.rca-submitted`
- `emails.incidents.incident-reassigned`
- `emails.incidents.incident-postponed`
- `emails.incidents.incident-closed`

**Recipients:** QMS users, assigned users, reporters, department heads

### 2. External Incident Service
**Service:** `ExternalIncidentService`
**Location:** `app/Services/Tenant/Incident/ExternalIncidentService.php`

**Email Notifications:**
- **External Incident Created** - Notifies QMS users when external incidents are reported

**Email Templates:**
- `emails.incidents.incident-created`

**Recipients:** QMS users

### 3. Observation Management Module
**Service:** `ObservationEmailService`
**Location:** `app/Services/Tenant/Observation/ObservationEmailService.php`

**Email Notifications:**
- **Observation Created** - Notifies QMS users and admins
- **Observation Assigned** - Notifies assigned users, department level users, department head, and QMS users
- **Observation Accepted** - Notifies QMS users and observer
- **Observation Declined** - Notifies QMS users and observer with decline reason
- **RCA Submitted** - Notifies QMS users and observer when Root Cause Analysis is submitted
- **Observation Reassigned** - Notifies QMS users, observer, and new department users
- **Observation Postponed** - Notifies QMS users about postponement
- **Observation Closed** - Notifies QMS users and observer
- **Escalation Notification** - Notifies escalated level users and department head

**Email Templates:**
- `emails.observations.observation-created`
- `emails.observations.observation-assigned`
- `emails.observations.observation-assigned-qms`
- `emails.observations.observation-accepted`
- `emails.observations.observation-declined`
- `emails.observations.rca-submitted`
- `emails.observations.observation-reassigned`
- `emails.observations.observation-postponed`
- `emails.observations.observation-closed`
- `emails.observations.due-date-reminder`

**Recipients:** QMS users, assigned users, observers, department heads, escalated level users

### 4. Certificate Management (SRC Management) Module
**Service:** `CertificateService`
**Location:** `app/Services/Tenant/SrcManagement/CertificateService.php`

**Email Notifications:**
- **Certificate Created** - Notifies department head when new certificate is added
- **Certificate Renewal Acknowledgement** - Notifies department head when certificate is renewed

**Email Templates:**
- `emails.tenant.src-management.certificate-created`
- `emails.tenant.src-management.certificate-renewal-acknowledgement`

**Recipients:** Department heads, users with certificate permissions

### 5. Certificate Expiry Reminder System
**Service:** `SendCertificateExpiryReminderCommand`
**Location:** `app/Console/Commands/SendCertificateExpiryReminderCommand.php`

**Email Notifications:**
- **Initial Alert Reminders** - Sent when certificate reaches alert_days before expiry
- **Frequency Reminders** - Sent at regular intervals after initial alert
- **Expired Certificate Notifications** - Sent when certificate expires

**Email Templates:**
- `CertificateExpiryReminderMail`
- `CertificateFrequencyReminderMail`
- `CertificateExpiredMail`

**Recipients:** Department heads, admin users, QMS users

### 6. Indicator/KPI Management Module
**Service:** `IndicatorService`
**Location:** `app/Services/Tenant/Indicator/IndicatorService.php`

**Email Notifications:**
- **Indicator Assigned** - Notifies when indicator is assigned to department
- **KPI Submission Reminder** - Reminds department head about KPI submission

**Email Templates:**
- `IndicatorAssignedMail`
- `KpiSubmissionReminderMail`

**Recipients:** Assigned users, department heads

### 7. Committee Management Module
**Services:** 
- `CommiteeMeetingService` - `app/Services/Tenant/Committee/CommiteeMeetingService.php`
- `CommitteeMeetingAcceptanceService` - `app/Services/Tenant/Committee/CommitteeMeetingAcceptanceService.php`
- `CommitteeService` - `app/Services/Tenant/Committee/CommitteeService.php`

**Email Notifications:**
- **Committee Role Assignment** - Notifies users when assigned to committee roles (Chairman, Secretary, Member)
- **Meeting Scheduled** - Notifies committee members when meeting is scheduled
- **Meeting Rescheduled** - Notifies committee members when meeting is rescheduled
- **Meeting Cancelled** - Notifies committee members when meeting is cancelled
- **Meeting Acceptance Confirmation** - Sends calendar invite when meeting is accepted

**Email Templates:**
- `CommitteeRoleAssigned`
- `CommitteeMeetingScheduled`
- `CommitteeMeetingRescheduled`
- `CommitteeMeetingCancelled`
- `CommitteeMeetingAccepted`

**Recipients:** Committee chairmen, secretaries, members

## Email Configuration

### Mail Classes Location
All mail classes are located in `app/Mail/` directory:
- `app/Mail/IncidentEmail.php`
- `app/Mail/ObservationEmail.php`
- `app/Mail/CommitteeEmail.php`
- `app/Mail/CommitteeMeetingAccepted.php`
- `app/Mail/FeedbackFormEmail.php`
- `app/Mail/Indicator/IndicatorAssignedMail.php`
- `app/Mail/Indicator/KpiSubmissionReminderMail.php`
- `app/Mail/Tenant/SrcManagement/CertificateCreatedMail.php`
- `app/Mail/Tenant/SrcManagement/CertificateRenewalAcknowledgementMail.php`
- `app/Mail/Tenant/SrcManagement/CertificateExpiryReminderMail.php`
- `app/Mail/Tenant/SrcManagement/CertificateExpiredMail.php`
- `app/Mail/Tenant/SrcManagement/CertificateFrequencyReminderMail.php`

### Email Templates Location
Email templates are located in `resources/views/emails/` directory:
- `resources/views/emails/incidents/`
- `resources/views/emails/observations/`
- `resources/views/emails/tenant/src-management/`

### Email Configuration Files
- `config/mail.php` - Main mail configuration
- `config/mailSubjects.php` - Email subject configurations

## Email Delivery Methods

### 1. Immediate Sending
```php
Mail::to($recipients)->send($mailable);
```

### 2. Queued Sending
```php
Mail::to($recipients)->queue($mailable);
```

### 3. Via MailService
```php
$mailService->send($mailable, $recipients);
$mailService->queue($mailable, $recipients);
```

## Email Features

### 1. Multi-Recipient Support
- Primary recipients (TO)
- CC recipients
- Dynamic recipient lists based on roles

### 2. Template Variables
- Dynamic subject lines with placeholders
- Context-specific data passed to templates
- User personalization

### 3. Attachment Support
- File attachments for relevant notifications
- Document linking

### 4. Role-Based Recipients
- QMS users
- Admin users
- Department heads
- Assigned users
- Observers/Reporters

## Email Logging

### 1. Email Log Service
**Service:** `EmailLogService`
**Location:** `app/Services/Tenant/AuditLogs/EmailLogService.php`

### 2. Email Listener
**Listener:** `LogSentEmails`
**Location:** `app/Listeners/LogSentEmails.php`

## SMS Integration

All email notifications now have corresponding SMS notifications:
- SMS is sent automatically alongside emails
- Users must have valid mobile numbers
- SMS can be enabled/disabled via configuration
- SMS logging for audit trails

## Configuration Requirements

### Environment Variables
```env
MAIL_MAILER=smtp
MAIL_HOST=your-smtp-host
MAIL_PORT=587
MAIL_USERNAME=your-email
MAIL_PASSWORD=your-password
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=noreply@yourcompany.com
MAIL_FROM_NAME="QMS System"

# SMS Configuration
SEND_OTP=true
SMS_API_URL=your-sms-api-url
SMS_USERNAME=your-sms-username
SMS_PASSWORD=your-sms-password
```

### Queue Configuration
For better performance, configure queues for email processing:
```env
QUEUE_CONNECTION=database
```

## Email Subject Configuration

Email subjects are configured in `config/mailSubjects.php` with placeholders:
- `{incident_no}` - Incident number
- `{observation_no}` - Observation number
- `{certificate_title}` - Certificate title
