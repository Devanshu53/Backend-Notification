# Notification Service with RabbitMQ, Node.js, and MongoDB

This project implements a notification service with:

- ✅ **API service** to send notifications (email, SMS, in-app)
- 🔄 **Consumer service** to consume messages from RabbitMQ queue and send notifications
- 🗃️ **MongoDB** for storing notifications and user data
- 📬 **RabbitMQ** as message broker
- 📦 Dockerized setup
- ☁️ Ready for cloud deployment (Render, CloudAMQP)

---

## 🚀 Features

- Push notifications to queue via API
- Consumer retries failed notifications up to 3 times
- Email & SMS sending via third-party services (Nodemailer, Twilio)
- Notification status stored in MongoDB
- Dockerized for easy deployment
- Cloud-ready (Render + CloudAMQP)

---

## 🔄 System Flow

1. `Client` sends a request to `POST /notifications`
2. API pushes notification into RabbitMQ queue
3. Consumer listens to queue, processes the message
4. Notification is sent via email/SMS, and result is saved in MongoDB
5. You can fetch status using `GET /notifications/:userId`

---

## 🛠️ Tech Stack

- Node.js + Express
- MongoDB + Mongoose
- RabbitMQ (via `amqplib`)
- Nodemailer (Email)
- Twilio (SMS)
- Docker
- Render (Deployment)

---

# 📡 Notification Service API Documentation

### 🔗 Base URL

`https://backend-notification-cidq.onrender.com`

---

## 📌 1. Create User

### `POST /users`

Create a new user to whom notifications can be sent.

🔗 **Example URL:**  
`https://backend-notification-cidq.onrender.com/users/create-user`

---

### ✅ Request Body

```json
{
  "name": "Devanshu Pundir",
  "email": "devanshupundir599@gmail.com",
  "phone": "+916005804090"
}
```

### 📤 Response (201 Created)

```json
{
  "statusCode": 201,
  "data": {
    "name": "Devanshu Pundir",
    "email": "devanshupundir599@gmail.com",
    "phone": "+916005804090",
    "_id": "68297daabfe491bada2f2393",
    "createdAt": "2025-05-18T06:26:50.408Z",
    "updatedAt": "2025-05-18T06:26:50.408Z",
    "__v": 0
  },
  "message": "User created successfully",
  "success": true
}
```

## 📌 2. Send Notification

### `POST /notifications`

Send a notification (email, SMS, or in-app).

🔗 **Example URL:**  
`https://backend-notification-cidq.onrender.com/notifications`

---

### ✅ Request Body (JSON)

```json
{
  "userId": "68286a4e9a51b791bdfdfaec",
  "type": "email",
  "message": "email testing msg"
}
```

### Response (202 Accepted)

```json
{
  "statusCode": 202,
  "data": {
    "notificationId": "68297d7a955c6f007e6a3934",
    "userId": "68286a4e9a51b791bdfdfaec",
    "notificationType": "email",
    "queuedAt": "2025-05-18T06:26:02.690Z"
  },
  "message": "Notification has been accepted and queued for processing.",
  "success": true
}
```

🧾 Note:
If you select email or sms as the notification type, you will actually receive a real email or SMS on your configured address/number. However, if you select in-app, the message will just be stored in the database — no real notification will be sent.

## 📌 3. Get All Notifications for a User

### `GET /users/{id}/notifications`

Fetch all notifications sent to a specific user.

🔗 **Example URL:**  
`https://backend-notification-cidq.onrender.com/users/68286a4e9a51b791bdfdfaec/notifications`

---

### 📤 Response

```json
{
  "statusCode": 200,
  "data": [
    {
      "_id": "68297d7a955c6f007e6a3934",
      "userId": "68286a4e9a51b791bdfdfaec",
      "type": "in-app",
      "message": "email testing",
      "status": "sent",
      "createdAt": "2025-05-18T06:26:02.064Z",
      "updatedAt": "2025-05-18T06:26:02.826Z",
      "__v": 0
    },
    {
      "_id": "68297d5f955c6f007e6a3931",
      "userId": "68286a4e9a51b791bdfdfaec",
      "type": "email",
      "message": "email testing 999",
      "status": "sent",
      "createdAt": "2025-05-18T06:25:35.990Z",
      "updatedAt": "2025-05-18T06:25:39.331Z",
      "__v": 0
    },
    {
      "_id": "68297d48955c6f007e6a392e",
      "userId": "68286a4e9a51b791bdfdfaec",
      "type": "sms",
      "message": "sms testing 3",
      "status": "sent",
      "createdAt": "2025-05-18T06:25:12.761Z",
      "updatedAt": "2025-05-18T06:25:13.893Z",
      "__v": 0
    }
    // ...more items
  ],
  "message": "Success",
  "success": true
}
```

## 📌 4. Get Single Notifications status

### `GET /notifications/{notificationId}/status`

Returns the current status of a specific notification.

🔗 **Example URL:**  
`https://backend-notification-cidq.onrender.com/notifications/6828d59f33e30d6b9041c21d/status`

---

### 📤 Response

```json
{
  "statusCode": 200,
  "data": {
    "notificationId": "6828d59f33e30d6b9041c21d",
    "userId": "68286a4e9a51b791bdfdfaec",
    "type": "email",
    "message": "email testing buddy 2",
    "status": "sent",
    "createdAt": "2025-05-17T18:29:51.495Z",
    "updatedAt": "2025-05-17T18:29:54.275Z"
  },
  "message": "Notification status fetched successfully",
  "success": true
}
```
