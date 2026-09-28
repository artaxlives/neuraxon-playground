# Changed from the plan

None. Implemented the single-file, zero-dependency HTML/CSS/JS reconstruction option using the specified v/s/r equations, fixed-step Euler integration, and a 2–6–1 network with 25 trainable weights including biases. The page clearly identifies the model as an independent reconstruction, not a source port. No bundler or generated images were needed.

The fixed dataset contains 40 hardcoded points in four interleaved clusters, including points near the class boundary. Training starts automatically using deterministic perturbation proposals; only strictly lower mean squared error is accepted. The decision heatmap redraws each active frame. Three separate SVG state paths, loss history, accuracy, proposal counts, pause/resume, deterministic reset, and three speed settings are implemented.

Implementation choices for details left open by the plan: τᵥ=0.8, τₛ=2, τᵣ=1.2, α=0.35, k=0.12, and Euler step 0.12. A spike is an upward crossing of v=1 represented as a one-step impulse of amplitude 1/Δt. Predictions start from zero states and integrate 32 substeps (3.84 model-time units) for each sample. The live diagnostic neuron instead retains state while cycling through the fixed samples. Reset restores the same initial weights and random seed, clears training history and counters, and preserves the pause and speed settings. Trace history is initialized by running the actual neuron dynamics so all three series are visible immediately.

Verification: opened and executed the page script in a Node DOM/canvas harness. Confirmed initial canvas drawing and three populated state paths, 40 samples available immediately, automatic training, stable loss while paused, exact restoration of the initial loss and step zero on reset, resume, speed changes, and finite extended training. Over 150 normal-speed animation frames, loss fell from 0.2814 to 0.0424 and accuracy reached 95%. Verified the page has no external requests or local asset references; all styles, scripts, data, and graphics are inline. The real visible heading, description, controls, and explanation exist in the initial HTML before JavaScript runs. Runtime simulation errors produce a visible error message and are rethrown.

# Could not do

None. All planned functionality is implemented. True backpropagation, dataset editing, persisted weights, deep architectures, and GPU acceleration remain outside the requested scope.

# Omitted for lack of a service

None. This artifact needs no external service, account, API key, or runtime network access.
