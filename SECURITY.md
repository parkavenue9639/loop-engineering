# Security

Loop Engineering is instruction-only:

- no scripts;
- no package dependencies;
- no bundled MCP servers;
- no network access by itself.

The skill can still influence how an agent plans and delegates work. Review `skills/loop-engineering/SKILL.md` before installing and adapt delegation rules to your local agent permissions.

## Reporting issues

Open a GitHub issue with:

- the exact skill version or commit;
- the agent where it was used;
- the prompt that triggered the issue;
- what goal preview or delegation behavior was unsafe or confusing.

Do not include secrets, private logs, production traces, or credentials in public issues.
