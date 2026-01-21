# Manny Manifolds Design Folder — Project Tree

```
Design_Folder/
│
├── canonical/                          # Canonical specifications (source of truth)
│   ├── foundations_v1.1.md            # Irreducible principles + primitives
│   ├── design_document_v1.1.md        # Normative contract, definitions, gates
│   ├── architecture_v1.1.md           # Module/flow spec, motion law, run-modes
│   ├── CANONICAL_INDEX.md             # Authoritative list of canonical docs
│   │
│   ├── validation/                    # Validation framework
│   │   ├── validation_framework.md
│   │   ├── scale_tier_plan.md
│   │   ├── emergence_diagnostics_v0.md
│   │   └── README.md
│   │
│   ├── high_level/                    # North-star references (supporting)
│   │   ├── unified_manifold/          # Unified manifold PDFs + gap analysis
│   │   ├── runtime_protocols/         # Runtime protocol specs (organized by category)
│   │   │   ├── physics_entry/
│   │   │   │   └── anchoring_routing_spec_v1.md
│   │   │   ├── observability/
│   │   │   │   └── jazz_protocol.md
│   │   │   ├── interfaces/
│   │   │   │   └── conversational_layer_delegation_protocol.md
│   │   │   └── lifecycle/
│   │   │       └── training_track_v1.1.md
│   │   ├── background_refs/           # Reference-only materials
│   │   └── training_track_v1.1.md
│   │
│   ├── candidate/                     # Candidate specifications (non-canonical)
│   │   ├── README.md                  # Candidate spec guidelines (promotion requirements)
│   │   ├── physics_candidate_index_v1.1.md  # Physics candidate index & conformance checklist
│   │   ├── gravity/
│   │   │   └── gravity_emergence_spec.md
│   │   ├── lenses/
│   │   │   └── lenses_spec.md
│   │   ├── plasticity/
│   │   │   └── plasticity_learning_physics_spec.md
│   │   ├── promotion_motif_emergence/
│   │   │   └── promotion_motif_stub.md
│   │   ├── traversal/
│   │   │   └── traversal_stub.md
│   │   ├── jazz/
│   │   │   ├── README.md
│   │   │   └── jazz_narration_spec_v1_1.md
│   │   └── training/
│   │       └── README.md
│   │
│   ├── experiment/                    # Experiment playbooks (v0)
│   │   ├── jazz_expermient_playbook_v0.md
│   │   └── gravity_experiment_v0.md
│   │
│   └── archive/                       # Historical versions
│       ├── foundations_v1.0.md
│       ├── design_document_v1.0.md
│       ├── architecture_v1.0.md
│       └── candidate_v1.1/
│
├── implementation/                    # Implementation specifications
│   ├── v1.1/                          # v1.1 milestone specs
│   │   ├── milestone_plan.md
│   │   ├── INDEX.md
│   │   ├── m0_harness_alive.md
│   │   ├── m1_trace_fidelity.md
│   │   ├── m1.5_spine_compression_scaffold.md
│   │   ├── m2_runmodes_virtual_stage_isolation.md
│   │   ├── m3_plasticity_learning_as_curvature.md
│   │   ├── m4_semantic_mass_executive_thermostat.md
│   │   ├── m5_motifs_as_promotion.md
│   │   ├── m6_consolidation_rollback.md
│   │   ├── post_milestones_runtime_spec.md
│   │   └── archive/
│   │       └── jazz_training_high_level.md
│   └── briefs/
│       └── MM_Conversational_Layer_Delegation_Spec.md
│
├── governance/                        # Governance and change control
│   ├── governance_charter.md
│   ├── canonical_freeze_v1.0.md
│   ├── canonical_freeze_v1.1.md
│   ├── doc_classification.md
│   ├── spec_integrity_checklist.md
│   └── BUS_PACKET/                   # Handoff packet
│       ├── 00_README_START_HERE.md
│       ├── 01_CANON_INDEX.md
│       ├── 02_NON_NEGOTIABLES.md
│       ├── 03_MVP_PROOF_PLAN.md
│       ├── 04_RISK_REGISTER.md
│       └── 05_ROADMAP_SAFE_EXPANSIONS.md
│
├── research/                          # Research materials (non-canonical)
│   ├── agi/                          # AGI positioning and research tracks
│   │   └── research_tracks/
│   │       ├── agency/
│   │       ├── goal_formation/
│   │       ├── self_modification/
│   │       └── world_model/
│   ├── current/                      # Current research
│   │   ├── ai movement psychology/
│   │   ├── consolidated_agi_path/
│   │   ├── manny/
│   │   ├── sensation and cognition/
│   │   └── thread_dynamics_and_state_of_mind/
│   ├── architecture_legacy/           # Legacy architecture docs
│   ├── conversational_layer/
│   ├── validation/                   # Legacy validation materials
│   ├── surveys_analysis/
│   ├── frictions/
│   └── exports/
│
├── future/                            # Future work (non-canonical)
│   ├── future_research_map(cannonical).md
│   ├── manny_domain_strategy_product_governance_framework.md
│   ├── candidate/                    # Candidate future work
│   │   ├── bus_package_and_agi_approach.md
│   │   ├── deferred_future_work_register.md
│   │   ├── dimensions_as_first_order_citizens.md
│   │   ├── human_behavior_and_agi.md
│   │   ├── training_manny.md
│   │   └── Feature Proposal — Manny × Notebook Lm Integration & Ux Inspiration.pdf
│   └── reference/                    # Reference materials
│       └── ai_movement_psychology/
│
├── archive/                           # General archive
│   └── README.md
│
├── ongoing_thoughts.md                # Personal ideation notepad
├── README.md                          # Repository overview
├── CHANGELOG.md
├── CONTRIBUTORS.md
├── LICENSE
│
└── Root symlinks (convenience):
    ├── foundations.md → canonical/foundations_v1.1.md
    ├── design_document.md → canonical/design_document_v1.1.md
    └── architecture.md → canonical/architecture_v1.1.md
```

## Key Directories

- **canonical/** — Source of truth for Manny Manifolds specifications
- **implementation/** — Implementation milestone and runtime specifications
- **governance/** — Governance rules, freezes, and change control
- **research/** — Research materials, analysis, and positioning (non-canonical)
- **future/** — Future work and deferred items (non-canonical)
