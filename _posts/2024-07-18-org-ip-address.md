---
layout: single
title: Find Salesforce Org IP Address at Run time
date: 2024-07-18
author: ehsan
tags: [Salesforce]
---

There used to be a Heroku app that could return your Org IP address and other details. However, the app is currently down, and even if it were operational, I wouldn't recommend using it in a Salesforce application.

```bash
HttpRequest req = new HttpRequest();
req.setEndPoint('https://salesforcelwc.herokuapp.com/myip');
req.setMethod('GET');
HttpResponse res = new HTTP().send(req);
```

But we can build our own IP Address app inside the Org.

## Apex Controller

Create a controller to set a `public` property `ipAddress`. The class constructor will try to retrieve the IP address from the HTTP headers.

```java
public class ViewIPAddressController {
	public String ipAddress { get; set; }

	public ViewIPAddressController() {
		ipAddress = ApexPages.currentPage().getHeaders().get('True-Client-IP');

		// this will work if no caching is in place or user is logged in via secure URL
		if (String.isBlank(ipAddress)) {
			ipAddress = ApexPages.currentPage()
				.getHeaders()
				.get('X-Salesforce-SIP');
		}

		// this logic will execute if proxy is in use
		if (String.isBlank(ipAddress)) {
			ipAddress = ApexPages.currentPage()
				.getHeaders()
				.get('X-Forwarded-For');
		}
	}
}
```

- Header key `True-Client-IP` is often set by content delivery networks (CDNs) or web application firewalls to indicate the original client IP address:
- Header `X-Salesforce-SIP` is used when the user is logged in via a secure URL or when no caching is in place.
- Header `X-Forwarded-For` is commonly used to identify the originating IP address of a client connecting to a web server through an HTTP proxy or load balancer.

## VisualForce Page

A Visualforce page is required to send the current page's PageReference to the controller.

```html
<apex:page controller="ViewIPAddressController" showHeader="false">
  <ipAddress>{!ipAddress}</ipAddress>
</apex:page>
```

## Apex usage

Finally, when we need to get the Salesforce Org IP address, we can execute this snippet:

```java
String ipAddress = (new PageReference('/apex/ViewIPAddress'))
	.getContent()
	.toString()from
	.substringBetween('<ipAddress>', '</ipAddress>');
```
