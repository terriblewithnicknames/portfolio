---
title: Armadillo Product overview
date: 2025-12-05
bread: false
toc: true
---
# What is Armadillo

Armadillo is a fictional taxi service operating in a futuristic yet down-to-earth setting of Sprawl City. Unlike its AI-driven competitors, Armadillo functions in a more classic way, offering human drivers and app-like user experience.

# Who is it for
Armadillo is for people who want to reduce the presence of AI and technology in their daily lives. Whether it is a simple lack of trust, nostalgia, craving for human connection, or personal trauma, Armadillo is there to move you from point A to point B in a reliable, human way. No ads, no subscription fees.

As of now, Armadillo is available to registered residents and visitors of Sprawl City only within the city administrative borders. Expansion into neighboring regions is one of the company's long-term strategic goals.

## User roles
Armadillo functionality is available to users depending on their role.

Users registered as Passengers can:
- book, rate, and cancel trips;
- select cabs of different tiers;
- use discount coupons;
- contact Armadillo customer support via email.

Users registered as Drivers can:
- receive, review and accept/decline available trips;
- navigate to the pickup and drop-off points;
- close successfully completed trips.

Admin users can:
- oversee the Armadillo operations;
- group passengers or drivers;
- issue and manage discount coupons;
- process trip ratings and passenger/driver complaints;
- issue partial or full refunds;
  etc.

# How it works
## High-level system diagram
Here's a high-level diagram of the Armadillo's system:

{{< figure
    src="Armadillo_system_diagram.png"
    alt="System diagram of Armadillo taxi service"
    width="100%"
    class="indoc-img"
>}}

Components:
- **Client** - allows Passengers, Drivers, and Admins to interact with the system.
- **API** - handles authentication, authorization, rate limits, routing, and communication between a client and microservices.
- **Microservices** - do legwork: process API requests, read from or write to corresponding DBs, execute business logic, trigger webhooks and internal notifications, etc. 
- **Server/DBs** - stores actual data about defined internal resources.
- **3rd party PSP** - processes payments (fares) and refunds.

## Core resources
The core resources of Armadillo are:
- **Cabs** - company car fleet. 
- **Passengers** - profiles of users registered in the system as passengers.
- **Drivers** - profiles of users registered in the system as drivers.
- **Groups** - associations of users gathered by one or multiple characteristics for admin-level segmentation and targeted actions.
- **Fares** - fare estimates and associated PSP transactions.
- **Trips** - passenger trips created in the system.
- **Coupons** - discount coupons issued for groups or individual passengers.
- **Refunds** - records about fare refunds and associated PSP transactions. 
- **Ratings** - passenger trip ratings.

## Key concepts
Some of the key concepts are:
- Trip lifecycle
- Fare estimation
- Route calculation
- Payment authorization and release
- Discount coupon issuance and management
- Partial and full refunds
- User segmentation and targeting

## User flow examples
### Trip lifecycle

Simplified:
{{< figure
    src="Armadillo_trip_flow_simplified_v2.png"
    alt="Diagram of a trip flow at Armadillo taxi service"
    width="100%"
    class="indoc-img"
>}}

Detailed:

0. A user downloads the Armadillo app and creates a passenger account.

1. While in the main app view, a passenger sets the pickup and drop-off points on the map. This sends a POST request to the backend.
2. `/fares` receives and processes the request by calculating the fare, adding it to the "Fares" table along with other related details, and passing the response back to API.
3. API returns the estimated fare to the passenger client, allowing the passenger to make further decisions.
4. A passenger either accepts the fare and confirms the trip submission, or declines it by closing the trip submission form.
	1. If a fare is declined, the flow ends here: `/fares` receives a POST request to change the `fare_status` to "rejected". The `rejected` fare record will be automatically deleted after its TTL runs out.
    
	2. If a fare is accepted, the flow continues: `/fares` updates the `fare_status` to "initiated" and sends the fare to the 3rd party PSP for capturing; `/trips` creates a new entry in the "Trips" and links it to the previously accepted fare. The newly created trip status is "submitted".
5. "Submitting" a trip triggers the cab-to-passenger matching flow. Submitted trips form a pool and get offered to cabs located closest to the passenger. Eligible drivers receive corresponding notifications in their clients.
6. A driver either accepts or declines the trip.
	1. If a trip is not accepted/declined, it stays in the pool, while the matching flow keeps offering it to other drivers, from closest to farthest. If a trip has timed out or got canceled by the passenger, it gets removed from the pool and receives a "canceled" status via `/trips`. Any linked fares get released via `/fares`. The flow ends here.

	2. If a trip is accepted, the flow continues: `/trips` adds "accepted" status to the trip and passes pickup point details to the driver. Involved passenger and driver receive corresponding notifications.
7. Once the cab has arrived at the pickup, the driver confirms it in the client. This sends an API request to `/trips` to add "at_pickup" status to the trip; the set status triggers a corresponding notification to the passenger.
8. Once the passenger is ready for departure, the driver starts the trip from their client. This sends an API request to `/trips` to add "started" status to the trip; the set status triggers a corresponding notification to the passenger.
9. Once the cab has arrived at the trip destination, the cab coordinates match the destination point. This sends an API request to `/trips` to add "at_drop" status to the trip; the set status triggers corresponding notifications to the passenger and the driver.
10. Once the trip has received the "at_drop" status, the passenger exits the cab; the driver confirms the successful trip completion from their client. This sends an API request to `/trips` to add "closed" status to the trip; the set status triggers corresponding notifications to the passenger and the driver.

### Trip rating

0. A passenger books and successfully finishes a trip.

1. A successfully finished trip receives the "closed" status. This triggers the passenger client to present the passenger with the trip rating pop-up.
	1. If the passenger doesn't want to rate the trip, they can either close the rating pop-up or let it get automatically dismissed after 30 min. Either way, the trip rating flow ends here.

	2. If the passenger wants to rate the trip, they leave the rating and confirm the action via the rating pop-up. This sends an API request to `/ratings` to create a new entry in the "Rating" table and return to the passenger client with the "thank you" message. The flow continues.

2. Further actions depend on the rating left:
	1. Unsatisfactory ratings (3 stars or less) trigger Customer Care notifications and flows. Passenger receives an automatic email survey asking them to elaborate on their rating and provide additional feedback. If they respond to it, the email thread gets forwarded to CS department for further processing.

	2. Satisfactory ratings (4-5) do not trigger additional actions by default.
