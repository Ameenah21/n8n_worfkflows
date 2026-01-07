# AI-Enhanced Meeting Notes Summarizer & Task Creator
An n8n workflow that turns raw meeting notes into approved summaries and actionable tasks, with a human-in-the-loop review step.

Trigger

Google Drive (new or updated meeting notes)

Flow

Extract key summaries and action items using AI

Send summary via Gmail for human review

On approval:

Email final summary to meeting attendees

Create tasks in Asana

On feedback:

Apply corrections

Resend for review
(repeat until approved)

This workflow ensures accuracy, accountability, and task follow-through, while keeping humans in control of final outputs.