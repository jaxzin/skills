# My answers on the force-push ADR

1. Agents tidying their own branches with --force-with-lease: yes, keep that. They do it all the time.
2. Asking me before each force push: not "friction". The real reason it's out is that I'm often unavailable, and an agent must never sit waiting on me or guess my answer. Fix that.
3. Branch protection: irrelevant to the rule. The rule applies whether or not a repo has protection on.
4. The "Background" section: take it out of the ADR. The story belongs in a lessons note, not here.
5. Does `release/*` count as a shared branch? Honestly not sure yet. Leave that open, don't pick.
