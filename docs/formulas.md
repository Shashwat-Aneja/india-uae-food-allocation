# Price & Allocation Formulas

## Notation
- `P_ind` = base price in India (per tonne)
- `T_trans` = transport cost (origin → UAE) per tonne
- `S_storage` = storage cost per tonne
- `I_insurance` = insurance per tonne
- `H_handling` = handling & port fees per tonne
- `Tariffs` = import tariff per tonne (if any)
- `Margin` = required margin per tonne
- `TransitTime` = estimated days from origin to destination
- `DemandUrgency` ∈ [0,1] (1 = critical)
- `Surplus` = quantity available in region (tonnes)
- `TotalSurplus` = total surplus across India considered

## Landing Cost
LandingCost = P_ind + T_trans + S_storage + I_insurance + H_handling + Tariffs + Margin

## TransitTime Penalty
TransitPenalty = α * max(0, TransitTime - TargetTime)
(where α is a tunable penalty factor)

## Allocation Score (higher is better)
AllocScore = w1*(Surplus/TotalSurplus) + w2*DemandUrgency - w3*(TransitPenalty/MaxPenalty)
Weights w1,w2,w3 sum to 1 (e.g., 0.4, 0.4, 0.2)

## Selection Rule
Select allocations sorted by AllocScore until UAE demand is met or India surplus limit reached. Provide top-k routes by lowest LandingCost.

## Cost Comparison
Savings = GlobalMarketPrice - LandingCost
