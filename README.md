# Flam R&D Assignment

## Problem Statement
The assignment requires finding the values of three unknown variables (`theta`, `M`, `X`) from a given set of 2D points (`xy_data.csv`) that are sampled from a parametric curve:

$$ x = t \cos(\theta) - e^{M|t|} \sin(0.3t)\sin(\theta) + X $$
$$ y = 42 + t \sin(\theta) + e^{M|t|} \sin(0.3t)\cos(\theta) $$

Where:
- $0^\circ < \theta < 50^\circ$
- $-0.05 < M < 0.05$
- $0 < X < 100$
- Parameter $t$ ranges from $6 < t < 60$

## Solution Methodology

To find the unknown parameters, we formulate the problem as an optimization task. The equations can be simplified by defining the term representing the displacement (or orthogonal deviation) from the rotated central line:
Let $d = e^{M|t|} \sin(0.3t)$.

We can rearrange the parametric equations to describe a translation by $(X, 42)$ and a rotation by angle $\theta$:
$$ x - X = t \cos(\theta) - d \sin(\theta) $$
$$ y - 42 = t \sin(\theta) + d \cos(\theta) $$

By applying an inverse rotation of $-\theta$ to the translated coordinates, we can isolate $t$ and $d$:
$$ t = (x - X)\cos(\theta) + (y - 42)\sin(\theta) $$
$$ d_{observed} = -(x - X)\sin(\theta) + (y - 42)\cos(\theta) $$

<strong>NOTE</strong>: The calculations are explained in the notebook

### Optimization Process
1. **Data Cleaning**: The dataset (`xy_data.csv`) is read and checked for missing values and duplicates.
2. **Objective Function**: For a given set of parameters $(\theta, M, X)$, we compute $t$ and the observed displacement $d_{observed}$ for all points in the dataset.
3. We then compute the theoretical displacement $d_{predicted} = e^{M|t|} \sin(0.3t)$.
4. The residuals are calculated as $d_{observed} - d_{predicted}$. The objective is to minimize the L1 norm (sum of absolute residuals) over all points.
5. **Global Optimization**: We use `scipy.optimize.differential_evolution` to minimize this objective function globally, constraining the search space within the specified bounds.

## Results
The optimization successfully converges to the following values for the unknown variables:

- **Theta ($\theta$)**: $\approx 30^\circ$ (or $0.523598$ radians)
- **M**: $0.03$
- **X**: $55.0$

*(These values give a near-zero reconstruction error across the dataset.)*

## Example Submission Format (Desmos)
Using the extracted parameters, the parametric curve translates to the following format:
`\left(t*\cos(0.523598)-e^{0.03\left|t\right|}\cdot\sin(0.3t)\sin(0.523598)+55.0, 42+t*\sin(0.523598)+e^{0.03\left|t\right|}\cdot\sin(0.3t)\cos(0.523598)\right)`

