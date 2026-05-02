## 1. Create configuration documentation page

- [ ] 1.1 Create `content/docs/reference/config.md` with required frontmatter (title: "Configuration", description, weight for sidebar ordering)
- [ ] 1.2 Write introduction section: what `uf config` does, why unified configuration matters
- [ ] 1.3 Write command reference section: `init` (creates/updates `.uf/config.yaml`), `show` (displays merged YAML), `validate` (schema validation) — with usage examples

## 2. Document layered loading

- [ ] 2.1 Write layered loading section: 3-tier precedence diagram (user `~/.config/uf/config.yaml` > repo `.uf/config.yaml` > env vars)
- [ ] 2.2 Include precedence examples showing how repo config overrides user config for specific keys

## 3. Document configuration sections

- [ ] 3.1 Write section overview table: all 7 sections (setup, scaffold, embedding, sandbox, gateway, doctor, workflow) with purpose and key settings
- [ ] 3.2 Write common customization examples as YAML code blocks: package manager (`setup.package_manager: dnf`), embedding model, gateway port (`gateway.port: 9090`), skip list (`setup.skip: [gaze, mxf]`)

## 4. Update common-workflows.md

- [ ] 4.1 Add references to `uf config init` and `uf config validate` commands in the workflow configuration section of `content/docs/getting-started/common-workflows.md`
- [ ] 4.2 Add link to the full configuration docs page

## 5. Verification

- [ ] 5.1 Run `npm run build` and confirm no build errors
- [ ] 5.2 Run `npm run dev` and verify config page renders correctly with all sections
- [ ] 5.3 Verify page appears in Reference section navigation
- [ ] 5.4 Verify both light and dark mode rendering
