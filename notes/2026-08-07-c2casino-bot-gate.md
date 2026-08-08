# C2Casino bot-gate

Status: solved and submitted successfully

## Evidence

The C2Casino root page contained this HTML comment:

```text
probe_sequence: C2{b0t_g4t3_cl3ar3d_h4ck3r_d3t3ct3d}
```

## Submission

```text
C2{b0t_g4t3_cl3ar3d_h4ck3r_d3t3ct3d}
```

## Note

The `/api/bcasino/probe` endpoint returned a hex-looking string and a taunting description. Do not assume it is a plaintext flag; the page comment above was the valid submitted flag.

