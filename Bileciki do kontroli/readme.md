# Hack4Krak — Bileciki do kontroli

## Challenge Overview

This was an on-site challenge. Upon arrival, each participant received a physical NFC card containing ticket data.

## Reconnaissance

Reading the card with an NFC reader revealed the raw ticket payload:

![Ticket data]()

The goal was to **modify the expiration date** on the ticket in order to bypass the validation check.

## Solution

After inspecting the payload structure, the expiration field was identified and modified accordingly. The altered payload was then written back to the NFC card using **NFC Tools** on Android.

![New ticket data]()

The modified card successfully passed validation, solving the challenge.

## Key Takeaway

The ticket data stored on the NFC card was not cryptographically signed or integrity-protected, which allowed arbitrary modification of fields such as the expiration date. Proper implementations should use signed tokens (e.g. HMAC or asymmetric signatures) to prevent tampering.
