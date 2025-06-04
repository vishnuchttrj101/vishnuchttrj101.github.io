---
layout: post
title: "TALE OF CUROVMS - version - 2.0.1"
date: 2025-06-04 17:00:00 +0530
categories: blog
tags: [jekyll, tutorial]
---





## 1. CVE-2024–40582

Hey, I recently visited one of the posh buildings in CyberCity, and I noticed they were using a third-party visitor management system(Curo VMS), where you enter your phone number, details, and a Govt. ID proof first and then it generates a QR code. By scanning that QR code into the machines, you get access inside the building, just like Delhi Metro.
One day while i was checking my browser history, i noticed a url of the webcurovms having a parameter base64 encoded. It started scratching that part of my brain, is the data we giving this VMS secure, can anyone access those data??
Let's find out.
I started digging into it.
The url looked like this https://fakeurl.com/index.html?vms=<base64 encoded string>, the string used here is a base64 string, I decoded it and found that this was basically made of building number, databasename, current date and time.
First i tried to hit the same url but it resulted in error. Then i changed the date and time to match the current time and date and it worked. Now i am able to open the VMS interface and can now log in.

![My Photo](/assets/images/medium-write-up/1.png)



As my curiosity didn't stop over here, i tried a random number and moved ahead, to my surprise, the OTP it generated don't need to check it from your phone number, you can just fire you developer tool and see the OTP in response.

![My Photo](/assets/images/medium-write-up/2.png)



Even this didn't settled my curiosity, i generated a fake pass.


![My Photo](/assets/images/medium-write-up/3.png)



## The Journey Continues (CVE-2024–40583)


As i enumerated the portal further, i checked the request and the response, that is used to fetch the pass details like phone, number, username, company name etc.
The request itself contained the admin credentials of the API, with the parameters named as 'WebApiUser' and 'WebApiPassword'. It was a SOAP service, the strfunc takes the data in json like appID, building id, userid and password.
These parameters already contains, the data, i.e. apiuser and apipassword that was stored in a javascript file.


![My Photo](/assets/images/medium-write-up/4.png)



![My Photo](/assets/images/medium-write-up/5.png)




SOAP servicecredentials stored in clear textNow, the url that is used to fetch the visitor details contained a function named as GETVISITORDETAILS.
The parameter mobile no. can be tweaked and used to fetch the details of other users.
The response, contains visitor name, mobile no, address, image(contains User Govt. IDs).


![My Photo](/assets/images/medium-write-up/6.png)

I really cannot believe that how can any company, be so reckless with such sort of personal information.
These vulnerabilities can be exploited to impersonate someone.
Gather sensitive data, about an individual.
As soon as i found those, i reported it to the vendor immediately.
But, no response from them, tried mailing them 3 times but no response.
I thought of reporting it to MITRE, fortunately got a response from them and got 2 CVE IDs assigned.
I asked them how to proceed as vendor was not responding, i got a mail from MITRE stating to report this to KB Cert, i did the same, they added the vendor in the chat but they never joined. I am attaching the timelines of my contact trying to reach out to the vendors.
27th June - Mailed Pentaminds about this issue and asked how can i report (no response)
5 July - reported it to MITRE
9 July - Got CVE Id assigned
12 August - Reported it to KB cert, they added the vendor also to the chat, but vendor never responded.
I would like to request Pentaminds to patch this as soon as possible.


Thanks For Reading fellasss!!!!!!!!!!



Keep Hacking!!!!!!!!!!!!!!!!!!
