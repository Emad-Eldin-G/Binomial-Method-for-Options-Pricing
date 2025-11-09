# The Binomial Method for Options Pricing
> In progress

## Overview
Options are **financial contracts** that give buyers the "option" to buy or sell (long or short) an underlying asset (stock) for a **specified price at a certain time in the future**.  

Options derive their price from the underlying asset, but it's not that simple. There are several ways to price options, each way incorporating different parameters.  

This project explores the ***Binomial Method using the risk-neutal formula***, and will also demonstrate ***how the Binomial Method price converges to Black-Scholes*** as the number of steps in fixed physical time goes grows towards ∞.

```mermaid
graph LR
    A["S₀"]:::root

    A --> B["S₀u"]:::up
    A --> C["S₀d"]:::down

    B --> D["S₀u²"]:::up
    B --> E["S₀ud"]:::mid
    C --> E
    C --> F["S₀d²"]:::down

    D --> G["S₀u³"]:::up
    D --> H["S₀u²d"]:::mid
    E --> H
    E --> I["S₀ud²"]:::mid
    F --> I
    F --> J["S₀d³"]:::down

    classDef root fill:#fef3c7,stroke:#f59e0b,stroke-width:2px,color:#92400e;
    classDef up fill:#dbeafe,stroke:#3b82f6,stroke-width:1.5px,color:#1e3a8a;
    classDef down fill:#fee2e2,stroke:#ef4444,stroke-width:1.5px,color:#7f1d1d;
    classDef mid fill:#e0f2fe,stroke:#0284c7,stroke-width:1.5px,color:#075985;
