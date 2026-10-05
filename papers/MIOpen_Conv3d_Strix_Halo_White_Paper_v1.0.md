# MIOpen Conv3d Failure on AMD Strix Halo under Windows

## Reproduction, Isolation and a Scoped Workaround

**Nigel Hutchinson / NH Applied**  
**Version 1.0 — 5 October 2026**

**Canonical publication:** https://doi.org/10.5281/zenodo.23158752  
**Upstream issue:** https://github.com/ROCm/TheRock/issues/5845

---

## Abstract

This technical white paper documents investigation of a reproducible MIOpen Conv3d failure on an AMD Ryzen AI MAX+ 395 “Strix Halo” system running Windows 11.

A bounded stage-3 video-VAE workload reproduced a size-dependent failure on the tile-512 MIOpen path, while the corresponding tile-256 control completed successfully. The same tile-512 geometry also completed when the MIOpen/cuDNN-compatible backend path was disabled using `torch.backends.cudnn.enabled = False`, providing evidence that the failure is associated with the MIOpen execution path rather than the tensor geometry alone.

The paper describes the reproduction boundary, isolation methodology, evidence obtained, and a narrowly scoped application-level workaround candidate that can allow affected users to bypass the failing MIOpen path while leaving the wider application unchanged.

The workaround should be considered **experimental rather than a confirmed mitigation**. The ACE evaluation terminated as **INCONCLUSIVE** because repeated validation and numerical-correctness comparison against a declared reference tolerance were not completed. An upstream correction to the affected AMD/MIOpen path remains the preferred resolution.

This work was conducted independently by NH Applied on consumer AMD Strix Halo hardware.

---

## Technical finding

The reduced investigation produced the following bounded result:

- tile-256 control: completed with the MIOpen-backed path enabled;
- matched tile-512 case: terminated on the MIOpen-backed path;
- matched tile-512 geometry with `torch.backends.cudnn.enabled = False`: completed with finite output.

This supports a possible temporary application-level workaround of disabling the affected backend around the relevant VAE/Conv3d operation and restoring it afterwards. It is **not** presented as an upstream fix or as a fully validated mitigation.

---

## Read the full white paper

The archival PDF and DOCX, citation metadata and permanent DOI are hosted on Zenodo:

**https://doi.org/10.5281/zenodo.23158752**

The associated upstream discussion is:

**ROCm/TheRock issue #5845:** https://github.com/ROCm/TheRock/issues/5845

---

## Citation

Hutchinson, Nigel. (2026). *MIOpen Conv3d Failure on AMD Strix Halo under Windows: Reproduction, Isolation and a Scoped Workaround* (Version 1.0). NH Applied. Zenodo. https://doi.org/10.5281/zenodo.23158752

---

Copyright © 2026 Nigel Hutchinson / NH Applied. White-paper licensing is governed by the Zenodo record (CC BY 4.0).
