
# 🧠 n8n Projects Collection

A curated collection of my **n8n automation workflows** — each project lives in its own folder and explores a real-world use case such as appointment scheduling, data syncing, social automation, and AI-powered agents.

This repository is meant to:
- Showcase practical automation designs built on **n8n**.
- Serve as a **reference for workflow architecture**, subflow design, and error handling.
- Provide **reusable building blocks** for anyone working on similar automations.

---

## 🗂️ Repository Structure

Each folder represents one complete n8n project.
│
├── barber-booking-bot/ # AI booking assistant with scheduling + rescheduling flows
├── Meeting summaries and to-do lists with asana, gmail,slack integration
└── README.md # This file

Each project folder includes:
- `workflow.json` — exportable n8n workflow file  
- `README.md` — project-specific documentation (setup, purpose, demo flow)  
- Optional `subflows/` — reusable sub-workflows for modular design  
- Optional `assets/` — diagrams, screenshots, or sample data  

---

## 🧩 Prerequisites

To use any workflow in this repo:

1. Install [n8n](https://n8n.io) (locally or via Docker)
2. Import the project’s `.json` file into your n8n instance
3. Configure environment variables and credentials (see project README)
4. Activate the workflow

---

## 🧠 Design Principles

All projects follow a few consistent patterns:
- **Tool-based design:** Core logic split into small, reusable sub-workflows (“tools”)
- **Defensive automation:** Built-in safeguards against double requests, race conditions, and null inputs
- **Explainable logic:** Every complex step is documented and labeled for clarity
- **Human-in-the-loop:** When relevant, the agent asks for user confirmation before committing actions

---

## 🧱 Example Project: Barber Booking Bot

An AI-powered booking assistant that:
- Handles scheduling, rescheduling, and cancellations  
- Manages availability checks for multiple barbers  
- Uses modular subflows for actions like `get_booking`, `check_availability`, and `update_booking`  
- Includes prompt and tool documentation for both the main agent and its subflows

👉 See [barber-booking-bot/README.md](./barber-booking-bot/README.md) for detailed flow and setup.

---

## 🚀 Future Additions

- Slack or Discord automation templates  
- CRM data enrichment pipelines  
- AI content generation agents  
- Analytics and monitoring flows  

---

## 💬 Feedback & Contributions

This repository is continuously evolving.  
If you’d like to suggest improvements or report an issue:
- Open a GitHub issue  
- Or start a discussion on [n8n Community](https://community.n8n.io)

---

## 🪪 License

This repository is shared for **learning and demonstration purposes**.  
You’re free to fork and adapt workflows with attribution.

---

**Author:** [Ameenah Lawal](https://www.linkedin.com/in/aminatlawal21/)  
**Focus:** Data & Automation Engineering • AI Agents • Workflow Design


