---
description: Manage multiple knowledge repositories (create, switch, list)
---

You are managing knowledge repositories for the doc-copy skill.

Parse the user's command to determine the action:

## Commands

**List all repos:**
- `krepo list` or just `krepo`
- Show all configured knowledge repos and which is currently active

**Create new repo:**
- `krepo create NAME` or `krepo new NAME`
- Create a new knowledge repository with the given name
- Set up the directory structure: `krepos/NAME/docs/imported/{articles,reports,guides,references,other}/`
- Create a `.claude/krepos.json` config file if it doesn't exist
- Add the new repo to the config
- Set it as the active repo
- Create a README for the new repo

**Switch to a repo:**
- `krepo switch NAME` or `krepo use NAME`
- Switch the active knowledge repository to NAME
- Update the config file

**Show current repo:**
- `krepo current`
- Display which knowledge repo is currently active

**Delete a repo:**
- `krepo delete NAME`
- Remove the repo from config (ask for confirmation first)
- Optionally delete the directory

## Configuration Format

Store configuration in `.claude/krepos.json`:
```json
{
  "active": "default",
  "repos": {
    "default": {
      "name": "default",
      "path": "docs/imported",
      "description": "Default knowledge repository",
      "created": "2025-11-16"
    },
    "Cam": {
      "name": "Cam",
      "path": "krepos/Cam/docs/imported",
      "description": "Cam's knowledge repository",
      "created": "2025-11-16"
    }
  }
}
```

## Implementation Steps

When the user runs a krepo command:

1. **Parse the command** to extract action and arguments
2. **Load config** from `.claude/krepos.json` (create if doesn't exist)
3. **Execute the action**:
   - For `create`: Make directories, update config, create README
   - For `switch`: Update active repo in config
   - For `list`: Display all repos with active indicator
   - For `current`: Show active repo
4. **Save config** back to `.claude/krepos.json`
5. **Report results** to the user

## Directory Structure

```
ClaudeSkill/
├── docs/
│   └── imported/          # Default repo
│       ├── articles/
│       ├── reports/
│       ├── guides/
│       ├── references/
│       └── other/
├── krepos/
│   ├── Cam/
│   │   ├── README.md
│   │   └── docs/
│   │       └── imported/
│   │           ├── articles/
│   │           ├── reports/
│   │           ├── guides/
│   │           ├── references/
│   │           └── other/
│   └── Work/
│       ├── README.md
│       └── docs/
│           └── imported/
│               ├── articles/
│               ├── reports/
│               ├── guides/
│               ├── references/
│               └── other/
└── .claude/
    └── krepos.json        # Configuration file
```

## When Creating a New Repo

1. Create directory structure: `krepos/{NAME}/docs/imported/{articles,reports,guides,references,other}/`
2. Create `.gitkeep` files in each category directory
3. Create a README.md for the repo in `krepos/{NAME}/README.md`
4. Update `.claude/krepos.json` with new repo entry
5. Set as active repo
6. Inform user of success

## Integration with doc-copy Skill

After this command completes, the doc-copy skill will automatically use the active repository configured in `.claude/krepos.json`. When invoked via `/kthis`, it will save documents to the currently active repo's path.

Now execute the user's krepo command.
