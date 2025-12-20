# Spa Booking & Reception System (n8n)
An n8n-based booking and receptionist system designed to manage availability, bookings, rescheduling, and cancellations across multiple therapists and calendars, while maintaining consistency between Google Calendar, a central database, and customer records.

Although built for a spa (with therapists as service providers), the system is service-agnostic and can be adapted for salons, clinics, studios, or any appointment-based business.

## Why This Was Built This Way
The initial approach was an agent-first solution, where an LLM handled reasoning and booking decisions directly. This quickly surfaced several challenges that required a more deterministic, workflow-driven design:
- Multiple calendars (one per therapist) made availability checks error-prone
- Consistency issues across:

    - Google Calendar

    - Booking database

    - Customer records

- Time validation

    - Preventing bookings in the past

    - Enforcing business hours

- Race conditions

    - Concurrent booking requests

    - Double bookings and conflicts

    - Unclear source of truth when updates failed midway

To address this, logic was moved into explicit n8n workflows with clear responsibilities, while the agent acts only as an orchestrator, not the authority.

## What This System Does
Core Features
- Availability checks across one or all therapists
- Time validation (past bookings, business hours)
- Booking creation with conflict prevention
- Customer history tracking
- Rescheduling with re-validation
- Cancellation with calendar + database sync
- Deterministic outputs for agent consumption
### Assumptions & Constraints
These assumptions simplify logic but can be changed if needed:
- No buffers between bookings
- Different services have different durations
- Therapists (service providers) have individual calendars
- One booking = one therapist
Built for a spa, but service providers can represent:
Doctors, Stylists, Instructors, Consultants

### System Architecture (High Level)

## Workflows
### 1. check_availability
Purpose:Check if a requested time slot is available for a given service and therapist (or across all therapists).
Inputs: date, time,service, service_duration, therapist_preference (optional)

Flow
1. Parse inputs

2. Calculate start_time and end_timestamp (Google Calendar format)

3. Validate:

    - Time is not in the past

    - Time is within business hours
    (Note: this is sometimes pre-validated by the agent)

4. Validate service provider exists
5. Retrieve therapist details (Google Calendar ID)
6. Check calendar availability
7. Format and return availability result

Notes
- Workflow timezone must match Google Calendar timezone
(can be configured in n8n workflow settings)

- If no therapist is provided, availability is checked across all therapists

### 2. get_all_availability

Purpose: Return all available time slots for a service on a given day.

Inputs:
date, service, service_duration,therapist_preference(optional)
Examples
“What times is John available on Thursday for a deep massage?”

“Show all available times tomorrow for any therapist”

Flow
1. Same initial validation and parsing as check_availability
2. Returns a list of available time slots for:

- A specific therapist, or
- All therapists

### 3. create_booking

Purpose: Create a confirmed booking after availability has been validated.

Inputs:customer_name, customer_email, customer_phone,service,service_duration, therapist_name, date, time
Flow
1. Parse inputs
2. Validate service
3. Retrieve therapist details
4. Create calendar event
5. Insert booking record
6. Retrieve customer record
7. If new → create customer
8. If existing → update:
- Visit count
- Last visit details

### 4. get_booking

Purpose: Retrieve upcoming bookings for enquiries, rescheduling, or cancellation.

Inputs:customer_phone,status (e.g. confirmed)
Flow
1. Retrieve bookings from database
2. Filter:
- Remove past bookings
- Remove cancelled bookings
3. Return an indexed, ordered list for selection by the user or agent

### 5. reschedule_booking:
Purpose: Reschedule an existing booking after validating the new time.
Inputs:customer_phone, selected_index, service_duration, new_date,new_time

Flow
1. Retrieve bookings via get_booking
2. Select booking by index
3. Recalculate start and end timestamps
4. Confirm new slot availability
5. Update Google Calendar event
6. Update booking record
7. Return formatted confirmation

### 6. cancel_booking:
Purpose:
Cancel an existing booking cleanly.

Inputs: customer_phone, selected_index

Flow
1. Retrieve bookings via get_booking
2. Select booking by index
3. Delete calendar event
4. Update booking record status
5. Return confirmation output

## Tech Stack & Scalability
** Current stack
- n8n – workflow orchestration
- Google Calendar – availability and bookings
- Google Sheets – lightweight database for bookings and customer records
This setup works well for fast iteration and small-to-medium operations.

** Scalability path
- Google Sheets can be replaced with a relational database (PostgreSQL) for stronger constraints, concurrency handling, and scale

- Can integrate with a standard CRM for customer lifecycle management

- A vector store can be added to scale the agent’s knowledge base (services, policies, FAQs)

The workflows are designed so storage and knowledge layers can be swapped without rewriting core logic.
Easy to extend with;
- Buffers
- Payments
- Class-based bookings
- Multi-location support