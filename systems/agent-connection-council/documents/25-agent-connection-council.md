<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a Self-Running AI Networking Engine

**Project Link:** [View Project](https://learn.nextwork.org/projects/7249e849-a950-423d-a7ae-dd6b54a86586)

**Author:** Roy Piring Jr  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/7249e849-a950-423d-a7ae-dd6b54a86586_iixc4skk)

## The Problem Worth Solving

### Building a self-running AI networking engine for Marcus

This project focused on designing a repeatable networking system that reduces manual follow-up work while preserving personalized communication. The goal was to create a workflow that could reliably capture conference contacts, enrich outreach efforts, and maintain human approval before any communication is sent.

The solution starts with QR-code-based contact capture and ends with approved email delivery. Cloudflare Turnstile protects the landing page from automated submissions, Supabase stores and manages contact data, and a 25-agent Connection Council operating inside Obsidian generates personalized follow-up drafts. Resend handles email delivery only after Marcus reviews and approves the content, ensuring automation remains under human control.

## Architecting for the Long Game

### Pre-build artifacts that survive the builder

Before implementation, I created architectural artifacts to document key design decisions and system behavior. Six Architecture Decision Records (ADRs) were developed to capture the reasoning behind selecting Cloudflare Pages, Supabase, Claude Max, static HTML, Obsidian, and Cloudflare Turnstile.

I also produced a Mermaid architecture diagram to map the complete workflow from QR-code interaction through email delivery. Documenting these decisions early created a reference point for future maintenance, troubleshooting, and platform changes. This approach helps preserve system knowledge and reduces dependency on individual builders or undocumented assumptions.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/7249e849-a950-423d-a7ae-dd6b54a86586_l1rh9swk)

### Decision rationale and reversal cost

ADR-002 documented the decision to use Supabase instead of Firebase. Supabase provided a PostgreSQL foundation, Row Level Security (RLS), and pg_net triggers that could initiate email delivery without requiring an additional webhook platform.

The decision carried a medium reversal cost because the underlying PostgreSQL data remains portable, while the RLS policies and pg_net automation are platform-specific. The tradeoff was intentional. Accepting a higher migration effort provided a simpler architecture by consolidating multiple responsibilities into a single managed service.

## Deploying a Bot-Protected Global Landing Page

### Getting Marcus's contacts onto the global CDN

The landing page infrastructure was designed to provide secure, globally available contact capture. The deployment process included publishing the codebase to GitHub, connecting the repository to Cloudflare Pages, enabling Cloudflare Turnstile protection, and creating a keep-alive Worker for database availability.

This design minimizes operational overhead while supporting automated deployments and distributed content delivery. Each component serves a focused purpose, making the environment easier to maintain and troubleshoot over time.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/7249e849-a950-423d-a7ae-dd6b54a86586_sb9olpwl)

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/7249e849-a950-423d-a7ae-dd6b54a86586_xapj1yxa)

### Three infrastructure components working in concert

The private GitHub repository acts as the system of record and automatically triggers deployments whenever updates are pushed to the main branch.

Cloudflare Pages hosts the application on Cloudflare’s global network with HTTPS enabled by default and automated redeployment workflows.

A Cloudflare Worker executes on a scheduled cron job and sends a daily request to Supabase. This prevents inactivity-related pauses on the free-tier database and helps maintain application readiness without manual intervention.

## Building the Mobile-First QR Landing Page

### One scan, zero friction, full attribution

The landing page was designed as a single-file HTML application tuned for mobile devices and styled with Tailwind CSS. The page includes Marcus’s professional branding, direct contact links, a GDPR-compliant contact form, and Cloudflare Turnstile protection.

Form submissions are sent directly to Supabase through its REST API. The implementation also captures UTM parameters embedded within QR-code links, allowing event attribution and campaign tracking without requiring additional analytics tooling. This creates a direct path from initial scan to structured data collection.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/7249e849-a950-423d-a7ae-dd6b54a86586_jhvebtpw)

### Accessibility built in from the start

Accessibility requirements were incorporated during implementation rather than treated as a post-build task. Text contrast ratios exceeded WCAG AA requirements, raising readability across devices and environments.

Interactive controls were designed with larger touch targets to support mobile usability. Form fields include proper labels, icons contain descriptive accessibility attributes, decorative graphics remain hidden from assistive technologies, and keyboard navigation is supported through visible focus indicators. These measures raise usability while supporting accessibility compliance goals.

## Provisioning the Compliant CRM with Auto-Send Trigger

### A database that acts the moment Marcus approves

The CRM layer was built on Supabase using a multi-tenant schema designed to support visitors, events, sends, and responses. Indexed tables speed query performance, while Row Level Security controls data access based on user role and ownership.

The architecture supports a review-and-approval workflow where email drafts remain pending until explicitly approved. Once approved, database automation handles delivery without requiring additional manual actions, reducing operational friction while maintaining oversight.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/7249e849-a950-423d-a7ae-dd6b54a86586_xsccg5r7)

### Row Level Security protecting visitor data

The visitors table uses Row Level Security to limit public access. Anonymous users can submit contact information through the public form but cannot retrieve, modify, or delete stored records.

This design protects personally identifiable information while still enabling public submissions. The separation between write access and read access helps reduce data exposure risk and aligns with security best practices for externally facing forms.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/7249e849-a950-423d-a7ae-dd6b54a86586_25upxctc)

### The pg_net trigger that closes the automation gap

A database trigger monitors status changes from pending to approved. When approval occurs, the trigger executes a function that retrieves the Resend API credential from Vault and initiates email delivery using pg_net.

The same workflow updates status tracking fields and records delivery timestamps. Consolidating approval and delivery logic inside the database reduces dependency on external orchestration services and simplifies the overall automation path.

## Configuring Legally-Compliant Email Delivery

### Resend, domain verification, and compliance by design

The email delivery layer was designed to support authenticated sending while maintaining compliance requirements for commercial communication. Domain verification establishes sender legitimacy and raises deliverability by aligning outbound email with verified infrastructure.

Template design also incorporates regulatory requirements and communication transparency standards. This ensures automated outreach remains operationally effective while respecting recipient rights and disclosure obligations.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/7249e849-a950-423d-a7ae-dd6b54a86586_jn5xy5nz)

### CAN-SPAM, GDPR, and EU AI Act Art 50 satisfied

Each email template includes a physical mailing address and an unsubscribe mechanism to satisfy CAN-SPAM requirements for commercial messages.

The solution also includes AI-generated content disclosure language indicating that messages were drafted with AI assistance and reviewed by a human before delivery. Combined with consent-based contact capture and opt-out functionality, these controls support GDPR considerations and transparency requirements referenced in EU AI Act Article 50.

## Scaffolding the 25-Agent Connection Council

### An Obsidian vault that sounds like Marcus, not a robot

The Connection Council was organized within an Obsidian vault that serves as the operational workspace for twenty-five specialized agents. The implementation included directory structures, configuration files, brand voice definitions, and agent prompt modules.

This structure provides consistency across generated outreach content while maintaining Marcus’s professional tone. Organizing agent behavior through documented prompts raises maintainability and makes future refinements easier to manage.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/7249e849-a950-423d-a7ae-dd6b54a86586_nfyehpud)

### Self-refining thresholds that keep templates sharp

Template performance is monitored using reply-rate thresholds. Templates falling below a 15% response rate after at least ten sends are flagged for review, while templates exceeding a 30% response rate become candidates for default usage.

The E5 Weekly Refinement process reviews these metrics every Sunday and proposes refinements for human evaluation. Importantly, templates are never modified automatically, preserving oversight and preventing uncontrolled content drift.

## Proving the Full Pipeline End-to-End

### Nine-point compliance and health verification

The final validation phase focused on testing the complete workflow from QR-code interaction through email delivery. This included event attribution testing, workflow verification, and compliance review activities.

The objective was to confirm that each component functioned correctly both independently and as part of the broader automation pipeline. End-to-end validation reduces deployment risk and provides confidence in operational readiness.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/7249e849-a950-423d-a7ae-dd6b54a86586_iixc4skk)

### From QR scan to delivered email, and the Day 7 second touch

The workflow begins when a contact scans a QR code and submits information through the protected landing page. Contact data is stored in Supabase, where agent-generated follow-up drafts are created and placed into a pending state.

After approval, database automation sends the email through Resend using authenticated delivery controls. If no response is received after seven days, the system identifies the contact through the stale_sends view and generates a second-touch draft that returns to the approval queue for review before delivery.

## Secret Mission: PWA Dashboard and Self-Healing Monitor

### Real-time approvals, offline history, and proactive health alerts

The administrative dashboard was designed as an installable Progressive Web App with magic-link authentication, approval workflows, performance summaries, and draft management capabilities. Users can review, edit, approve, or reject outreach drafts from a centralized interface.

Supabase Realtime provides live updates without page refreshes, while service-worker caching enables offline access to historical records. A separate Cloudflare Worker performs recurring health checks against Supabase, Resend, scheduled jobs, and pending draft thresholds. When issues are detected, automated notifications are sent through Resend, enabling proactive operational monitoring.

## Reflections and Takeaways

### Tools and concepts mastered

This project combined Cloudflare Pages, Cloudflare Workers, Supabase, Resend, Obsidian, and Cloudflare Turnstile into a unified networking platform. Through implementation, I gained experience with architectural planning, automated workflows, managed cloud services, and agent-based content generation.

Key concepts included ADR-driven architecture, real-time data synchronization, Progressive Web App design, service-worker caching, compliance-aware automation, and multi-agent workflow orchestration.

### Time and challenges

This project took approximately three hours to complete. The most challenging component was configuring Supabase Realtime to ensure dashboard updates appeared instantly without requiring user refresh actions.

I also spent time validating the health-monitoring Worker to ensure it could accurately verify system dependencies and identify operational issues. The project provided practical experience designing a scalable networking workflow that combines automation, human approval, compliance controls, and cloud-native services into a single operational platform.

---

*Built with [NextWork](https://learn.nextwork.org) - [View this project](https://learn.nextwork.org/projects/7249e849-a950-423d-a7ae-dd6b54a86586)*
