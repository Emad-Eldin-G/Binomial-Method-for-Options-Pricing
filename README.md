# Backward Induction Binomial Method for Options Pricing
> In progress

## Overview
An option is a contract giving the holder the right (but not the obligation) to buy (call) or sell (put) a specified quantity of the underlying asset at a predetermined price (the strike) on or before expiry.


This project explores the ***Cox, Ross and Rubinstein Binomial Method for options pricing***.   

$$
C_0 = e^{-rT} \ \mathbb{E}^*[C_T] = e^{-rT} \left[ p \ C_u + (1 - p) \ C_d \right]
$$

where  

$$
p = \frac{e^{r\Delta t} - d}{u - d}
$$

and  

- \( $$C_0$$ \): current option price  
- \( $$C_u$$ \): option value after an up move  
- \( $$C_d$$ \): option value after a down move  
- \( $$r$$ \): risk-free interest rate  
- \( $$T$$ \): total time to maturity  
- \( $$\Delta t \$$): length of one time step  
- \( $$u$$, $$d$$ \): up and down factors  
- \( $$p$$ \): risk-neutral probability


The idea behind the risk-neutral formula is that it aims to create a no-arbitrage market, where all assets are priced “correctly” relative to one another. This is important as any arbitrage (inconsistency in pricing) creates guaranteed profit opportunities that will be exploited, so the price must prevent that.  

It is important to note that the formula above is what we call a “one-step” model. For $$N$$ steps, we will perform “backward induction” to find our option price ($$C_0$$) by working backward from step $$N$$ to step 0. The following visualizes how backward induction looks like as a binomial sum formula.

$$
C_0=e^{-rT}\sum_{k=0}^{N}\binom{N}{k}\left(p^\ast\right)^k\left(1-p^\ast\right)^{N-k} C\left(S_0u^kd^{N-k}\right)!
$$

Although there is a direct formula to compute the option price, it has several drawbacks. Think of a 64-bit computer, the maximum unsigned integer it can hold $$2^{64}\ -\ 1$$ is 18,446,744,073,709,551,615, or $$1.84\ \times\ {10}^{19}$$. In the formula, we use binomial coefficients in $$\binom{N}{k}$$, where the numbers become astronomically large as $$N$$ increases. For example: $$\binom{500}{250} = 3\times{10}^{149}$$, which is a $$130$$ orders of magnitude larger than $$1.84\ \times\ {10}^{19}$$.  

Hence, it's computationally more efficient and more numerically stable to iteratively apply the “one-step” model at each step from $$N$$ to $$0$$, building backwards. This project will implement the backward induction binomial algorithm using Python Dynamic Programming, Python NumPy Vectorization, and C++.


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
