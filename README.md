# Managed agents

AgentMail gives agents email inboxes. Build a **Managed Agents** section in the AgentMail console where customers can configure agents that respond to incoming email, without building and hosting a separate service for each one.

## The problem

Customers have different jobs for agents to do. Today, getting one running involves more than describing the task: someone needs to connect its inputs and tools, run it, retain its state, and make its work understandable.

We want a shared product and backend that handles those responsibilities so a customer can create an agent by configuring its behavior and connecting it to an AgentMail inbox. **AgentMail is the only connector for this trial:** use its API for inbox and message access, and its incoming-email events to trigger agents.

## Who is the customer?

Build for someone configuring and operating email agents for their team or customers. They know the job they want done and need a product that handles the setup and execution. Users supply their own instructions and context; the builder should support different email-agent behaviors without application-code changes for each use case.

Rho, an AgentMail customer whose team wants to create agents for its customers, is one example of this need. The platform should work across customers and use cases.

## What we'd like you to build

Build this as a **Managed Agents** destination in the sidebar of an existing console. Assume the user is already logged in, has API credentials, and has an AgentMail workspace with inboxes. Use the supplied console shell; focus on the agent builder and its backend.

Someone should be able to **create an agent, define its behavior, connect an inbox, and inspect the work triggered by incoming email**.

Keep agent configuration to three inputs:

- **Name:** a label that helps the user find the agent later.
- **Instructions:** one Markdown document describing the task and any useful context. Support writing or pasting Markdown; a plain text editor is sufficient. Importing an `.md` file is optional.
- **Inbox:** one existing AgentMail inbox whose incoming emails trigger the agent.

Treat the model and available AgentMail tool capabilities as fixed for the trial. Make the capabilities understandable; provider selection, model settings, document management, and a tool-permissions editor are outside scope.

Through the frontend, the user should be able to:

- Create and save an agent with those inputs.
- Find saved agents, reopen their configuration, and see their linked inbox and whether they are ready to receive email events.
- Open the latest run and inspect its input, status, output, tool activity, and any execution error.
- Refresh or return later without losing saved configuration or results.

Incoming AgentMail `message.received` events should trigger the appropriate agent through the execution harness without a UI Run action.

Latest-run inspection is sufficient; a run-history browser is not required.

Demonstrate the builder with different user-defined instructions through the same creation, event-triggered execution, and inspection flow. We do not provide preset agent configurations.

Use AgentMail's existing [API and webhook service](https://docs.agentmail.to/webhooks-overview). You own the API integration, webhook registration, receiving/parsing/validating events, and routing an event to the appropriate agent. Use the existing SDK or direct API calls; you do not need to build a webhook delivery service or a general API simulator.

Create synthetic webhook payloads that match the documented contract and a local delivery/replay script targeting your backend intake. Replays must exercise the same event-handling path as received webhooks. Local replay is sufficient for testing the receiver without a public tunnel; live HTTP webhook delivery requires a reachable URL. An event should start a run without someone pressing Run in the UI.

## Scope

Build a working Next.js frontend and backend with application data persisted in Postgres. You own the configuration experience, schema, APIs, validation, and execution integration.

Execution within the local event-handler request is acceptable. Decide how to handle failures and repeated events within the available time, and explain the limitations of your approach.

Keep the infrastructure bounded: one existing workspace, AgentMail inboxes and email events, and the supplied execution harness. AgentMail is the only connector. Other connectors, a connector marketplace, arbitrary-code hosting, a visual workflow builder, authentication, and cloud deployment are outside scope. Use the harness's local action-recording mode when testing generated email actions.

## Questions to consider

- How will a first-time user understand what the agent can do and when it runs?
- What should they see first when inspecting a result or failure?
- Can different instructions work through the same product and data model?
- What happens if an event arrives twice or execution fails?

## Working together

Start by exploring the existing AgentMail API and sketching the builder's user flow and technical approach. We'll discuss them together, then you will spend most of the session building.

Use us as teammates: ask questions, challenge assumptions, and tell us when something blocks you. Before the end, we'll have someone try the product so you can observe it and make an adjustment.

At the end, show the working experience and walk through the decisions behind it. Keep brief [design notes](DECISIONS.md) and the commands needed to run and check your work. You may use AI tools, libraries, and existing components; be ready to explain and modify what you build.

## Development environment

Use Next.js and TypeScript, Postgres, and the supplied local execution harness. We provide the console shell with an assumed logged-in workspace, AgentMail API credentials, database access, model access, and harness documentation. You build the AgentMail integration and local event-testing tools. You choose the application architecture.

See [setup](docs/SETUP.md) and [the harness contract](docs/HARNESS.md) for their preparation status. Keep credentials on the server and out of source control.
