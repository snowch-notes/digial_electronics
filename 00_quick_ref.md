# Latches & Flip-Flops Quick Reference

## Key Differences
- **Latches**: Level-triggered (transparent when enable is active)
- **Flip-Flops**: Edge-triggered (change only on clock edges)

---

## LATCHES (Level-Triggered)

### SR Latch (Set-Reset)
**Inputs:** S (Set), R (Reset), EN (Enable)  
**Outputs:** Q, Q̄

| EN | S | R | Q(next) | Q̄(next) | State |
|----|---|---|---------|----------|-------|
| 0  | X | X | Q(prev) | Q̄(prev) | Hold |
| 1  | 0 | 0 | Q(prev) | Q̄(prev) | Hold |
| 1  | 0 | 1 | 0       | 1        | Reset |
| 1  | 1 | 0 | 1       | 0        | Set |
| 1  | 1 | 1 | **Invalid** | **Invalid** | **Forbidden** |

**Key Points:**
- S=R=1 is forbidden (creates race condition)
- When EN=0, output holds previous state
- When EN=1, behaves like basic SR latch

### D Latch (Data/Transparent Latch)
**Inputs:** D (Data), EN (Enable)  
**Outputs:** Q, Q̄

| EN | D | Q(next) | Q̄(next) | State |
|----|---|---------|----------|-------|
| 0  | X | Q(prev) | Q̄(prev) | Hold |
| 1  | 0 | 0       | 1        | Reset |
| 1  | 1 | 1       | 0        | Set |

**Key Points:**
- Q follows D when EN=1 (transparent)
- No invalid states (improvement over SR)
- Most common latch type

---

## FLIP-FLOPS (Edge-Triggered)

### SR Flip-Flop
**Inputs:** S (Set), R (Reset), CLK (Clock)  
**Outputs:** Q, Q̄

| CLK | S | R | Q(next) | Q̄(next) | State |
|-----|---|---|---------|----------|-------|
| ↑   | 0 | 0 | Q(prev) | Q̄(prev) | Hold |
| ↑   | 0 | 1 | 0       | 1        | Reset |
| ↑   | 1 | 0 | 1       | 0        | Set |
| ↑   | 1 | 1 | **Invalid** | **Invalid** | **Forbidden** |
| -   | X | X | Q(prev) | Q̄(prev) | Hold |

### D Flip-Flop (Data Flip-Flop)
**Inputs:** D (Data), CLK (Clock)  
**Outputs:** Q, Q̄

| CLK | D | Q(next) | Q̄(next) | State |
|-----|---|---------|----------|-------|
| ↑   | 0 | 0       | 1        | Store 0 |
| ↑   | 1 | 1       | 0        | Store 1 |
| -   | X | Q(prev) | Q̄(prev) | Hold |

**Key Points:**
- Q = D on next clock edge
- Most predictable and widely used
- No forbidden states

### JK Flip-Flop
**Inputs:** J, K, CLK (Clock)  
**Outputs:** Q, Q̄

| CLK | J | K | Q(next) | Q̄(next) | State |
|-----|---|---|---------|----------|-------|
| ↑   | 0 | 0 | Q(prev) | Q̄(prev) | Hold |
| ↑   | 0 | 1 | 0       | 1        | Reset |
| ↑   | 1 | 0 | 1       | 0        | Set |
| ↑   | 1 | 1 | Q̄(prev) | Q(prev)  | **Toggle** |
| -   | X | X | Q(prev) | Q̄(prev) | Hold |

**Key Points:**
- Eliminates forbidden state of SR
- J=K=1 causes toggle (very useful!)
- More complex but more versatile

### T Flip-Flop (Toggle Flip-Flop)
**Inputs:** T (Toggle), CLK (Clock)  
**Outputs:** Q, Q̄

| CLK | T | Q(next) | Q̄(next) | State |
|-----|---|---------|----------|-------|
| ↑   | 0 | Q(prev) | Q̄(prev) | Hold |
| ↑   | 1 | Q̄(prev) | Q(prev)  | Toggle |
| -   | X | Q(prev) | Q̄(prev) | Hold |

**Key Points:**
- Simplest flip-flop for counting
- T=1 always toggles output
- Can be made from JK (tie J and K together)

---

## SYMBOLS LEGEND
- **↑** = Rising edge (0→1 transition)
- **↓** = Falling edge (1→0 transition) 
- **-** = No edge / steady state
- **X** = Don't care
- **Q(prev)** = Previous state of Q
- **Q̄(prev)** = Previous state of Q̄

---

## TIMING CONSTRAINTS

### Setup and Hold Times
- **Setup Time (tsu)**: Input must be stable BEFORE clock edge
- **Hold Time (th)**: Input must remain stable AFTER clock edge
- **Propagation Delay (tpd)**: Time from clock edge to output change

### Typical Values (approximate)
| Parameter | TTL | CMOS |
|-----------|-----|------|
| Setup Time | 20ns | 5ns |
| Hold Time | 5ns | 2ns |
| Prop. Delay | 15ns | 10ns |

---

## COMMON APPLICATIONS

### By Type
- **D Flip-Flop**: Registers, shift registers, state machines
- **JK Flip-Flop**: Counters, frequency dividers, state machines  
- **T Flip-Flop**: Binary counters, frequency division
- **SR Flip-Flop**: Set/reset control, basic memory

### Design Tips
1. Use D flip-flops for most applications (simplest, most predictable)
2. Use JK when you need toggle functionality
3. Use T for simple binary counting
4. Avoid SR flip-flops unless you specifically need set/reset control
5. Always check setup/hold timing in real designs
