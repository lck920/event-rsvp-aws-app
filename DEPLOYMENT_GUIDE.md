# Deployment Guide

This guide covers the key steps needed to deploy the backend to AWS Lambda and host the frontend on S3 & CloudFront.

## Backend (Lambda)

### 1. Environment Variables
Set these **environment variables** in your Lambda configuration. Feel free to customize:

| Variable | Example | Description |
|---|---|---|
| `REGION` | `ap-southeast-1` | AWS region |
| `DB_HOST` | `mydb.xxxxx.ap-southeast-1.rds.amazonaws.com` | MySQL host |
| `DB_USER` | `admin` | MySQL user |
| `DB_PASS` | `********` | MySQL password |
| `DB_NAME` | `eventsdb` | MySQL database |

### 2. Install Dependencies
Before zipping your code for Lambda, install the Node.js packages:
```bash
npm install
```

## API Summary (API Gateway)

When setting up your API Gateway, map the following endpoints to your Lambda function:

| Method | Path | Purpose |
|---|---|---|
| `GET` | `/events` | Fetch all events |
| `GET` | `/event/{event_id}` | Fetch single event |
| `GET` | `/stats/{event_id}` | Get RSVP counts |
| `GET` | `/attendees/{event_id}` | Get attendee list |
| `POST` | `/rsvp` | Submit RSVP |

> **Note:** Make sure **CORS** is set up properly on your API Gateway so your frontend can communicate with it.

## Hosting (S3 + CloudFront)

1. **Upload** all frontend files (`index.html`, `.css`, `.js`) to your S3 bucket.
2. **Create a CloudFront distribution** pointing to that S3 bucket.
3. Set the **Default Root Object** to `index.html`.

---

## Making Frontend Changes

When you update your HTML, CSS, or JS and upload to S3, CloudFront might still serve the old cached files. Here's how to clear the cache:

### CloudFront Invalidation (Console)

1. Go to **CloudFront** → your distribution → **Invalidations** → **Create invalidation**
2. Enter:
   ```text
   /*
   ```
   *This clears the entire cache.*
3. Wait for the status to show **Completed**.
4. Go back to your site and **hard refresh**:
   - **Windows:** `Ctrl + F5`
   - **Mac:** `Cmd + Shift + R`
   - *(then refresh)*
