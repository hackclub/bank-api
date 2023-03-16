# Hack Club Bank's Rails Models

Below is a list of relevant public facing models in the Hack Club Bank codebase.

A couple of notes:
- Organizations are called `event`s in the codebase
- `hcb_codes` are what Users know of as a transaction. It links to other
objects.
- There are many models that are not listed here (there is around 81 models
total). These are mostly non-public facing models used by the our Transaction
Engine. Some of those may contain additional information that could be helpful
to expose. If you find that a listed model does not contain information that's
already visible in the Bank website, please let me ([Gary](https://garytou.com))
know and I can provide additional details. In addition, some fields may be
omitted from the list below.


## ach_transfers

Outgoing payment via the ACH network (ACH Credit Origination).

- event_id `bigint`
- creator_id `bigint`
- routing_number `string`
- bank_name `string`
- recipient_name `string`
- amount `integer`
- approved_at `datetime`
- created_at `datetime`
- updated_at `datetime`
- recipient_tel `string`
- rejected_at `datetime`
- scheduled_arrival_date `datetime`
- payment_for `text`
- state `string`
  - pending, in_transit, rejected, failed, deposited


## bank_fees

A fee charged by Hack Club Bank on Organization's revenue.

- event_id `bigint`
- hcb_code `string`
- state `string`
  - pending, in_transit, settled
- amount_cents `integer`
- created_at `datetime`
- updated_at `datetime`
- fee_revenue_id `bigint`

## checks

Outgoing payment via mailed check.

- creator_id `bigint`
- lob_address_id `bigint`
- memo `text`
- check_number `integer`
- amount `integer`
- expected_delivery_date `datetime`
- send_date `datetime`
- transaction_memo `string`
- voided_at `datetime`
- approved_at `datetime`
- exported_at `datetime`
- refunded_at `datetime`
- created_at `datetime`
- updated_at `datetime`
- rejected_at `datetime`
- payment_for `text`
- state `string`
  - scheduled, in_transit, in_transit_and_processed, deposited, canceled, voided, refunded


## comments

Comments on various models.

- commentable_type `string`
  - such as donation, invoice, hcb_code, etc.
- commentable_id `bigint`
- user_id `bigint`
- created_at `datetime`
- updated_at `datetime`
- admin_only `boolean`


## disbursements

Internal transfers between two Organizations (`event`s).

- event_id `bigint`
- amount `integer`
- name `string`
- rejected_at `datetime`
- created_at `datetime`
- updated_at `datetime`
- source_event_id `bigint`
- errored_at `datetime`
- requested_by_id `bigint`
- state `string`
  - reviewing, pending, in_transit, deposited, rejected, errored
- pending_at `datetime`
- in_transit_at `datetime`
- deposited_at `datetime`


## documents

Organization specific documents such as Fiscal Sponsorship Confirmation letter.

- event_id `bigint`
- name `text`
- user_id `bigint`
  - the creator of the doc
- created_at `datetime`
- updated_at `datetime`
- slug `text`


## donations
- email `text`
- name `text`
- amount `integer`
- amount_received `integer`
- status `string`
- event_id `bigint`
- payout_id `bigint`
- created_at `datetime`
- updated_at `datetime`
- message `text`
- hcb_code `text`
- state `string`
  - pending, in_transit, deposited, failed, refunded
- recurring_donation_id `bigint`


## events

These are Organizations.

- name `text`
- start `datetime`
- end `datetime`
- address `text`
- sponsorship_fee `decimal`
- created_at `datetime`
- updated_at `datetime`
- slug `text`
- hidden_at `datetime`
- donation_page_enabled `boolean`
- donation_page_message `text`
- is_public `boolean`
- public_message `text`
- omit_stats `boolean`
  - whether an Organization is HQ-related
- country `integer`
- category `integer`
  - hack club, hackathon, robotics grant, hardware grant, etc.
- demo_mode `boolean`
- demo_mode_request_meeting_at `datetime`
- is_indexable `boolean`
  - Can the org be listed at https://bank.hackclub.com/api/v3/organizations
- deleted_at `datetime`


## hcb_codes

This is a pretty important model. It currently represents a "logical"
Transaction. HcbCodes are what users think of a transactions. It has the ability
to group multiple "physical" transactions together. HcbCode itself does not
store a lot of information. But from an HcbCode, we can get its "linked object"
such as a Donation, Invoice, ACH Transfer etc.

- hcb_code `text`
- created_at `datetime`
- updated_at `datetime`
- marked_no_or_lost_receipt_at `datetime`


## invoices
- sponsor_id `bigint`
- amount_due `bigint`
- amount_paid `bigint`
- memo `text`
- due_date `datetime`
- subtotal `bigint`
- total `bigint`
- item_description `text`
- item_amount `bigint`
- created_at `datetime`
- updated_at `datetime`
- hosted_invoice_url `text`
- invoice_pdf `text`
- creator_id `bigint`
- manually_marked_as_paid_at `datetime`
- manually_marked_as_paid_user_id `bigint`
- manually_marked_as_paid_reason `text`
- slug `text`
- fee_reimbursement_id `bigint`
- payment_method_type `text`
- payment_method_card_brand `text`
- payment_method_card_checks_address_line1_check `text`
- payment_method_card_checks_address_postal_code_check `text`
- payment_method_card_checks_cvc_check `text`
- payment_method_card_country `text`
- payment_method_card_exp_month `text`
- payment_method_card_exp_year `text`
- payment_method_card_funding `text`
- payment_method_card_last4 `text`
- payment_method_ach_credit_transfer_bank_name `text`
- payment_method_ach_credit_transfer_routing_number `text`
- payment_method_ach_credit_transfer_swift_code `text`
- archived_at `datetime`
- archived_by_id `bigint`
- hcb_code `text`
- state `string`
  - open, paid, void



## lob_addresses

The mailing address that a Check should be sent to.

- event_id `bigint`
- description `text`
- name `string`
- address1 `string`
- address2 `string`
- city `string`
- state `string`
- zip `string`
- country `string`
- created_at `datetime`
- updated_at `datetime`


## organizer_position_invites

Invites a User to join an Organization.

- event_id `bigint`
- user_id `bigint`
- sender_id `bigint`
- created_at `datetime`
- updated_at `datetime`
- accepted_at `datetime`
- rejected_at `datetime`
- organizer_position_id `bigint`
- cancelled_at `datetime`
- slug `string`


## organizer_positions

This is what gives an User access to an Organization. This gets created after a
User accepts an OrganizerPositionInvite.

- user_id `bigint`
- event_id `bigint`
- created_at `datetime`
- updated_at `datetime`
- deleted_at `datetime`


## receipts

Receipts uploaded by users for transactions.

- user_id `bigint`
- created_at `datetime`
- updated_at `datetime`
- receiptable_type `string`
  - hcb_code, etc.
- receiptable_id `bigint`


## recurring_donations

Regularly scheduled recurring donations.

- email `text`
- name `text`
- event_id `bigint`
- amount `integer`
- created_at `datetime`
- updated_at `datetime`
- canceled_at `datetime`


## sponsors

The recipient of an invoice.

- event_id `bigint`
- name `text`
- contact_email `text`
- address_line1 `text`
- address_line2 `text`
- address_city `text`
- address_state `text`
- address_postal_code `text`
- created_at `datetime`
- updated_at `datetime`
- slug `text`


## stripe_cards

Hack Club Bank cards

- event_id `bigint`
- stripe_brand `text`
- stripe_exp_month `integer`
- stripe_exp_year `integer`
- last4 `text`
- card_type `integer`
  - virtual or physical
- stripe_status `text`
  - frozen or active
- stripe_shipping_address_city `text`
- stripe_shipping_address_country `text`
- stripe_shipping_address_line1 `text`
- stripe_shipping_address_postal_code `text`
- stripe_shipping_address_line2 `text`
- stripe_shipping_address_state `text`
- stripe_shipping_name `text`
- created_at `datetime`
- updated_at `datetime`
- activated `boolean`
- name `string`


## tags

Transaction tags (beta feature)

- label `text`
- color `text`
- created_at `datetime`
- updated_at `datetime`
- event_id `bigint`


## user_sessions
- user_id `bigint`
- created_at `datetime`
- updated_at `datetime`
- timezone `string`
- deleted_at `datetime`
- webauthn_credential_id `bigint`
- expiration_at `datetime`


## users
- created_at `datetime`
- updated_at `datetime`
- email `text`
- full_name `string`
- phone_number `text`
- slug `string`
- phone_number_verified `boolean`
- webauthn_id `string`
- session_duration_seconds `integer`
- birthday `date`


## webauthn_credentials
- user_id `bigint`
- name `string`
- webauthn_id `string`
- public_key `string`
- sign_count `integer`
- created_at `datetime`
- updated_at `datetime`
- authenticator_type `integer`
