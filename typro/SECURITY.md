 ```diff      
!                      ________________                     !
!                      \      __      /         __          !
!                       \_____()_____/         /  )         !
!                       '============`        /  /          !
!                        #---\  /---#        /  /           !
!                       (# @\| |/@  #)      /  /            !
!                        \   (_)   /       /  /             !
!                        |\ '---` /|      /  /              !
!                _______/ \\_____// \____/ o_|              !
!               /       \  /     \  /   / o_|               !
!              / |           o|        / o_| \              !
!             /  |  _____     |       / /   \ \             !
!            /   |  |===|    o|      / /\    \ \            !
!           |    |   \@/      |     / /  \    \ \           !
!           |    |___________o|__/----)   \    \/           !
!           |    '              ||  --)    \     |          !
!           |___________________||  --)     \    /          !
!                |           o|   ''''   |   \__/           !
!                |            |          |                  !
!                                                           !
!                  "🚧 DON'T CROSS ME 🚧... !"             !
```

## Secrets and CI/CD Security

- Keep secrets out of git. Use local `.env` only for development.
- Use `.env.example` as the template and never commit real values.
- Store production values in GitHub repository secrets and in Streamlit Cloud Secrets.
- If a secret was committed before, rotate it immediately and remove it from git history.

### Minimum Recommended Secrets Hygiene

1. Remove tracked secret files from index (if already tracked):
	- `git rm --cached typro/utils/.env`
2. Commit the `.gitignore` and `.env.example` updates.
3. Add real secret values in hosted secret managers only.

