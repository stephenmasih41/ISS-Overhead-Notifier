# 🚀 ISS Overhead Notifier

This project was created on **Day 33 of my Python Bootcamp** 🐍.
It’s my **first time working with APIs in Python** 🎉.

The program checks two things:

1. If the **International Space Station (ISS)** 🛰️ is passing **right above your location**.
2. If it’s currently **dark outside** 🌑.

If both conditions are true, it sends you an **email notification** 📩 to remind you to look up and see the ISS! 🌌

---

## ✨ Features

* Uses **APIs** to get real-time ISS position and sunrise/sunset data.
* Automatically checks every 60 seconds ⏱️.
* Sends an email alert 📧 when conditions are met.
* Great beginner project for learning **API calls**, **JSON parsing**, and **automation** in Python.

---

## 🛠️ How the Code Works

### 🔹 Imports

```python
import requests
from datetime import datetime
import smtplib
import time
```

* `requests` → To make API calls 🌍.
* `datetime` → To check current time ⏰.
* `smtplib` → To send emails securely 📧.
* `time` → To pause the program between checks ⏱️.

---

### 🔹 Your Location & Email Credentials

```python
MY_LAT = 44.389355  # Latitude 📍
MY_LONG = -79.690331  # Longitude 📍
my_email = "youremail@gmail.com"
password = "yourpassword"
```

* Replace these with your **own latitude/longitude** 🌍.
* Use your **email + app password** for sending alerts.

---

### 🔹 Check if ISS is Overhead

```python
def iss_overhead():
    response = requests.get(url="http://api.open-notify.org/iss-now.json")
    data = response.json()
    iss_latitude = float(data["iss_position"]["latitude"])
    iss_longitude = float(data["iss_position"]["longitude"])

    if MY_LAT - 5 <= iss_latitude <= MY_LAT + 5 and MY_LONG - 5 <= iss_longitude <= MY_LONG + 5:
        return True
```

* Calls the **ISS API** 🛰️.
* Gets the **current position of the ISS**.
* Checks if the ISS is within **±5 degrees** of your location.

---

### 🔹 Check if It’s Dark Outside

```python
parameters = {"lat": MY_LAT, "lng": MY_LONG, "formatted": 0}

def is_dark():
    response = requests.get("https://api.sunrise-sunset.org/json", params=parameters)
    data = response.json()
    sunrise = int(data["results"]["sunrise"].split("T")[1].split(":")[0])
    sunset = int(data["results"]["sunset"].split("T")[1].split(":")[0])
    time_now = datetime.now().hour

    if time_now >= sunset or time_now <= sunrise:
        return True
```

* Calls the **Sunrise-Sunset API** 🌅.
* Extracts **sunrise & sunset hours**.
* Checks if the current time is **after sunset or before sunrise** (dark).

---

### 🔹 Main Loop

```python
while True:
    time.sleep(60)
    if iss_overhead() and is_dark():
        with smtplib.SMTP("smtp.gmail.com", port=587) as connection:
            connection.starttls()
            connection.login(user=my_email, password=password)
            connection.sendmail(
                from_addr=my_email,
                to_addrs="receiver_email@yahoo.com",
                msg="Subject:Look UP\n\nThe ISS is above you in the sky.")
```

* Runs forever 🔄.
* Waits **60 seconds** between checks.
* If ISS is overhead AND it’s dark → sends an **email alert** 📧.

---

## 🎉 What I Learned

* My **first experience with APIs** in Python 🌍.
* How to **fetch data from APIs** and parse JSON 📝.
* How to **send automated emails** using `smtplib` 📧.
* Scheduling tasks with `time.sleep()` ⏱️.

---
