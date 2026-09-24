# tick logs stay empty while steps run; a live tick looks the same as a hung one

On 2026-09-24, after install, ~/.ASF/logs/tick-botseon-{tick,batch,daily}.log were still empty after 4 minutes of running (tick and batch were inside factory-cron.sh / factory-batch.sh command steps). The operator can't tell a live tick from a hung one.
Expected: the tick writes a line when each step starts and ends (step, owner, pid, time), flushed at once. The doctor's SCHEDULER row can then show the current step.
