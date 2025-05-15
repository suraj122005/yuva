# yuva
from flask import Flask, render_template, request
import requests
import os

app = Flask(_name_)

API_KEY = 'your_openweathermap_api_key'

@app.route("/", methods=["GET", "POST"])
def index():
    weather = {}
    if request.method == "POST":
        city = request.form.get("city")
        url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={API_KEY}&units=metric"
        response = requests.get(url)
        if response.status_code == 200:
            data = response.json()
            weather = {
                "city": city,
                "temperature": data["main"]["temp"],
                "description": data["weather"][0]["description"],
                "humidity": data["main"]["humidity"],
                "wind": data["wind"]["speed"],
                "icon": data["weather"][0]["icon"]
            }
    return render_template("index.html", weather=weather)
