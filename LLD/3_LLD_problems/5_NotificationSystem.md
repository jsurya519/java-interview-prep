# Notification System - Low Level Design Problem Statement

## Overview

Design a notification system that allows an application to send notifications to users through multiple communication channels such as Email, SMS, and Push Notifications.

The system should be extensible so that new notification channels can be added in the future with minimal code changes.

---

## Functional Requirements

### 1. Send Notification

The system should be able to send a notification message to a user.

Example:

* Payment Successful
* Order Delivered
* Welcome to the Platform

---

### 2. Support Multiple Notification Channels

Initially, the system should support:

* Email
* SMS
* Push Notification

Each channel has its own implementation for delivering messages.

The design should allow adding future channels such as:

* WhatsApp
* Slack
* Microsoft Teams

without modifying existing channel implementations.

---

### 3. User Notification Preferences

Each user can configure the channels through which they wish to receive notifications.

Examples:

User A:

* EMAIL
* PUSH

User B:

* SMS

The system should send notifications only through the channels enabled by the user.

---

### 4. Bulk Notifications

The system should support sending the same notification to multiple users.

Example:

"Festival Sale Started"

can be sent to a list of users in a single operation.

---

### 5. Channel-Specific Delivery

Different channels may use different user attributes:

* Email Channel → Email Address
* SMS Channel → Phone Number
* Push Channel → Device Identifier

The channel implementation should be responsible for using the appropriate user information.

---

## Non-Functional Requirements

### Extensibility

The system should support introducing new notification channels without changing existing business logic.

### Maintainability

Notification delivery logic should be encapsulated within channel implementations.

### Separation of Concerns

The notification manager should coordinate notification delivery while individual channels handle the actual sending mechanism.

### Testability

Each channel implementation should be independently testable.

---

## Out of Scope (Version 1)

The following features are intentionally excluded from this version:

* Retry Mechanism
* Notification Scheduling
* Notification Templates
* Delivery Status Tracking
* Notification History
* Priority Notifications
* Rate Limiting
* Message Queues (Kafka, RabbitMQ, etc.)
* Audit Logging

These can be considered future enhancements.

---

## Sample Scenario

User Preferences:

* EMAIL
* PUSH

Notification:

"Your payment was successful."

Expected Behavior:

* Send Email Notification
* Send Push Notification
* Do Not Send SMS Notification

because SMS is not enabled in the user's preferences.

---

## Expected Design Components

* User
* NotificationMessage
* Channel (Interface)
* EmailChannel
* SmsChannel
* PushChannel
* ChannelType
* NotificationManager

The NotificationManager should determine which channels are enabled for a user and delegate notification delivery to the corresponding channel implementations.



```java
package lldproblems;

import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

enum ChannelType
{
    SMS, EMAIL, PUSH;
}

interface Channel
{
    ChannelType getChannelType();
    void send(NotificationMessage message, User user);
}

class NotificationMessage
{
    String content;
}

class SmsChannel implements Channel
{
    @Override
    public ChannelType getChannelType() {
        return ChannelType.SMS;
    }

    @Override
    public void send(NotificationMessage message, User user) {

        // Logic to send sms using sms-service provider;
    }
}

class EmailChannel implements Channel
{

    @Override
    public ChannelType getChannelType() {
        return ChannelType.EMAIL;
    }

    @Override
    public void send(NotificationMessage message, User user) {
        // Logic to send sms using Email-service provider;
    }
}

class PushChannel implements Channel
{

    @Override
    public ChannelType getChannelType() {
        return ChannelType.PUSH;
    }

    @Override
    public void send(NotificationMessage message, User user) {
        // Logic to send sms using push-service provider;
    }
}



class User
{
    private String userId;
    private String name;
    private String mailAddress;
    private String phone;
    private String deviceId;
    private Set<ChannelType> preferences;

    public User(String userId, String name, String mailAddress, String phone, String deviceId) {
        this.userId = userId;
        this.name = name;
        this.mailAddress = mailAddress;
        this.phone = phone;
        this.deviceId = deviceId;
        preferences = new HashSet<>();
    }

    public Set<ChannelType> getPreferences() {
        return preferences;
    }

    void addPreference(ChannelType type)
    {
        preferences.add(type);
    }

    void removePreference(ChannelType type)
    {
        preferences.remove(type);
    }

    public String getUserId() {
        return userId;
    }

    public String getName() {
        return name;
    }

    public String getMailAddress() {
        return mailAddress;
    }

    public String getPhone() {
        return phone;
    }

    public String getDeviceId() {
        return deviceId;
    }
}




public class NotificationManager {

    private Map<ChannelType, Channel> channels;

    void addChannel(Channel channel)
    {
       //logic;
    }

    void removeChannel(Channel channel)
    {

       //logic;
    }

    void sendNotification(String msg, User user)
    {
        // check valid user logic;
        // generate Notification message logic;
        // iterate over channels and compare with user-preference and send notification;
        // audit ;

        NotificationMessage message = generateNotificationMessage(msg);

        for(ChannelType type:user.getPreferences())
        {
            Channel channel = channels.get(type);
            if(channel != null)
                channel.send(message, user);
        }

    }

    void BulkNotification(String sms, List<User> users)
    {
        //iterate users and call sendNotification;
    }

    private NotificationMessage generateNotificationMessage(String msg)
    {
        //logic;
        return null;
    }
}

```