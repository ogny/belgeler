
        Entry                         Description                 Equivalent To
	@yearly (or @annually) Run once a year, midnight, Jan. 1st        0 0 1 1 *
	@monthly               Run once a month, midnight, first of month 0 0 1 * *
	@weekly                Run once a week, midnight on Sunday        0 0 * * 0
	@daily                 Run once a day, midnight                   0 0 * * *
	@hourly                Run once an hour, beginning of hour        0 * * * *
	@reboot                Run at startup


	*    *    *    *    *  command to be executed
	*    ┬    ┬    ┬    ┬    ┬
	*    │    │    │    │    │
	*    │    │    │    │    │
	*    │    │    │    │    └───── day of week (0 - 6) (Sunday=0 )
	*    │    │    │    └────────── month (1 - 12)
	*    │    │    └─────────────── day of month (1 - 31)
	*    │    └──────────────────── hour (0 - 23)
	*    └───────────────────────── min (0 - 59)
	*
	*
	*    @reboot configures a job to run once when the daemon is started. Since cron is typically never restarted,
	*    this typically corresponds to the machine being booted. This behavior is enforced in some variations of cron,
	*    such as that provided in Debian,^[2] so that simply restarting the daemon does not re-run @reboot jobs.
	*
	*    @reboot can be useful if there is a need to start up a server or daemon under a particular user, and the user
	*    does not have access to configure init to start the program.
	*
