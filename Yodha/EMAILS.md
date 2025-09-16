# Yodha Website - Send Mail API Documentation

## Endpoint
```
POST http://127.0.0.1:8000/api/send-mail
```

## Description
This API is used to send an email from the Yodha website.  
The `data` field contains any key-value pairs submitted by the user and will be rendered dynamically in the email.

## Payload

```json
{
    "to": "hn3121147@gmail.com",
    "subject": "Order Details",
    "data": {
        "any_key_1": "any_value_1",
        "any_key_2": "any_value_2",
        "any_key_3": "any_value_3"
    }
}
```

## Payload Fields

| Field   | Type   | Description                                      |
|---------|--------|--------------------------------------------------|
| to      | string | Recipient email address                         |
| subject | string | Subject of the email                             |
| data    | object | Arbitrary key-value pairs submitted by user. Can contain any custom keys and values as needed. |

## Example Request

```bash
curl -X POST http://127.0.0.1:8000/api/send-mail \
-H "Content-Type: application/json" \
-d '{
    "to": "hn3121147@gmail.com",
    "subject": "Order Details",
    "data": {
        "name": "Rajesh Kumar",
        "phone_no": "+91 9876543210",
        "email": "rajesh.kumar@example.in",
        "query": "I would like to inquire about your digital services and pricing."
    }
}'

# Or with completely different data:

curl -X POST http://127.0.0.1:8000/api/send-mail \
-H "Content-Type: application/json" \
-d '{
    "to": "hn3121147@gmail.com",
    "subject": "Custom Data",
    "data": {
        "product_id": "12345",
        "description": "Interested in product features",
        "budget": "5000 INR"
    }
}'
```

## Response

```json
{
    "success": true,
    "message": "Email sent successfully"
}
```

## Notes
- The `data` object is dynamic: any key-value pair can be used.
- Ensure your mail service (e.g., MailHog or real SMTP) is configured in `.env` file.
- The backend will process the `data` object and render all key-value pairs in the email content.

