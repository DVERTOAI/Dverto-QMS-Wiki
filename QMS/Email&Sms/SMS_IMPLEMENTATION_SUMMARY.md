# SMS Implementation Summary

This document summarizes the SMS functionality that has been added to all modules where email notifications are sent.

## Modules with SMS Implementation

### 1. Incident Management
**Service:** `IncidentSmsService`
**Location:** `app/Services/Tenant/Incident/IncidentSmsService.php`

**SMS Methods:**
- `sendIncidentAssigned()` - Notifies assigned users and QMS users
- `sendIncidentCreated()` - Notifies QMS users and reporter
- `sendIncidentAccepted()` - Notifies QMS users and reporter
- `sendIncidentDeclined()` - Notifies QMS users and reporter
- `sendRCASubmitted()` - Notifies QMS users and reporter
- `sendIncidentClosed()` - Notifies QMS users and reporter
- `sendIncidentReassigned()` - Notifies QMS users and reporter about reassignment
- `sendIncidentPostponed()` - Notifies QMS users about postponement

**Integration:** SMS calls are enabled in `IncidentEmailService.php`

### 2. Observation Management
**Service:** `ObservationSmsService`
**Location:** `app/Services/Tenant/Observation/ObservationSmsService.php`

**SMS Methods:**
- `sendObservationAssigned()` - Notifies assigned users and QMS users
- `sendObservationCreated()` - Notifies QMS users and observer
- `sendObservationAccepted()` - Notifies QMS users and observer
- `sendObservationDeclined()` - Notifies QMS users and observer
- `sendRCASubmitted()` - Notifies QMS users and observer
- `sendObservationClosed()` - Notifies QMS users and observer
- `sendObservationReassigned()` - Notifies QMS users and observer about reassignment
- `sendObservationPostponed()` - Notifies QMS users about postponement
- `sendObservationEscalated()` - Notifies escalated level users

**Integration:** SMS calls are enabled in `ObservationEmailService.php`

### 3. Certificate Management (SRC Management)
**Service:** `CertificateSmsService`
**Location:** `app/Services/Tenant/SrcManagement/CertificateSmsService.php`

**SMS Methods:**
- `sendCertificateCreated()` - Notifies department head and permission users
- `sendCertificateRenewalAcknowledgement()` - Notifies about successful renewal
- `sendCertificateExpiryReminder()` - Notifies about upcoming expiry
- `sendCertificateExpired()` - Notifies about expired certificates

**Integration:** 
- SMS calls added to `CertificateService.php`
- SMS notifications added to `SendCertificateExpiryReminderCommand.php`

### 4. Indicator/KPI Management
**Service:** `IndicatorSmsService`
**Location:** `app/Services/Tenant/Indicator/IndicatorSmsService.php`

**SMS Methods:**
- `sendIndicatorAssigned()` - Notifies about indicator assignment
- `sendKpiSubmissionReminder()` - Reminds department head about KPI submission

**Integration:** SMS calls added to `IndicatorService.php`

### 5. Committee Management
**Service:** `CommitteeSmsService`
**Location:** `app/Services/Tenant/Committee/CommitteeSmsService.php`

**SMS Methods:**
- `sendCommitteeRoleAssigned()` - Notifies about role assignment
- `sendMeetingScheduled()` - Notifies about meeting scheduling
- `sendMeetingRescheduled()` - Notifies about meeting rescheduling
- `sendMeetingCancelled()` - Notifies about meeting cancellation

**Integration:** 
- SMS calls added to `CommiteeMeetingService.php` for all meeting notifications
- SMS notification added to `CommitteeMeetingAcceptanceService.php` for meeting acceptance confirmation

### 6. External Incident Service
**Service:** Uses `SmsNotificationService` directly
**Location:** `app/Services/Tenant/Incident/ExternalIncidentService.php`

**SMS Functionality:** Already implemented - sends SMS to QMS users when external incidents are created

## Core SMS Service
**Service:** `SmsNotificationService`
**Location:** `app/Services/SmsNotificationService.php`

**Features:**
- Configurable SMS API integration
- Debug mode support
- SMS logging with `SmsLog` model
- Mobile number validation
- Custom message and template ID support

## Configuration
**Config File:** `config/sms.php`

**Key Settings:**
- `enabled` - Enable/disable SMS sending
- `api` - SMS API configuration (URL, credentials, sender ID)
- `default_message` - Default SMS message template
- `templates` - SMS template IDs for different types

## How SMS is Triggered

1. **Email Services**: Each email service now calls corresponding SMS service methods
2. **Automatic**: SMS notifications are sent automatically when emails are sent
3. **Conditional**: SMS is only sent if user has a valid mobile number
4. **Configurable**: Can be enabled/disabled via config
5. **Logged**: All SMS attempts are logged for tracking

## Benefits

1. **Comprehensive Coverage**: SMS notifications added to all modules with email functionality
2. **Consistent Implementation**: All SMS services follow the same pattern
3. **Error Handling**: Proper exception handling and logging
4. **Flexible**: Can be enabled/disabled per module if needed
5. **Scalable**: Easy to add new SMS notifications following the established pattern

## Usage

SMS notifications will be sent automatically alongside email notifications. Users need to have valid mobile numbers in their profiles to receive SMS notifications.

To enable SMS:
1. Set `SEND_OTP=true` in `.env`
2. Configure SMS API credentials in `.env`
3. Ensure users have mobile numbers in their profiles