# Pathgather Data Engineering Challenge

## Problem
Pathgather is an enterprise talent development platform that helps employees advance their careers by learning new skills. In the enterprise learning space today, employees can find learning content from a wide variety of both public and private providers, including public websites such as Coursera, Udacity, Lynda, and Pluralsight, but also company-specific private systems known as "Learning Management Systems" (LMS). With thousands of resources in different formats spread across all these systems, it's difficult for employees to find what they need and keep track of what they're learning.

Pathgather integrates with a large number of the most popular enterprise learning systems to provide a unified platform for employees to learn. Via our web & mobile apps, employees engage with learning resources on the Pathgather platform in a variety of ways, all of which are tracked as "events" and recorded in our data warehouse. Examples of events include performing a search, viewing a resource, launching a resource on the external page, etc.

By tracking these events for all users and all resources, Pathgather can analyze user behavior to determine which resources are the most popular, and create personalized recommendations for users that leverage both their activity as well as the activity of their co-workers.

## Challenge
**Your challenge is to process a sample dataset of Pathgather "events" and build a simple "recommender" algorithm that suggests a few learning resources given a specific user ID.**

We don't expect this to be an incredibly sophisticated system and will not focus much on the "quality" of the recommendations themselves - we're much more interested in how you tackle the problem, test your solution, and design your system to be incrementally improved over time.

Use whatever language and tools you prefer to tackle these requirements:
1. Download the sample dataset from http://pathgather-storage.s3.amazonaws.com/data-challenge/Pathgather_Team_Learning_Events_Aug_26_2017.csv.tar.gz
2. Import of the dataset and process it as needed
3. Build a basic recommendation algorithm that:
  * Takes a `user_id` as input
  * Returns an array of `target_id`s that represent the recommended resources
  * The array of recommendations should be sorted by relevance

These are meant to be flexible, so use your best judgment to design a solution that'll work in the time alloted. Specifically, it's worth mentioning a few "anti"-requirements:
1. No specific requirement for the number of recommendations returned (1, 2, 100...)
2. No specific requirement for how relevance is represented - or that it is returned at all - as long as the results are ordered
3. No specific requirement to allow multiple imports / training / streaming updates; a one-time process is fine
4. No specific requirement to persist the processed data to a database of any kind
5. No specific performance or timing requirements at all: on the amount of time it takes to process the dataset, return recommendation results, etc.

Example pseudocode:
```ruby
import_data "./Pathgather_Team_Learning_Events_Aug_26_2017.csv"
get_recommendations "2b79dd35-cd95-421e-a3c0-52bd0e9d2396"
# => [ "3890624d-69bc-4f8c-9016-d8d28edfe126", "eabec1e7-b0c0-4739-a64d-8c1bb5f82c7b", "c23db441-1be7-4772-8faa-9392741bf95a" ]
```

## Expectations
We don't expect you to finish this challenge; set a timelimit for whatever time you can spare (no longer than three hours, please) and tackle the problem like you would for building a proof-of-concept solution, so we're *definitely* not expecting production-quality code. However, we'll have you walk us through your solution on use our debrief call to discuss how you'd take this to the next stage, so we do expect to be able to understand what you've done! In other words, we definitely prefer *code quality* over *code quantity*.

TL;DR:
* Write good, clean code, but not production quality! We'll have you walk through the code in your debrief call
* Use whatever language and tools you prefer
* Spend no more than 3 hours on this challenge - we know your time is valuable!
* We don't care (much) about the quality of the recommendations, so don't over-complicate your model
* Use the Internet! It's not cheating to Google stuff (but do cite your sources for any code you directly copy)
* Assume requirements will change over time and we'll want to improve the model later :)

## Dataset
You can download the dataset here: [Pathgather_Team_Learning_Events_Aug_26_2017.csv](http://pathgather-storage.s3.amazonaws.com/data-challenge/Pathgather_Team_Learning_Events_Aug_26_2017.csv.tar.gz)

The dataset is a CSV file, pulled from a small instance of Pathgather used internally by our team so
there is only ~35,000 events. Each event is a single row from our data warehouse, and is structured
as follows:

column|description
-|-
created_at|timestamp of event creation
user_id|UUID of the user who performed the action
event_type|type of event (e.g. 'view_learnable')
target_table|specific target resource type (e.g. 'paths' or 'public_content')
target_id|UUID of the target resource

created_at|user_id|event_type|target_table|target_id
-|-|-|-|-
2017-08-26T14:01:13.231Z|bc45fe62-8064-4965-9e6b-b975cab1a08c|view_learnable|paths|31ae894c-f13a-48eb-ad33-cdd7f539b555
2017-08-25T21:06:51.839Z|bc45fe62-8064-4965-9e6b-b975cab1a08c|view_learnable|paths|31ae894c-f13a-48eb-ad33-cdd7f539b555
2017-08-25T20:43:11.778Z|2b79dd35-cd95-421e-a3c0-52bd0e9d2396|view_learnable|company_content|3bb3f861-7c49-4d1f-a430-3b0bb1463b1f
2017-08-25T20:42:56.374Z|2b79dd35-cd95-421e-a3c0-52bd0e9d2396|view_learnable|company_content|3bb3f861-7c49-4d1f-a430-3b0bb1463b1f
2017-08-25T20:35:07.823Z|f2572d33-ee48-4e33-8810-20dd10cded47|view_learnable|paths|ecb40d70-4c4b-4c2c-913a-432daa85d372
2017-08-25T20:19:56.335Z|bc45fe62-8064-4965-9e6b-b975cab1a08c|view_learnable|paths|f3c8ad2f-20fc-4198-ba30-824db8d6bb2d
2017-08-25T20:15:18.311Z|3252c387-4faf-4d22-af66-de7baa27c253|view_learnable|paths|f130b78e-a302-43fb-9560-b36d33dae9de
2017-08-25T20:03:04.741Z|bc45fe62-8064-4965-9e6b-b975cab1a08c|view_learnable|company_content|4b32e642-02c2-4021-85eb-df2e9bb32954
2017-08-25T19:45:19.851Z|3252c387-4faf-4d22-af66-de7baa27c253|view_learnable|company_content|2b311e7f-6e0c-4f7a-ab70-72a5a54392aa
2017-08-25T19:33:52.379Z|3252c387-4faf-4d22-af66-de7baa27c253|view_learnable|paths|c64e8fad-3b19-41a5-b1f5-43d50ce28368

## Event Types
event_type | description
-|-
view_learnable | user views the detail page for a specific resource
launch_learnable | user launches the link to a specific resource, which opens in a new tab
complete_learnable | user completes a specific resource, which is added to their profile history
queue_learnable | user queues a specific resource, saving it to quickly find again later
create_learnable | user creates a new resource, by sharing a link to an external provider
edit_learnable | user edits their shared resource
endorse_learnable | user endorses a specific resource, indicating that it was high quality
recommend_learnable | user recommends a specific resource to another user
comment_learnable | user leaves a comment on a specific resource
