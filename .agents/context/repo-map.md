# Repo map

Quick orientation for agents working **in this checkout**.

```text
.
├── AGENTS.md              # bootloader (this repo)
├── README.md              # human hub
├── contributing/          # maintainer process (development, editing, collation)
├── design/                # goals + ADRs
├── .agents/               # plans, context, optional skills
├── .bootstrap/            # host-tool-check (POSIX, check-only)
├── .config/               # PRJX portable metadata
├── mise.toml, hk.pkl      # tools + hooks/checks
└── content/               # guideline substance
    ├── shared/            # portable
    ├── personal/          # this operator
    ├── operator/          # human sandboxing docs
    └── templates/         # opinionated stacks
```

## Load order (typical)

1. Root `AGENTS.md`
2. Task-relevant tree under `content/` (`shared` default; `personal` / `operator` when applicable)
3. `contributing/development.md` when changing tooling or onboarding the clone
4. `design/` for durable project decisions

## Tooling entrypoints

```sh
sh .bootstrap/host-tool-check
mise install && mise run preload
mise run check    # or: mise run format
```

Details: [contributing/development.md](../../contributing/development.md).
