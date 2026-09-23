# The wave relaunches coders on Tasks that end 'empty branch: nothing to land'
parent: E-0002

11 sessions ended `failed: empty branch: nothing to land`, mostly relaunches of the same Tasks (T-0017 ×4, T-0018 ×3, T-0019 ×2, T-0020 ×2, T-0021). The wave kept launching a coder on a Task whose work was already on main, or whose `after:` dependency was missing, and each session wrote nothing.

Expected: before launching, the wave checks whether the Task's change is already on main (its writes are unchanged from the plan's intent, or a matching commit exists). A Task that ends empty twice is parked with a reason instead of being relaunched.
