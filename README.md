# Always Organic

### Storefront engineering: client state and external service boundaries

[Lawi Mwaura](https://github.com/Lawi-Mwaura)

<p align="center"><img src="assets/always-organic.jpg" width="65%" alt="Always Organic homepage excerpt showing its typography and shopping links." /></p>

*Actual public homepage excerpt captured on 1 October 2026. Customer records and commercial rules are excluded.*

## Problem statement

A storefront combines temporary client state, server-derived data, and external operations. Each needs a clear lifecycle, validation boundary, and useful empty or failure state.

## Technologies used

TypeScript · Next.js · React · Tailwind CSS · Supabase · TanStack Query · Zod · Resend · Framer Motion

## Engineering scope

A Next.js and React storefront with product discovery, a cart interface, server-derived data, and service integrations. This public overview covers the software responsibilities rather than commercial operations.

**Technologies:** TypeScript, Next.js, React, Tailwind CSS, Supabase, TanStack Query, Zod, Resend, Framer Motion.

## System design

**Component architecture.** The boxes identify technologies and responsibilities; boundaries group the application runtime and managed backend. Relationships show dependencies and integration protocols, rather than a step-by-step processing flow.

```mermaid
C4Component
    title Always Organic - application architecture
    Container_Boundary(app, "Next.js application") {
        Component(web, "Storefront interface", "React + Tailwind CSS", "Discovery and client cart state")
        Component(query, "Server-data client", "TanStack Query", "Fetch, cache and refresh")
        Component(api, "Integration routes", "Next.js + Zod", "Runtime payload validation")
    }
    System_Boundary(supabase, "Supabase backend platform") {
        ContainerDb(db, "Supabase Database", "PostgreSQL + data API", "Server-derived application records")
    }
    System_Ext(email, "Resend", "Notification service")
    Rel(web, query, "Uses", "React hooks")
    Rel(query, db, "Queries", "Supabase SDK / HTTPS")
    Rel(web, api, "Calls", "HTTPS")
    Rel(api, email, "Integrates", "Server-side API")
    UpdateElementStyle(web, $bgColor="#24486B", $fontColor="#FFFFFF", $borderColor="#24486B")
    UpdateElementStyle(query, $bgColor="#24486B", $fontColor="#FFFFFF", $borderColor="#24486B")
    UpdateElementStyle(api, $bgColor="#24745C", $fontColor="#FFFFFF", $borderColor="#24745C")
    UpdateElementStyle(db, $bgColor="#966F20", $fontColor="#FFFFFF", $borderColor="#966F20")
    UpdateElementStyle(email, $bgColor="#7653A1", $fontColor="#FFFFFF", $borderColor="#7653A1")
    UpdateLayoutConfig($c4ShapeInRow="2", $c4BoundaryInRow="1")
```

*Simplified responsibility map. It omits commercial rules, provider identifiers, private schema, and customer data.*

| Concern | Source evidence |
| :--- | :--- |
| Client versus server state | The application uses React for interface state and TanStack Query for server-data workflows. |
| Runtime input validation | A server notification route validates the shape of the request with Zod before processing it. |
| External services | Integrations are implemented through server routes rather than represented only as visual controls. |
| Empty-state behavior | Existing cart component tests cover the empty-cart message and a return-to-shop link. |
| Presentation | Shared header, footer, product-card, and homepage components separate presentation responsibilities. |

## Challenges and tradeoffs

**A cart is client state, but a completed external operation requires authoritative confirmation.** Interface state and server records should have distinct lifecycles. This distinction is the starting point for testing repeated actions, refreshed sessions, and stale cached data.

**Integration results need an explicit contract.** Input validation prevents malformed payload shapes from entering a service adapter. It does not, by itself, establish authorization, delivery, or an end-to-end successful operation. Those concerns need their own checks and consistent result handling.

**Empty and unavailable states are part of the interface.** A component should explain what the user can do when there is no data. The existing cart tests are a small example of this; they are not a complete integration suite.

## Outcomes

- Interface state and server-data queries have separate responsibilities.
- A reviewed server route validates payload structure before processing.
- Existing cart tests document empty-state copy and a return-to-shop action.

These are implementation outcomes supported by the reviewed source, not measured production improvements.

## Metrics and evidence

| Measure | Evidence |
| :--- | :--- |
| Interface evidence | Sanitized public web excerpt captured on 1 October 2026. |
| Verification scope | Source and existing component coverage reviewed; tests and service integrations were not executed. |
| Production metrics | No verified conversion, traffic, order, or latency figures supplied. |

## Validation scope

This showcase update reviewed the application structure, selected route logic, and existing component-test coverage, and captured the public interface. The cart tests and live service integrations were not executed during this documentation update. Next verification priorities are client-state restoration, query invalidation, validation failures, and external-service error contracts.

Source remains private. This repository contains only interface captures and an engineering overview; commercial logic is omitted.

[Contact Lawi](mailto:lawimwaura@gmail.com)
