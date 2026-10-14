<!-- solve-problem:e9afb792-06d1-41ac-b051-6a82b3c48a0f -->
# Sampling equitable colorings near q = Δ

A [Tangle Solve problem](https://solve.tangle.tools/problems/e9afb792-06d1-41ac-b051-6a82b3c48a0f).

## What is the problem?

Given a graph G = (V, E) with n vertices and maximum degree Δ, and an integer q, a **proper q-coloring** assigns colors {1,…,q} so adjacent vertices differ. An **equitable** (or balanced) coloring further prescribes the size of each color class — either exactly n_i vertices of color i for each i (with Σn_i = n), or each class size within one of the others (the standard equitable-coloring notion; the census suggests the fixed-size version, |n_i − n_j| ≤ 1 or exactly prescribed counts).

**Goal:** design an algorithm that, for any fixed ε > 0 and q ≥ (1+ε)Δ, outputs a sample from a distribution within total variation distance ≤ δ (in poly(n, 1/δ) time) of the uniform distribution on proper q-colorings of G with the prescribed class sizes.

Formal setting: Markov chain or decoupling-based sampler on the state space Ω = {equitable proper q-colorings of G with class sizes (n_1,…,n_q)}. The chain must respect both the hard constraint (properness, local) and the global conservation constraint (each color used a prescribed number of times). Note the classical Hajnal–Szemerédi theorem guarantees Ω ≠ ∅ for q ≥ Δ+1 in the relaxed "within one" sense, so nonemptiness is not the issue — rapid mixing is.

Ambiguity to resolve before starting: whether "prescribed balanced sizes" means exact counts n_i or the |n_i − n_j| ≤ 1 relaxation; results for one typically transfer with modest loss, but mixing proofs differ.

## Why does it matter?

Equitable colorings are a classical structural topic (Hajnal–Szemerédi) with applications in scheduling, load balancing, and register allocation; a sampler turns existence results into algorithmic ones. Physically, this is sampling from a canonical (fixed-species-count) ensemble rather than the grand canonical ensemble of ordinary coloring samplers — directly relevant to Potts-model simulations at fixed composition. Finally, q = (1+ε)Δ is the conjecturally correct threshold: ordinary coloring sampling is known to be fast there (via spectral independence), so the equitable version tests whether global constraints shift the threshold — a benchmark question for the sampling community.

## What's already known?

- **Ordinary colorings:** Jerrum (1995) proved Glauber dynamics for proper q-colorings mixes in poly(n) for q > 2Δ; Vigoda (1999) improved this to q > (11/6)Δ via block dynamics; Chen–Delcourt–Moitra–Peres–Sun (2019+, spectral independence) settled q ≥ (1+ε)Δ up to logs/linear-in-Δ caveats. So the non-equitable problem is essentially solved.
- **Equitable version:** Kuchukova, Perkins & Povill (2026) (verify details — this is the cited arXiv:2603.08259) give a polynomial-time sampler for q above roughly 2Δ. Their approach presumably runs Glauber-type dynamics restricted to balanced states or couples fixed-size colorings to ordinary colorings; the reason it stalls near 2Δ is that the global constraint breaks the standard path-coupling/spectral-independence arguments, which rely on vertex-independent perturbations.
- **No lower bound / hardness result** is recorded separating equitable from ordinary sampling near q = Δ.

## What would count as solving it?

1. **Upper bound (main target):** an algorithm sampling within TV distance δ in poly(n, 1/ε, log(1/δ)) time for all q ≥ (1+ε)Δ, any graph of max degree Δ, fixed ε. Ideally via proving rapid mixing (mixing time poly(n)) of a natural chain on equitable colorings.
2. **Partial progress:** any improvement of the 2Δ threshold (e.g., q ≥ (1+ε)Δ for large constant Δ, or q ≥ cΔ with c < 2).
3. **Lower bound:** a construction showing some natural local chain mixes exponentially slowly for some q = (1+ε)Δ, or evidence the threshold genuinely shifts.

A referee would accept a fully proved mixing-time bound with explicit polynomial dependence, or a randomized algorithm with a provable TV-distance guarantee.

## How to start

1. Read Jerrum's coupling argument and the spectral-independence machinery for colorings (Chen–Moitra survey path), then read arXiv:2603.08259 to see exactly where their analysis requires q > 2Δ — the gap is your attack surface.
2. Natural approach A: **projection/lifting.** Sample an ordinary coloring (known fast at (1+ε)Δ), then rebalance color classes via a chain that permutes/recolors to hit exact counts; analyze via comparison with the ordinary chain.
3. Natural approach B: **spectral independence with a global constraint.** The pinned field in equitable colorings is a conditioned distribution; try to extend the bound-on-influence field arguments to the conditioned state space, treating the color-count constraint as a hard-core-like global pin.
4. Main obstacle: global conservation invalidates single-site updates' independence structure; swaps that preserve counts are long-range moves whose coupling contracts only for larger q. Block moves or "hole" moves (à la Vigoda) adapted to preserve counts are the natural refinement.

## Key references

- A. Kuchukova, W. Perkins, A. Povill (verify initials), *Sampling Colorings with Fixed Color Class Sizes*, arXiv:2603.08259 (2026). — Gives polynomial-time sampling of colorings with prescribed class sizes for q above ~2Δ; the current state of the art for this exact problem. https://arxiv.org/abs/2603.08259
- M. Jerrum, "A very simple algorithm for estimating the number of k-colorings of a low-degree graph," *Random Structures & Algorithms*, 1995. — Path coupling proving Glauber dynamics mixes for q > 2Δ for ordinary colorings.
- E. Vigoda, "Improved bounds for coloring and sampling k-colorings," *FOCS* 1999. — Block/flip dynamics pushing ordinary-coloring sampling to q > 11Δ/6.
- Z. Chen, A. De, S. Ko, L. Mikulincer, A. Moitra et al. (multiple authors across the spectral-independence line; verify exact attribution), "Spectral independence in high-dimensional expanders..." line of work (2019–2021). — Established sampling of ordinary colorings for q ≥ (1+ε)Δ via spectral independence.
- A. Hajnal, E. Szemerédi, "Proof of a conjecture of Erdős," 1970. — Guarantees existence of equitable colorings for q ≥ Δ+1.

## Warnings

- The census record is sparse; the "2026" paper is described secondhand — verify its exact theorem statement, the precise notion of balancedness (exact counts vs. within-one), and whether the regime is q > 2Δ or q ≥ 2Δ before building on it.
- Don't assume the threshold matches ordinary colorings: global constraints can create exponentially slow bottlenecks invisible in the non-equitable chain.
- Distinguish sampling from counting: FPAUS for this problem need not give an FPRAS directly without self-reducibility adapted to the constraint.
- Common dead end: naive path coupling on count-preserving single-site moves typically fails entirely (single-site moves generally don't preserve class sizes).

---
*`ALGO-000143` · unverified*

## Acceptance guidance

1. **Upper bound (main target):** an algorithm sampling within TV distance δ in poly(n, 1/ε, log(1/δ)) time for all q ≥ (1+ε)Δ, any graph of max degree Δ, fixed ε. Ideally via proving rapid mixing (mixing time poly(n)) of a natural chain on equitable colorings.
2. **Partial progress:** any improvement of the 2Δ threshold (e.g., q ≥ (1+ε)Δ for large constant Δ, or q ≥ cΔ with c < 2).
3. **Lower bound:** a construction showing some natural local chain mixes exponentially slowly for some q = (1+ε)Δ, or evidence the threshold genuinely shifts.

A referee would accept a fully proved mixing-time bound with explicit polynomial dependence, or a randomized algorithm with a provable TV-distance guarantee.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/e9afb792-06d1-41ac-b051-6a82b3c48a0f?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000143.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000143)
