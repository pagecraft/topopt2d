# topopt2d

A small Python framework for 2D topology optimization. It minimizes compliance
under a volume constraint using Q4 plane-stress finite elements, SIMP material
interpolation, a linear density filter, and the optimality criteria (OC) update.

## Layout

| Module | Purpose |
| --- | --- |
| `topopt/fem.py` | `Problem` dataclass, Q4 element stiffness, assembly, linear solve |
| `topopt/filters.py` | Cone-weighted density filter |
| `topopt/optimizer.py` | Compliance and sensitivities, OC update, `optimize()` driver |
| `topopt/problems.py` | Benchmarks: `mbb_beam`, `cantilever` |
| `tests/` | Unit and integration tests |
| `examples/run_mbb.py` | End-to-end example that saves a density plot |

## Usage

```python
from topopt import cantilever, optimize

problem = cantilever(40, 20)
result = optimize(problem, volfrac=0.4, rmin=1.5, max_iter=100)
print(result.compliance_history[-1], result.converged)
```

## Conventions

* Unit element size and unit thickness. Quantities are dimensionless; scale
  `E0`, loads, and geometry consistently if you need physical units.
* Element `e = ex * nely + ey` (left to right, bottom to top). Node `n = i * (nely + 1) + j`.
  DOFs are `[ux, uy]` at `2n`, `2n + 1`.
* Plane stress only. Void stiffness is `Emin` (default `1e-9 * E0` scale), not zero.
* Public functions have type hints and docstrings; new modules go under `topopt/`
  with matching tests under `tests/`.

## Tests

```bash
pip install -r requirements.txt
python -m pytest        # or: python -m unittest discover -s tests -t .
```

## License

MIT
