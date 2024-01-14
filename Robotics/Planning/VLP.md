# Video Language Planning
## Key Points
- limitations
  - LLM: struggle with grounding, 
  - VLM: struggle to reason over dynamics
- leverage long-horizon abstract planning from LLM/VLM and detailed motions & dynamics from test-to-video-models
- VLM as heuristic scoring fcn + replan
## Model
$\pi_{\text{VLM}}(x, g) \rightarrow a$, $f_{\text{VM}}(x, a)$ <span style="color:red"> (no goal) </span>
##
