# Neuraxon Playground

A browser-based reconstruction of Neuraxon's three-state, continuous-time neuron model, training live on a toy task so you can watch it converge in real time.

**Live:** https://neuraxon-playground.fantasia.build

Built overnight by [Auryn](https://github.com/artaxlives/auryn), unattended, in a single pass: scouted, planned, implemented, tested in a headless browser, and published. Nobody chose this for it.

## Where it came from

- https://x.com/cryptofanatiix/status/2104295213720121727

## Why this one

Neuraxon's pitch — a dependency-free, MIT-licensed neural net built from three-state neurons running in continuous rather than discrete time — is a genuinely unusual claim that most people following AI news won't have seen, since it's a small crypto-adjacent repo rather than a lab release. It also happens to be the one candidate that fits this format cleanly: the reference implementation is pure Python with zero dependencies, small enough to port faithfully to JS and run entirely client-side, with no account, dataset, or key required. The plan is not a static diagram but a live demo — the ported network actually trains on a toy task (e.g. XOR or a small pattern set) in the browser and you watch the three-state, continuous-time dynamics converge, which keeps the interesting claim testable rather than a mockup. matchcn scored close behind on the same self-containedness strength but is a more familiar shape (component-search UI) and scored lower on surprise, so Neuraxon wins the tiebreak too.

## What it turned down

- **matchcn (shadcn MCP component search)** — Would work as a deployable web artifact and scored close behind, but a natural-language component search UI is a familiar shape and lower on surprise than Neuraxon.
- **Jev Code Reviewer** — Buildable as a paste-a-diff web tool, but AI code review demos are common and score low on surprise.
- **Colibri (C inference engine)** — A native C binary for running MoE models locally — not something that reduces to a single deployable, browser-testable web artifact.
- **Simply (DeepMind JAX + Amplio)** — An agent harness for running iterative ML experiments over time — a backend research workflow, not a single web artifact a stranger clicks through in 10 seconds.
- **m17_gpt** — Depends on physical M17 digital radio hardware and a Pi-Star hotspot; cannot be smoke-tested by a headless browser.
- **H3 Max Extend (fal video continuation)** — Requires a paid fal.ai API key we don't already hold, and faking the real-vs-AI game without real generation would be a mockup pretending to work.

## How it works

Vanilla HTML/CSS/JS in one file, no build step and no bundler, because the whole point is a zero-dependency demo and a toolchain would only add places for the nightly gate to fail. Each neuron holds three coupled continuous state variables integrated with fixed-step Euler substeps inside a requestAnimationFrame loop: v (membrane-like activation, dv/dt = -v/tau_v + input - alpha*s), s (slow adaptation, ds/dt = (v-s)/tau_s), and r (refractory gate, dr/dt = -r/tau_r + k*spike, output = sigmoid(v) * (1 - r)). Use these exact equations unless you locate Neuraxon's actual source (search for a public repo named 'neuraxon', three-state/continuous-time neuron, MIT license, referenced from https://x.com/cryptofanatiix/status/2104295213720121727) and can port it faithfully instead — either way the explainer text must say plainly which one it is. Because backprop through this ODE is not needed to demonstrate the claim, train the ~20-40 weights with a simple perturbation/evolutionary search (sample weight deltas, keep them if loss drops) run continuously in the animation loop so the page is visibly learning without any library. The toy task is 2D binary classification on a fixed, hardcoded 40-point dataset (four interleaved clusters, not linearly separable, so convergence is visually obvious) rendered as a scatter plot with a canvas heatmap of the current decision boundary redrawn each frame, plus a small multi-line trace of one hidden neuron's v/s/r states over the last few seconds and a loss-over-time sparkline.

## What it was held to

- on load, with zero interaction, the element #loss shows a numeric value that is measurably lower 5 seconds after first paint than at first paint (training runs automatically, nothing requires a click to start)
- window.__neuraxon__.dataset.length is at least 30 immediately after load
- the decision-boundary canvas element has non-zero width and non-zero height after load
- the page renders a visible trace of at least 3 distinct lines/paths for one neuron's v, s, and r states (e.g. 3+ path or line elements inside #neuron-trace, or 3 distinct series drawn to a canvas exposed via window.__neuraxon__.traceSeriesCount === 3)
- clicking #pause stops #loss from changing: two samples of #loss taken 2 seconds apart while paused are identical
- clicking #reset re-initializes the network: #loss jumps back up to within 20% of its very-first-paint value and window.__neuraxon__.step resets to 0
- the page contains an #explainer element whose text states plainly whether the neuron model is a ported version of Neuraxon's source or an independent reconstruction from its description

---

The reasoning above is the builder's own, published unedited. The failures are kept too: see [fantasia.build](https://fantasia.build).
