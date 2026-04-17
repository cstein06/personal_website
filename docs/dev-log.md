## Dev Log

### 2026-04-17

- Investigated the HTTPS warning on `carlosstein.com`.
- Confirmed the repository is configured for GitHub Pages with `carlosstein.com` as the custom domain.
- Confirmed DNS is pointing at GitHub Pages:
  - apex `carlosstein.com` resolves to `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
  - `www.carlosstein.com` CNAME resolves to `cstein06.github.io`
- Confirmed the current TLS failure is a certificate mismatch, not a content issue:
  - GitHub is serving a certificate with `CN=*.github.io`
  - hostname validation fails for both `carlosstein.com` and `www.carlosstein.com`
- Queried the GitHub Pages API and found:
  - `cname` is set to `carlosstein.com`
  - `status` is `built`
  - `https_enforced` is `false`
  - `html_url` is still `http://carlosstein.com/`
- Attempted to enable HTTPS through the GitHub API, and GitHub returned: `The certificate does not exist yet`
- Re-saved the Pages custom domain through `gh api` with `{"cname":"carlosstein.com"}` to retrigger GitHub Pages domain validation/provisioning.
- Re-checked the site immediately after the API update:
  - Pages still reports `https_enforced: false`
  - `html_url` still reports `http://carlosstein.com/`
  - TLS still serves `CN=*.github.io`, so hostname validation still fails
- Updated the personal website content to add AI science as an explicit research topic.
  - Added `AI science and scientific discovery` to the main `Research Areas` card grid.
  - Added `Science as a learning system` to the deeper `Research Themes` section.
  - Expanded the research summary copy so agent-supported discovery is part of the top-level framing.
- Current conclusion:
  - the repo contents are not causing the browser warning
  - the blocking issue is that GitHub Pages has not yet provisioned the custom-domain certificate for `carlosstein.com`
- Next action:
  - wait for GitHub Pages certificate provisioning to complete, then enable HTTPS in Pages settings
  - if it does not appear soon, re-save the custom domain in GitHub Pages settings to force revalidation/provisioning
