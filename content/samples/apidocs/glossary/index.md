---
title: Glossary
date: 2026-01-06
bread: false
toc2: true
---
Learn more about the terminology frequently used in Armadillo.

# F

### Fare

An estimated cost of the Trip. A Fare can exist without a submitted Trip and can expire if not accepted.

Possible Fare statuses and their mapping to PSP transaction statuses:

| Fare status      | Description                                                                                                                                                                                                                         | Mapped PSP statuses                                                                                                                        |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `created`        | Fare is calculated and stored in the system.                                                                                                                                                                                        | n/a                                                                                                                                        |
| `rejected`       | Fare estimate has been rejected by the Passenger.                                                                                                                                                                                   | n/a                                                                                                                                        |
| `initiated`      | Fare estimate is sent to PSP for capturing.                                                                                                                                                                                         | “00: initiated”; “01: captured”; “03: processing”                                                                                      |
| `released`       | Previously authorized transaction associated with the Fare record has been released by PSP.                                                                                                                                         | “02: released”                                                                                                                             |
| `processed`      | Transaction associated with the fare record has been processed by PSP.<br><br>If `final_amount` = “0” due to the applied coupon, the fare receives the “processed” `fare_status` without sending anything to the PSP.               | “04: success”                                                                                                                              |
| `partial_refund` | A fare has been partially refunded.<br><br>If a fare has at least one “processed” refund and `refundable_total` is >0 and < `final_amount`, the `fare_status` is “partial_refund” with the `status_msg` = “20: partially_refunded”. | “20: partially_refunded”                                                                                                                   |
| `refunded`       | A fare has been fully refunded.<br><br>If a fare has at least one “processed” refund and `refundable_total` is =0, the `fare_status` is “refunded” with the `status_msg` = “21: refunded”.                                          | “21: refunded”                                                                                                                             |
| `failed`         | Transaction processing failed.                                                                                                                                                                                                      | “10: failed_retrying”; “11: failed_blocked_by_bank”; “12: failed_no_funds”; “13: failed_canceled_by_client”; “14: failed_declined” |


# G

### Group

Association of users gathered by one or multiple characteristics for admin-level segmentation and targeted actions.


# I

### Idempotency key

A unique client-generated identifier that ensures repeated requests with the same intent get processed only once.


# P

### PSP

Payment Service Provider (PSP) is a third-party service responsible for payment processing: authorization, capture, refund, etc.


# R

### Refund

A transaction returning previously charged funds to the customer. Refunds can be partial and full.

Possible Refund statuses and their mapping to PSP transaction statuses:

| Refund status | Description                                                                   | Mapped PSP statuses                                                                                     |
| ------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `created`     | Refund transaction has been initiated.                                        | “00: initiated”; “01: captured”; “03: processing”                                                   |
| `processed`   | Refund transaction associated with the fare record has been processed by PSP. | “04: success”                                                                                           |
| `failed`      | Transaction processing failed.                                                | “10: failed_retrying”; “11: failed_blocked_by_bank”; “12: failed_no_funds”; “14: failed_declined” |
>[!Note] Refunds cannot be canceled from the client's side.


# S

### Serviceable area

The geographic region that Trips can be estimated and executed within.

### Status reference

An ID of an internal thread, automatic email, or support ticket that explains, backs up, or provides context on the entity status in question.


# T

### Trip

An entity that represents the Passenger's ride request and its execution lifecycle, from submission to successful closure or cancellation.


### Trip rating

A 1-5 star grade that a Passenger can leave to express how satisfied they are with the Trip. Trip rating can affect Driver rating. 

Unsatisfactory Trip rating triggers an automatic follow-up email asking the Passenger to elaborate on the rating they left. If the Passenger responds, the correspondence becomes a support ticket until the issue resolution.

A Trip rating may transition through the following statuses:

| Trip rating status | Description                       | Affects Driver rating? | Status reference |
| ------------------ | --------------------------------- | ---------------------- | ---------------- |
| `accepted`         | Default status.                   | yes                    | optional         |
| `disputed`         | Trip rating is being reviewed.    | no                     | required         |
| `nullified`        | Trip rating has been disregarded. | no                     | required         |

>[!Note] A `disputed` or `nullified` Trip rating may return to the `accepted` status depending on the review outcome. Status reference is required in this case.

