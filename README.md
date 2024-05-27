# Solution-to-SQL-Murdery-Mystery 


A walkthrough of the solution to SQL Murder Mystery by Northwestern University Knight Lab. 
Step 1: Identify the Crime Scene
You vaguely remember that the crime was a murder that occurred sometime on Jan. 15, 2018 and that it took place in SQL City.


SELECT description 
FROM crime_scene_report
WHERE date = '20180115' AND type = 'murder' AND city = 'SQL City';


Response:
description
-----------------------------------------------------
Security footage shows that there were 2 witnesses. 
The first witness lives at the last house on "Northwestern Dr". 
The second witness, named Annabel, lives somewhere on "Franklin Ave".


Step 2: Investigate the Witnesses
The first witness lives at the last house on "Northwestern Dr". We need to find the address of this witness.


SELECT name, address_street_name 
FROM person 
WHERE address_street_name LIKE '%Northwestern Dr%' 
ORDER BY address_number DESC 
LIMIT 1;


Response:

name           | address_street_name
---------------|--------------------------
Morty Schapiro |  Northwestern Dr


The second witness, named Annabel, lives somewhere on "Franklin Ave".


SELECT name, address_street_name 
FROM person 
WHERE name like 'Annabel%'  AND address_street_name LIKE 'Franklin Ave%';

Response:

name	          |address_street_name
----------------|-------------------
Annabel Miller	|Franklin Ave



Step 3: (Find below alternative too)
Review Witness Interviews get there ids too 
With both witnesses identified, try to get additional clues.


SELECT *
FROM interview
WHERE person_id IN ("14887", "16371");


Transcripts from witnesses:

I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".

I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.

So, here's our clues:

Murderer is a man and a member of a gym called "Get Fit Now Gym" and has a gold membership no. starting from "48Z".
He was working out at the gym on 9th Jan.
He drives a car with car plate containing "H42W".

 
Check Gym Check-Ins using Clues


getting the gym check-ins:

SELECT *
FROM get_fit_now_check_in
WHERE membership_id LIKE '48Z%'
	AND check_in_date = "20180109";
We've gotten two members with membership_ids 48Z7A and 48Z55.

SQL Murder Mystery List 2 Suspects

Verify Gender and Car Plate Number
verify the gender and car plate information of potential suspects to gather more clues.

SELECT *
FROM drivers_license
WHERE gender = "male"
	AND plate_number LIKE '%H42W%';
We're getting closer! The owner of the car could either have license_id 423327 or 664760.

SQL Murder Mystery Car Plate Number

SELECT *
FROM person
WHERE license_id IN ("423327", "664760");
Now, let's check which of these two are a member of the gym.

SQL Murder Mystery Check Licesne Plate



SELECT *
FROM get_fit_now_member
WHERE person_id IN ("51739", "67318");

Check your solution
Did you find the killer?

INSERT INTO solution VALUES (1, 'Jeremy Bowers');
SELECT value FROM solution;


value
Congrats, you found the murderer! But wait, there's more... If you think you're up for a challenge, try querying the interview transcript of the murderer to find the real villain behind this crime. If you feel especially confident in your SQL skills, try to complete this final step with no more than 2 queries. Use this same INSERT statement with your new suspect to check your answer.





Step 3:
Here is how i'll decode in other way  😎🙃

I'm into SOCIALLY CONNECTING STUFF too🤪🤪🤪
I found this FACEBOOK in ERD . Let me solve from here ...

Check Facebook Event Check-ins
Considering the witnesses would likely have seen the murderer at some event, let's check the events on January 15, 2018.

SELECT p.name, f.event_name
FROM facebook_event_checkin AS f
JOIN person AS p ON f.person_id = p.id
WHERE f.date = '20180115';

Response:
name            | event_name
----------------|----------------------
Morty Schapiro  | The Funky Grooves Tour
Annabel Miller  | The Funky Grooves Tour
Jeremy Bowers   | The Funky Grooves Tour .... etc


Step 4: The Murderer
We found the same witnesses and a new individual, Jeremy Bowers, at the event "The Funky Grooves Tour." Jeremy Bowers was not previously mentioned, making him a new suspect. Given the context, it's highly likely that Jeremy Bowers is the murderer.


SELECT p.name, f.event_name
FROM facebook_event_checkin AS f
JOIN person AS p ON f.person_id = p.id
WHERE f.date = '20180115' AND f.event_name = 'The Funky Grooves Tour';


Response:
name            | event_name
----------------|----------------------
Morty Schapiro  | The Funky Grooves Tour
Annabel Miller  | The Funky Grooves Tour
Jeremy Bowers   | The Funky Grooves Tour


Conclusion
Jeremy Bowers is identified as the murderer based on the events attended by the witnesses and the new suspect's presence at the same event on the day of the murder.


Check your solution
Did you find the killer?

INSERT INTO solution VALUES (1, 'Jeremy Bowers');
SELECT value FROM solution;


value
Congrats, you found the murderer! But wait, there's more... If you think you're up for a challenge, try querying the interview transcript of the murderer to find the real villain behind this crime. If you feel especially confident in your SQL skills, try to complete this final step with no more than 2 queries. Use this same INSERT statement with your new suspect to check your answer.


Bye 😮‍💨...

