# CGSCMLI_Topology

Companion topology database, benchmark statistics, supplementary switching-state tables, and interactive exploration notebooks for the paper:

> **Requirement-to-Circuit Reverse Derivation of Common-Ground Switched-Capacitor Multilevel Inverters With Stress-Optimized Semiconductor Synthesis**  
> Zhifeng Dou, Haoran Liu, Falong Lu, Yongpeng Shen, and Qian Wang  
> College of Electrical and Information Engineering, Zhengzhou University of Light Industry

> [!IMPORTANT]
> **Pre-Publication Notice (Manuscript Under Peer Review):** The accompanying paper is currently under peer review. Should this manuscript be accepted for publication, the complete derived topology datasets, switching-state tables, semiconductor mappings, and interactive exploration notebooks will be publicly released in this GitHub repository in accordance with the descriptions in the paper.

The complete derivation methodology—including SMT capacitor-configuration solving, KVL charging-loop search, switch-path optimization, and semiconductor-mapping synthesis—is detailed in the paper. As outlined below, this repository is structured to host the resulting **Generalized Topologies (GTs), Power Electronic Circuits (PECs), switching states, semiconductor mappings, individual and total blocking voltage stresses, benchmark statistics, and interactive exploration notebooks**.

---

## Repository Structure

```text
CGSCMLI_Topology/
├── README.md                              # Overview, directory map, quick start
├── requirements.txt                       # Python dependencies for the interactive notebooks
├── docs/
│   ├── naming_and_encoding.md             # Guide to folder codes, topo_uid, branch_device_code (0/D/U/R/B), GenSkel
│   ├── json_schema.md                     # Schema reference for individual topology JSON & CSV catalogs
│   └── dataset_catalog.md                 # Detailed comparison of all 7 datasets & paper cross-references
├── notebooks/
│   ├── Topology_Explorer.ipynb            # Interactive topology browser, multi-metric sorter & table inspector
│   └── Literature_Strategy_Studio.ipynb   # Interactive viewer for the 58 literature & baseline strategies
├── strategies/
│   ├── literature_strategies.json         # Canonical registry of 58 reference configurations
│   └── strategy_precomputed_views.json    # Precomputed path/KVL views for fast interactive browsing
├── supplementary_tables/
│   ├── README.md                          # Documentation for omitted wide-gain switching matrix
│   ├── wide_gain_7L_switching_states.pdf  # Standalone supplementary PDF (circuit schematic, waveform & full Table I)
│   ├── wide_gain_7L_switching_states.csv  # Wide-input 7-level CGSCMLI switching states (15x, 12x, 9x, 6x, 3x; 40 switches)
│   └── wide_gain_7L_switching_states.json # Structured JSON representation of the wide-gain matrix
└── datasets/
    ├── 01_13L_baseline_sup18/             # 13L reference strategy case study (96 PECs / 38 GTs, M1/M2 lineage)
    ├── 02_13L_opt_scheme_sup16/           # 13L optimized output-scheme case study (8 PECs, min 13 SPs)
    ├── 03_cross_level_stochastic_pruning/ # Main 5L-13L benchmark via stochastic pruning (20,498 PECs / 2,443 GTs)
    ├── 04_cross_level_rl_ppo/             # Comparison 5L-13L benchmark via MaskablePPO RL (17,793 PECs)
    ├── 05_13L_fixed_charge_1944_schemes/  # 13L exhaustive 1,944 output schemes under reference charging (7,527 PECs)
    ├── 06_13L_opt_charge_schemes/         # 13L output-scheme traversal under optimized charging (2,384 PECs)
    └── 07_cross_level_exploratory/        # Exploratory multi-level benchmark & comparison figures (3,918 PECs)
```

---

## Dataset Overview

| ID   | Dataset Folder                                               | Scope                                    | Unique PECs | Key Highlights & Paper Reference                             |
| ---- | ------------------------------------------------------------ | ---------------------------------------- | ----------: | ------------------------------------------------------------ |
| 01   | [`datasets/01_13L_baseline_sup18`](datasets/01_13L_baseline_sup18) | 13L, 3C `[1,2,4]`, G=6, SupSP=18         |          96 | Contains **38 GTs** and **2 minimum 12-SP parent topologies** (`M1`: `5EE62B9F`, `Vmax=6Vdc`, `TSV=42Vdc` — prototype in Sec. III; `M2`: `A0E0A331`, `Vmax=7Vdc`, `TSV=43Vdc`) plus lineage classification |
| 02   | [`datasets/02_13L_opt_scheme_sup16`](datasets/02_13L_opt_scheme_sup16) | 13L, 3C `[1,2,4]`, G=6, SupSP=16         |           8 | Same reference charging (`Chg2121312131211`) with optimized output scheme (`Sch1713193219142`); reduces superstructure from 18 to 16 SPs (min 13 SPs) |
| 03   | [`datasets/03_cross_level_stochastic_pruning`](datasets/03_cross_level_stochastic_pruning) | 5L–13L, 153 combinations × 5 seeds       |      20,498 | **Main benchmark (Non-RL stochastic pruning)**: **2,443 GTs**, **20,498 PECs**, `2,940.1 s` total 3-stage runtime (Table I, Sec. IV-A) |
| 04   | [`datasets/04_cross_level_rl_ppo`](datasets/04_cross_level_rl_ppo) | 5L–13L, 153 combinations × 5 seeds       |      17,793 | **RL comparison (MaskablePPO reinforcement-learning pruning)**: `23,686.3 s` total runtime (Sec. IV-A) |
| 05   | [`datasets/05_13L_fixed_charge_1944_schemes`](datasets/05_13L_fixed_charge_1944_schemes) | 13L, all 1,944 output schemes            |       7,527 | Includes **3 highlight 12-SP PECs** (2 GTs), including the 16-device realization (`SP12_A3F24E41`: `4B/1R/6U/1D`, `H = 16`, `TSV = 47 Vdc`, `Vmax = 6 Vdc`) discussed in Sec. III-A |
| 06   | [`datasets/06_13L_opt_charge_schemes`](datasets/06_13L_opt_charge_schemes) | 13L, `Chg1231312123121` scheme traversal |       2,384 | Includes **321 AutoMin highlights** and the 18-SP alternative circuit (`SP18_E8EBC1F3`) used for ripple comparison in Sec. IV-B |
| 07   | [`datasets/07_cross_level_exploratory`](datasets/07_cross_level_exploratory) | 5L–13L exploratory (180 combinations)    |       3,918 | Weak-prior exploratory search covering broader capacitor/gain configurations (e.g., 11L `4C:[1-2-4-8]` at gains 5/10/15) & comparison figures |

---

## Quick Start

### 1. Install Lightweight Dependencies

```bash
pip install -r requirements.txt
```

### 2. Launch the Interactive Notebooks

```bash
jupyter lab
```

- Open [`notebooks/Topology_Explorer.ipynb`](notebooks/Topology_Explorer.ipynb) to filter, rank, and inspect any derived topology's switching table, per-SP semiconductor realization, blocking voltage stresses, and symbolic adjacency matrices.
- Open [`notebooks/Literature_Strategy_Studio.ipynb`](notebooks/Literature_Strategy_Studio.ipynb) to browse all 58 literature and baseline strategies with level-by-level circuit flow diagrams and KVL charging summaries.

### 3. Understanding Naming & File Formats

- Every topology is saved as an independent, self-contained `.json` file (sharded into subdirectories of `<= 500` files for smooth GitHub web browsing and path-length safety).
- See [`docs/naming_and_encoding.md`](docs/naming_and_encoding.md) for a complete breakdown of configuration codes such as `Pruning_2C_V1-1_5L_G2_SupSP12_Chg2121X_Sch21221`, `topo_uid` (`SP12_5EE62B9F_UB0D0UUUUUBD00U0U0`), and semiconductor class codes (`0`, `D`, `U`, `R`, `B`).
- See [`docs/json_schema.md`](docs/json_schema.md) for the exact schema of topology JSON objects and summary CSV tables.
- See [`supplementary_tables/README.md`](supplementary_tables/README.md) and [`supplementary_tables/wide_gain_7L_switching_states.pdf`](supplementary_tables/wide_gain_7L_switching_states.pdf) for the wide-input multi-gain 7-level circuit schematic, simulation waveform, and full switching matrix (`15x`, `12x`, `9x`, `6x`, `3x`).
