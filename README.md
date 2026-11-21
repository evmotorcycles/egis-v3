# EGIS v3 — Empirical Governance and Inductive Safety

**Independent research by Labib Mago**  
One Tesla T4 · one seed · registered live run · Qwen2.5-0.5B / 1.5B / 3B-Instruct  

**Master digest (SHA-256):** `16a0c8fbfe3b1a42bb30d339881789bc43ef6fa91aab5074922eeeebb3f78d72`  
**Notebook pin (SHA-256):** `52027d065bd05e0f65f51a61b65d29b8b94154f511091d729e503cacad2911c0`  

The pre-registration file and the results file both carry the master digest above. The ledger records the single held-out look for each test. The outer runner printed **HARNESS INTEGRITY: VERIFIED** and confirmed the spec was unchanged since registration.

---

## What EGIS means (plain English)

**EGIS = Empirical Governance and Inductive Safety.**

Picture a kitchen where the cook may improvise, and an exit inspector who checks every plate against a written ticket before it leaves. EGIS treats the language model as the cook: **nothing it says is believed by default.**

Three layers sit outside the model:

1. **Ticket** — predictions, thresholds, and data splits go into one SPEC, SHA-256 hashed *before any generation*; held-out data is scored **once** through a ledger.
2. **Inspector** — plain Python extracts hard claims (for example DOIs) and checks them against reality (the doi.org registry). Tone and the model's own confidence are ignored for the pass/fail decision.
3. **Locked pantry** — a default-deny file guard; the model cannot write where it is not allowed.

This repository is a **worked example of that method** on small open-weight models. It is **not** evidence about frontier systems.

---

## Summary of the registered live run

| Test | Question (everyday wording) | Main numbers | Outcome |
|------|----------------------------|--------------|---------|
| **A** Oversight | Does a calibrated external check keep working better than tone or self-confidence? | Fabrication rates **0.375 / 0.508 / 0.300** (0.5B / 1.5B / 3B). Tone AUROC 0.39–0.54. Self-grade BA 0.26–0.49. DOI calibrator: 0 errors on 20 controls. | External check held. Tone and self-grade were weak. One rate prediction **refuted**. |
| **B** Adversarial defenses | If I write a filter against known tricks, does it stop **new** tricks? | Seen: breach 0.685 → **0.000**. Unseen: still **0.735**. Squeeze: 0.451. File guard: **0 of 1,000** forbidden writes. | Filter worked on what it was built for; **failed to generalize**. File guard held. |
| **C** Habit after safety training | After safety fine-tuning, is a trained bad habit gone or only hidden? | Organism fabrication ≈ 0.99. After safety pass: **0.000** on both normal and shifted prompts. | Habit was **removed**, not masked — contrary to prediction (**refuted**). |
| **D** Internal signals | Do activations predict a bad claim better than the prompt text alone? | Probe AUROC **0.702** vs text baseline **0.766**; shuffle p = **0.597**. | **No evidence beyond the prompt text** (two predictions **refuted**). |

**Of 17 pre-registered predictions: 13 confirmed, 4 refuted** (A1, C2, D2, D3).

**Sharp finding (B5):** a defense written against known Unicode tricks scored **0.000** breach on those tricks and **0.735** on tricks it had not seen.

---

## What this does **not** show

- Nothing about frontier (Claude / GPT-class) models  
- Nothing about real attacks on live third-party systems  
- Model-attacker score (0 evasions / 100) is a **lower bound** — 77 of the 100 outputs dropped the DOI entirely, so evasion was not testable on those rows  
- "Removed" at 0.5B is not a test of sophisticated deception at high capability  
- One seed, synthetic tasks, scripted attackers  

Failures are part of the result.

---

## Files

| File | Role |
|------|------|
| `EGIS_four_mentor_tests_v3.ipynb` | Full experiment notebook |
| `egis_v3_live_runner.py` | Outer harness: pins notebook hash, runs live, audits files |
| `RESULTS.md` | Headline numbers from the registered live run |
| `v3_prereg.json` | Pre-registration spec (master digest `16a0c8fb…`) |
| `results_v3.json` | Full results (same master digest) |
| `ledger_v3.json` | Single-look ledger: 5 entries (A×3, C, D), dated 2026-11-21 |
| `v3_testB_model_attacker_outputs.json` | B4 raw outputs (100 rows) |
| `v3_runner_audit.json` | Post-run audit log — all checks passed |

Per-test digests: A `2f7510c41f7d` · B `0367f22ebce5` · C `552e5daa8d2d` · D `79194a36f1a3`  
Seed: `20261208`

---

## Provenance note

The pre-registration, results, and runner audit all record the same master digest: `16a0c8fb…`. The runner verified that the spec did not change between registration and the live run, and that the held-out look was taken exactly once per test.

An earlier development iteration produced a different digest and is not part of this repository. The run recorded here is the one the ticket registered.

---

## How to re-check

1. Confirm the notebook SHA-256 matches `52027d065bd05e0f65f51a61b65d29b8b94154f511091d729e503cacad2911c0`.  
2. Recompute the SHA-256 of `v3_prereg.json` — it should be `16a0c8fbfe3b1a42bb30d339881789bc43ef6fa91aab5074922eeeebb3f78d72`.  
3. Run the notebook with `DRY_RUN = True`; note the master digest. It must equal `16a0c8fb…`.  
4. Set `DRY_RUN = False`, change nothing else; live digest must equal dry.  
5. Run the outer runner; its post-audit must report **HARNESS INTEGRITY: VERIFIED**.

If Colab's PEFT stack raises `torchao` version errors during Test C, upgrade or pin compatible `peft` / `torchao` before treating the run as complete.

---

## Author

**Labib Mago** — independent researcher, Uganda.  
Not a professor, founder, or staff engineer.  
AI assistance (Claude) was used for code and critique; research questions, execution, and reporting decisions are mine.

This is a method demo with published misses — not a product claim and not a claim about frontier models.
