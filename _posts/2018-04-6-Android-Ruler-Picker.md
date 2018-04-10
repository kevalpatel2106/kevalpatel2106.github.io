---
layout: post
section-type: post
title: Android Ruler Picker
category: Open Source
redirect: https://github.com/kevalpatel2106/Open-Weather-API-Wrapper
---

Open Weather API Wrapper is an Android wrapper for the APIs of [Open Weather Map](https://openweathermap.org). This library handles all the network operations and parameter validations on behalf of the developer.

## Dependency:
- Add below lines to `app/build.gradle` file of your project.
<pre>
	<code class="java">
dependencies {
    compile 'com.kevalpatel2106:open-weather-wrapper:1.0'
}
</code>
</pre>

## How to use this library?:

#### **Initialization:**

- Initialize the library in your launch activity by providing the open weather api key and the unit system you want to use throughout the application.
- If you don't have the open weather api key, you can generate it from [here](http://openweathermap.org/appid).
<pre>
	<code class="java">
   	OpenWeatherApi.initialize("YOUR API KEY", Unit.STANDARD);
	</code>
</pre>

#### **Accessing the API:**

Open weather api provides you functions to access below information:
- _Get current weather for provided city, geo point or postal code._
- _Get three hourly forecast for city or, geo point._
- _Get the daily forecast for provided city or geo point._

You can get the required information by passing the required parameters. The information will be received in specific listeners.

Here is the example of getting the three hourly forecast of the weather by city name.
<pre>
	<code class="java">
	OpenWeatherApi.getThreeHoursForecast("Landon,uk", new ForecastListener() {
    	@Override
    	public void onResponse(WeatherForecast weatherForecasts) {
        	//Forecast received.
        	//Do someting
    	}

    	@Override
    	public void onError(String message) {
        	//Something went wrong.
        	//Display the error message to the user.
    	}
	});
	</code>
</pre>

Open Weather API Wrapper uses [RxJava](https://github.com/ReactiveX/RxJava) and [Retrofit](https://square.github.io/retrofit/) to handle the network operations.

## Demo:
- You can download the sample application from [here](https://github.com/kevalpatel2106/Open-Weather-API-Wrapper/releases/download/1.0/sample.apk).